---
layout: default
title: Online Order API

# 🛒 Online Order API

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Node.js](https://img.shields.io/badge/Node.js-18.x-brightgreen?logo=node.js)
![Express](https://img.shields.io/badge/Express.js-Backend-blue?logo=express)
![Swagger](https://img.shields.io/badge/Swagger-OpenAPI%203.0-85EA2D?logo=swagger)
![Status](https://img.shields.io/badge/Status-Active-success)
![Last Commit](https://img.shields.io/github/last-commit/james-glass1999/online-order-api)
![Repo Size](https://img.shields.io/github/repo-size/james-glass1999/online-order-api)
![Stars](https://img.shields.io/github/stars/james-glass1999/online-order-api?style=social)

A simple CRUD API built using **Node.js**, **Express**, and documented with **Swagger / OpenAPI 3.0**.  
This project demonstrates designing an API specification *first* with Swagger, then implementing the backend server that follows it.

---


# 🛒 Online Order API

A simple REST API built with **Node.js** and **Express**, documented using **OpenAPI (Swagger)**.  
This project manages online orders and demonstrates a full CRUD API from design to implementation.

---

## 📘 Project Overview

This API allows you to:

- Retrieve all orders (`GET /orders`)
- Add a new order (`POST /neworder`)
- Update an order’s status (`PUT /update/{id}`)
- Delete an order (`DELETE /delete/{id}`)

Data is stored in a simple JSON file (`orders.json`), making it easy to understand without needing a database.

---

## 🚀 Features

- Node.js + Express backend  
- Full OpenAPI/Swagger specification (`openapi.yaml`)  
- Example request bodies and responses  
- CRUD operations on order data  
- Tested with `curl` commands  
- Hosted on GitHub as an example project  

---

## 🧰 Tech Stack

- **Node.js**
- **Express.js**
- **Swagger / OpenAPI 3.0**
- **JSON file-based storage**

---

## 📡 API Endpoints

| Method | Endpoint        | Description             |
|--------|-----------------|-------------------------|
| GET    | `/orders`       | Get all orders          |
| POST   | `/neworder`     | Create a new order      |
| PUT    | `/update/{id}`  | Update an order’s state |
| DELETE | `/delete/{id}`  | Delete an order by ID   |

---

## 🧪 Example Code (server.js)

```js
// GET all orders
server.get('/orders', (req, res) => {
  res.json(orderData);
});

// POST new order
server.post('/neworder', express.json(), (req, res) => {
  orderData.orders.push(req.body);
  fs.writeFileSync('orders.json', JSON.stringify(orderData));
  res.send('Success');
});
