!!! note ""
    Good API documentation accelerates developer onboarding and reduces the burden on support teams. The examples in this section demonstrate a design-first approach to API documentation — beginning with a formal OpenAPI specification before implementation, ensuring that the contract between the API and its consumers is clearly defined from the outset. Each example showcases structured endpoint documentation, request and response schemas, and practical cURL references intended to serve developers at the point of need.

# Pulse API Reference

**Base URL:** `https://api.pulse.io/v2`  
**Version:** 2.1 · REST

---

## Authentication

All API requests must be authenticated using a Bearer token passed in the `Authorization` header. Keys are scoped to your workspace and carry the permissions of the user who generated them.

```http
Authorization: Bearer <your_api_key>
```

> **Keep your API key out of version control.** Use environment variables or a secrets manager to inject it at runtime.

### Rate limits

| Tier       | Requests per minute |
|------------|---------------------|
| Free       | 60                  |
| Pro        | 1,000               |
| Enterprise | Custom — contact sales |

---

## Errors

All errors follow a consistent shape. The `code` field is machine-readable; `message` is human-readable and safe to display in your UI.

```json
{
  "error": {
    "code": "validation_failed",
    "message": "email is already in use",
    "field": "email",
    "request_id": "req_01HX9..."
  }
}
```

### Error codes

| Code               | Status | Description                                                             |
|--------------------|--------|-------------------------------------------------------------------------|
| `unauthorized`     | 401    | Missing or invalid API key.                                             |
| `forbidden`        | 403    | Valid key, but insufficient scope.                                      |
| `not_found`        | 404    | Resource does not exist.                                                |
| `validation_failed`| 400    | One or more fields failed validation. Check the `field` property.       |
| `rate_limited`     | 429    | Retry after the value in the `Retry-After` response header.             |
| `server_error`     | 500    | Something went wrong on our end. We've been alerted.                   |

---

## Users

### List users

```
GET /v2/users
```

Returns a paginated list of users in your workspace, sorted by creation date descending. Use the `cursor` parameter to page through results.

#### Query parameters

| Parameter | Type    | Required | Description                                                               |
|-----------|---------|----------|---------------------------------------------------------------------------|
| `limit`   | integer | optional | Number of results per page. Min 1, max 100. Defaults to 20.              |
| `cursor`  | string  | optional | Opaque pagination cursor from a previous response's `next_cursor` field.  |
| `status`  | string  | optional | Filter by status: `active`, `inactive`, or `pending`.                    |
| `q`       | string  | optional | Full-text search across name and email fields.                            |

#### Example request

```bash
curl https://api.pulse.io/v2/users?limit=10&status=active \
  -H "Authorization: Bearer $API_KEY"
```

#### Response `200 OK`

```json
{
  "data": [
    {
      "id": "usr_01HX9ABC",
      "email": "alice@acme.com",
      "name": "Alice Chen",
      "role": "admin",
      "status": "active",
      "created_at": "2024-03-01T12:00:00Z"
    }
  ],
  "next_cursor": "cursor_abc123",
  "has_more": true
}
```

---

### Create a user

```
POST /v2/users
```

Creates a new user in your workspace and sends them an invitation email. The invitation expires after 72 hours.

#### Body parameters

| Parameter  | Type    | Required | Description                                                              |
|------------|---------|----------|--------------------------------------------------------------------------|
| `email`    | string  | required | Must be a valid email address and unique within the workspace.           |
| `role`     | string  | required | One of `admin`, `editor`, or `viewer`.                                   |
| `name`     | string  | optional | Display name shown in the UI. Max 120 characters.                        |
| `metadata` | object  | optional | Flat key-value store for custom attributes. All values must be strings. Max 50 keys. |

#### Example request

```bash
curl -X POST https://api.pulse.io/v2/users \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "bob@acme.com",
    "role": "editor",
    "name": "Bob Kim"
  }'
```

#### Response `201 Created`

```json
{
  "id": "usr_01HX9XYZ",
  "email": "bob@acme.com",
  "name": "Bob Kim",
  "role": "editor",
  "status": "pending",
  "invited_at": "2024-11-14T09:00:00Z"
}
```

#### Response `400 Bad Request`

