# A Smart Document Search Platform

A Django-based platform with PostgreSQL database including various AWS services, containerized using Docker.
The frontend client app is built using Next.js/React.

## Features

- **Django Backend**: Robust web framework for Python
- **PostgreSQL**: Relational database management system
- **Dockerized Environment**: Containerized services with Docker Compose
- **Custom User Model**: Extensible authentication system
- **Out of the box authentication and access control**: via Cognito User Pool

## Prerequisites

- Docker 20.10+
- Docker Compose 2.0+
- Python 3.9+ (for local development)
- AWS CLI (Optional)
- Node.js

## Getting Started

### 1. Clone Repository
```bash
git clone https://github.com/fazila-rahman/rag-based-doc-search.git
cd rag-based-doc-search

```

### 2. Environment Setup For The Backend (Django Server)
 - Create .env file in the backend directory
 - Edit the .env with your credentials as follows:
```bash
# Database credentials
DB_NAME=your_db_name
DB_USER=your_db_user
DB_PASSWORD=your_secret_password
DB_HOST=postgres
DB_PORT=5432

# AWS credentials and region
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_REGION=us-east-1

# Cognito
AWS_COGNITO_USER_POOL_ID=your-cognito-user-pool-id # AWS Cognito User Pool ID
AWS_COGNITO_APP_CLIENT_ID=your-cognito-app-client-id # AWS Cognito App Client ID

# S3
AWS_STORAGE_BUCKET_NAME=your-bucket

# DynamoDB
AWS_DYNAMODB_TABLE_NAME=your-table

# SQS
AWS_SQS_QUEUE_NAME=your-queue

# API Gateway
AWS_API_GATEWAY_REST_API_ID=your-real-api-id
AWS_API_GATEWAY_STAGE_NAME=dev

# CDN
AWS_CDN_BASE_URL=https://dfwe7vqgoi8l9.cloudfront.net

# CELERY
CELERY_BROKER_URL=redis://redis:6379/0
CELERY_RESULT_BACKEND=redis://redis:6379/0

# Pinecone Settings
PINECONE_API_KEY=your-pinecone-api-key
PINECONE_ENVIRONMENT=your-pinecone-environment
PINECONE_INDEX_NAME=pdf-embeddings-index

```

### 3. Environment Setup For The Frontend (Next.js/React)
 - Create .env.local file in the frontend directory
 - Edit the .env.local with your credentials as follows:
```bash

NEXT_PUBLIC_COGNITO_USER_POOL_ID=your-user-pool-id # AWS Cognito User Pool ID
NEXT_PUBLIC_COGNITO_CLIENT_ID=your-client-id # AWS Cognito App Client ID
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000 # Django Backend Server

```

### 4.  Build & Start Containers
```bash
docker-compose up --build
```
Following Key Service containers will be up and running 
 - 1. PostgreSQL: postgres:5432 (container)
 - 2. backend: 127.0.0.1:8000 (container)
 - 3. frontend: 127.0.0.1:3000 (container)


### 5.  Create Superuser
```bash
docker-compose exec backend python manage.py createsuperuser
```

### 6. Build and run the docker container
```bash
docker-compose up --build
```

### 7. Open the application in your browser at:

   ```
   $ http://127.0.0.1:8000/admin/
   $ http://127.0.0.1:8000/api/
   $ http://127.0.0.1:8000/api/info/
   $ http://127.0.0.1:8000/api/upload/
   $ http://127.0.0.1:8000/api/upload-request/
   $ http://127.0.0.1:8000/api/upload-callback/
   $ http://127.0.0.1:8000/api/device/

   $ http://127.0.0.1:3000/

   ```
## For Troubleshooting (Common Operations)
 - Run migrations (if you modify models):
```bash 
docker-compose exec backend python manage.py migrate
```

 - Access Postgresql:
```bash 
docker-compose exec postgres psql -U your_db_user -d your_db_name
```

 - Run Test Suites
```bash 
docker-compose exec backend python manage.py test
```
 - Containers not starting
```bash
docker-compose logs -f
```
 - Missing Dependencies
```bash
docker-compose down -v && docker-compose up --build
```
 - Check container status
```bash
docker-compose ps
```
 - Inspect logs
```bash
docker-compose logs backend
docker-compose logs localstack
```
 - Full Clean Rebuild Command
```
docker-compose down -v --remove-orphans
docker system prune -a -f
docker volume prune -f
docker network prune -f
docker-compose up --force-recreate --build
```

## Deployment
- This configuration is optimized for local development only. 
- For production:
    - Automate deployment on AWS using CDK or Terraform that creates IaC (Infrastructure as Code) to achieve version control, update easily, and deploy across dev/staging/production environments.
     - - You may add deployment scripts inside the 'infra' folder
    - Use managed PostgreSQL (e.g., AWS RDS)
    - Configure proper security groups
    - Set up CI/CD pipeline


## Contributing
 - Fork the repository
 - Create feature branch (git checkout -b feature/foo)
 - Commit changes (git commit -am 'Add foo feature')
 - Push branch (git push origin feature/foo)
 - Create Pull Request

### This guide assumes:
 - All AWS credentials use test defaults
 - Database credentials are preconfigured

### Note to Developers
 - Do not use pip freeze for Docker environments 
    - always manually maintain your requirements.txt with verified versions.
 - Developers only need Docker/Docker Compose installed - no Python or AWS CLI required locally!
 - The upload-callback API endpoint requires to send full-formed JSON e.g.
 ```
 {
    "file_hash": "9b63f7b87382a6af23f4e96a19d996ca9669637742fd50767e04e45ba518951d",
    "s3_uri": "s3://rag-file-upload-bucket/uploads/9b63f7b87382a6af23f4e96a19d996ca9669637742fd50767e04e45ba518951d.pdf"
}
 ```

## License
This project is licensed under the Apache License 2.0. See the LICENSE file for more details.