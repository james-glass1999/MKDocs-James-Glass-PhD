# Technical Portfolio & Documentation Site  
A professional MkDocs-powered portfolio showcasing technical writing, developer enablement, API documentation, and engineering work.

Welcome to the source repository for **my public portfolio website**, built using **MkDocs**.  
The site brings together my work as a **Senior Technical Writer**, **Developer Enablement Specialist**, and **AI-Driven Documentation advocate**. It serves as a central home for my experience, writing, coding projects, research, and examples of documentation systems in practice.

**Live Website:**  
https://thewritingtimes.blog

---

## What This Site Contains

The website is structured as a full documentation system, featuring:

### **🏠 Home**
An overview of my background, philosophy, and professional pillars:
- Technical Writing & Docs-as-Code  
- Developer Relations & Enablement  
- Knowledge Management & Documentation Strategy  

### **💼 Experience**
My professional journey across developer enablement, documentation leadership, engineering, teaching, and research.

### **📚 Training & Development**
Professional development, completed courses, and learning tracks.

### **🎓 Education**
Academic background including my PhD, Master’s degrees, and undergraduate studies.

### **🛠 Examples of Work**
Selected technical writing samples, engineering outputs, and documentation projects.

### **📡 API Example**
A complete API documentation project, including:
- `openapi.yaml`  
- Express.js implementation  
- cURL examples  
- Project structure  
- Code samples & screenshots  

### **📐 Diagrams**
Architecture diagrams, workflows, systems thinking models, documentation strategy layouts, and visual explanations.

### **📊 Dashboard**
Interactive examples of dashboard design and analytics views.

### **📜 References**
Notes, quotes, and reference material that inform my work.

---

## Repository Structure

```plaintext
docs/
  api-example/
  assets/
  courses/
  dashboard/
  education/
  examples/
  references/
  about.md
  diagrams.md
  experience.md
  index.md
mkdocs.yml
CNAME
README.md

```

All content is written in Markdown and compiled using MkDocs into a clean, searchable documentation site.

---

## Working With the Site Locally

###  Install MkDocs Material (optional)
```bash
pip install mkdocs-material
```

### Run the local development server
```bash
mkdocs serve
```

### Site avialble
```bash
http://127.0.0.1:8000
```

## Custom Domain Setup (Optional)

This website is deployed using **GitHub Pages** and configured to use a custom domain: thewritingtimes.blog

### `CNAME`
To use your own domain instead of the default GitHub Pages URL, follow these steps:

1. **Create a `CNAME` file** in the root of the repository  
The file should contain only your domain name, for example: thewritingtimes.blog


2. **Update DNS settings** with your domain provider  
Add the following DNS records:
- **A records** → point to GitHub Pages IPs:  
  ```
  185.199.108.153
  185.199.109.153
  185.199.110.153
  185.199.111.153
  ```
- **CNAME record** (if using a subdomain) → point to:  
  ```
  james-glass1999.github.io
  ```

3. **Enable GitHub Pages** in the repository settings  
- Go to Settings → Pages
- Select main as the source
- GitHub automatically detects and applies the CNAME file

Once this is set up, your MkDocs site will deploy correctly to your custom domain.

## Deploying With GitHub Pages
This site is deployed via GitHub Pages.

**To deploy:**
- Push changes to the default branch (e.g., main)
- GitHub Pages automatically builds and deploys the site
- MkDocs generates static HTML files that GitHub serves publicly.