```json
{
  "error": {
    "code": "validation_failed",
    "message": "email is already in use",
    "field": "email"
  }
}
```

---

### Update a user

```
PATCH /v2/users/{id}
```

Updates one or more fields on an existing user. Only the fields you include are modified — omitted fields are left unchanged.

#### Path parameters

| Parameter | Type   | Required | Description                                                   |
|-----------|--------|----------|---------------------------------------------------------------|
| `id`      | string | required | The user's unique identifier, prefixed with `usr_`.           |

#### Body parameters

| Parameter  | Type   | Required | Description                                                                 |
|------------|--------|----------|-----------------------------------------------------------------------------|
| `role`     | string | optional | Change the user's role. Cannot demote the last admin in a workspace.        |
| `name`     | string | optional | Updated display name. Max 120 characters.                                   |
| `metadata` | object | optional | Merges into existing metadata. To remove a key, pass `null` as its value.  |

#### Example request

```bash
curl -X PATCH https://api.pulse.io/v2/users/usr_01HX9ABC \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"role": "admin", "metadata": {"team": "design"}}'
```

#### Response `200 OK`

```json
{
  "id": "usr_01HX9ABC",
  "role": "admin",
  "metadata": { "team": "design" },
  "updated_at": "2024-11-14T10:30:00Z"
}
```

---

### Delete a user

```
DELETE /v2/users/{id}
```

Permanently deletes a user and immediately revokes all active sessions. **This action cannot be undone.**

> **Note:** You cannot delete the last admin in a workspace. Reassign the admin role to another user before proceeding.

#### Path parameters

| Parameter | Type   | Required | Description                                              |
|-----------|--------|----------|----------------------------------------------------------|
| `id`      | string | required | The user's unique identifier, prefixed with `usr_`.      |

#### Example request

```bash
curl -X DELETE https://api.pulse.io/v2/users/usr_01HX9ABC \
  -H "Authorization: Bearer $API_KEY"
```

#### Response `204 No Content`

No response body is returned on success.

#### Response `403 Forbidden`

```json
{
  "error": {
    "code": "forbidden",
    "message": "cannot delete the last admin"
  }
}
```

---

## Messages

### Send a message

```
POST /v2/messages
```

Sends a message to one or more recipients. Messages are delivered asynchronously — use the returned `message_id` to poll for delivery status.

#### Body parameters

| Parameter   | Type     | Required | Description                                                              |
|-------------|----------|----------|--------------------------------------------------------------------------|
| `to`        | string[] | required | Array of user IDs or email addresses. Max 50 recipients.                 |
| `subject`   | string   | required | Message subject line. Max 255 characters.                                |
| `body`      | string   | required | Message body. Supports Markdown. Max 10,000 characters.                  |
| `thread_id` | string   | optional | Attach to an existing thread. Omit to start a new one.                   |
| `send_at`   | string   | optional | ISO 8601 timestamp to schedule delivery. Must be within 30 days.         |

#### Example request

```bash
curl -X POST https://api.pulse.io/v2/messages \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "to": ["usr_01HX9ABC"],
    "subject": "Welcome aboard",
    "body": "Hi Alice, glad to have you."
  }'
```

#### Response `202 Accepted`

```json
{
  "message_id": "msg_01HX9DEF",
  "status": "queued",
  "recipient_count": 1
}
```

---

### List threads

```
GET /v2/threads
```

Returns all message threads the authenticated user is a participant in, ordered by most recent activity.

#### Query parameters

| Parameter | Type    | Required | Description                                                    |
|-----------|---------|----------|----------------------------------------------------------------|
| `limit`   | integer | optional | Results per page. Max 50. Defaults to 25.                      |
| `cursor`  | string  | optional | Pagination cursor from a previous response.                    |
| `unread`  | boolean | optional | When `true`, returns only threads with unread messages.        |

#### Example request

```bash
curl https://api.pulse.io/v2/threads?unread=true \
  -H "Authorization: Bearer $API_KEY"
```

#### Response `200 OK`

```json
{
  "data": [
    {
      "id": "thr_01HX9GHI",
      "subject": "Welcome aboard",
      "unread_count": 2,
      "last_message_at": "2024-11-14T10:00:00Z"
    }
  ],
  "has_more": false
}
```
