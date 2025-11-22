## 📄 Content Reviewer Workflow
### 🔍 Purpose

The Content Reviewer workflow analyzes any provided article, identifies issues, and returns only what must be changed—matching DealNews editorial standards. It is ideal for automated quality checks, SEO alignment, tone correction, and factual cleanup.


### 🚀 Key Capabilities

Detects grammar, clarity, tone, and formatting issues

Flags SEO issues such as weak headers or missing keywords

Identifies outdated deal info, pricing, or seasonal context

Returns only the sections needing revision

Produces clean, minimal output for workflow pipelines

Designed to plug into your existing Dify content automation system

### 📥 Inputs

Raw article text or HTML

Optional target keywords

Optional style/tone constraints

### 📤 Outputs

A structured list of issues

Recommended rewrites

No unnecessary markup or added commentary

## 📄 Content Reviewer & Updater Workflow
### 🔧 Purpose

This workflow reviews, fixes, and rewrites content automatically. It is a full corrective system: review → rewrite → return clean updated content.

### 🚀 Key Capabilities

Performs full article review

Automatically rewrites weak sections

Updates outdated info, pricing, or deals

Enhances formatting, tone, and SEO

Integrates ngrams + style rules

Maintains your strict “only return changed content” structure

Outputs final polished article text, ready for publishing

### 📥 Inputs

Original article text

SEO ngrams

Style guidelines

Optional DealNews formatting rules

### 📤 Outputs

Fully updated article

Inline applied improvements

Preserved formatting and section structure

## 📂 Recommended Repo Structure (Updated)

You should place both YAML files inside a new workflows/content-reviewing/ folder:

/
├── workflows/
│   ├── streaming/
│   │   ├── Promo Code Automation.yml
│   │   ├── Get Webpage Content.yml
│   │   ├── Collect Summarized Web Research.yml
│   │
│   ├── content-reviewing/
│   │   ├── Content Reviewer.yml
│   │   ├── Content Reviewer & Updater.yml
│   │   └── README-content-reviewing.md
│
└── README.md

## 🐳 Docker Hosting Setup

This repo includes a full Dify Docker stack:

- nginx reverse proxy  
- Certbot (SSL auto-renew)  
- Couchbase server  
- SSRF proxy  
- Postgres + Redis  
- Unified `.env` configuration  

---

## 📦 Key Improvements in This Docker Setup
✔ Certbot Container

Automated Let’s Encrypt certificate management.
See: certbot/README.md

✔ Unified .env

One file controls:

Domain + URLs

Database configuration

Redis credentials

Storage provider settings

Vector DB selection

CORS + proxy configuration

## ✔ Vector Database Selectable

VECTOR_STORE=weaviate

Supports Weaviate, Milvus, Qdrant, OpenSearch, etc.


## 📄 License

MIT — See LICENSE for details.

## 📝 Notes on .env

Common URLs

CONSOLE_API_URL

SERVICE_API_URL

APP_WEB_URL

FILES_URL

Database

DB_USERNAME

DB_PASSWORD

DB_HOST, DB_PORT

Redis

REDIS_PASSWORD

Storage

STORAGE_TYPE=local|s3|azure-blob|google-storage|...

Vector DB

VECTOR_STORE

WEAVIATE_ENDPOINT

MILVUS_URI

---

# Full Installation Guide (Dify Workflows + Docker Hosting)

## 1. Install Docker & Docker Compose
If Docker is not installed, run:

sudo apt update
sudo apt install docker.io docker-compose-plugin -y

Enable Docker:

sudo systemctl enable docker
sudo systemctl start docker

## 2. Clone the Repository

git clone https://github.com/j-zulick/dify.ai-content-automation.git

cd dify.ai-content-automation

## 3. Create and Configure Your `.env` File
Copy the example:

cp .env.example .env

Edit `.env` and set:

### Required URLs
- `CONSOLE_API_URL=https://yourdomain.com/console/api`
- `CONSOLE_WEB_URL=https://yourdomain.com/console`
- `SERVICE_API_URL=https://yourdomain.com/v1`
- `APP_API_URL=https://yourdomain.com/api`
- `APP_WEB_URL=https://yourdomain.com`
- `FILES_URL=https://yourdomain.com/files`

### Database
- `DB_USERNAME=postgres`
- `DB_PASSWORD=yourpassword`
- `DB_HOST=db`
- `DB_PORT=5432`

### Redis
- `REDIS_PASSWORD=yourpassword`

### Vector DB
Choose one:

VECTOR_STORE=weaviate

or 

VECTOR_STORE=milvus

## 4. Start Dify Stack
Run all services:

docker compose up -d

Check logs:

docker compose logs -f

## 5. (Optional) Start Middleware Stack
If needed:

docker compose -f docker-compose.middleware.yaml up -d

## 6. Configure SSL (Certbot)
If using your domain with HTTPS:

1. Place SSL certificates in:

nginx/ssl/dify.crt
nginx/ssl/dify.key

2. Set in `.env`:

NGINX_HTTPS_ENABLED=true

Restart:

docker compose down
docker compose up -d

## 7. Access Dify
Open:

https://yourdomain.com

## 8. Use the Workflows in Dify

1. Upload all `.yml` files in `workflows/`
2. Mark each workflow as **Available as Tool**
3. Configure search (`search_get`)
4. Connect Knowledge Base
5. Run **Streaming Service Promo Code Automation**

## 9. Updating the Server
To update:

git pull
docker compose down
docker compose up --build -d

## 10. Troubleshooting

### Check Services

docker compose ps

### Restart Everything

docker compose down
docker compose up -d

### Check specific container logs

docker compose logs api
docker compose logs web
docker compose logs nginx




