# Ecommerce Microservices 🏗️

A production-grade **microservices-based e-commerce platform** built with .NET Core, featuring independent microservices for Products, Orders, and Users management. The platform leverages Azure Kubernetes Service (AKS) for orchestration, multiple databases, and advanced messaging patterns for scalable enterprise solutions.

---

## 🌟 Key Features

- **Microservices Architecture** - Three independent microservices (Products, Orders, Users)
- **Multiple Databases** - MySQL, PostgreSQL, MongoDB for different data requirements
- **Message Queue** - RabbitMQ for asynchronous communication between services
- **Caching Layer** - Redis cache for performance optimization
- **API Gateway** - Centralized entry point for all microservices
- **Container Orchestration** - Azure Kubernetes Service (AKS) deployment
- **CI/CD Pipelines** - Azure DevOps for automated build and deployment
- **Multi-Environment Support** - Dev, QA, UAT, Staging, and Production environments
- **API Management** - Azure API Management (APIM) for API governance

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend Application                      │
└────────────────────────┬────────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────────┐
│                    API Gateway (Port 5000)                       │
│                                                                   │
│           Routes requests to microservices                       │
└────┬──────────────────┬───────────────────────┬──────────────────┘
     │                  │                       │
     │                  │                       │
┌────▼──────┐   ┌───────▼────────┐   ┌────────▼────────┐
│  Products │   │    Orders      │   │     Users       │
│Microservice│   │ Microservice   │   │  Microservice   │
└────┬──────┘   └────┬───────────┘   └────────┬────────┘
     │               │                        │
┌────▼──┐      ┌────▼──────┐           ┌─────▼──────┐
│ MySQL │      │ MongoDB   │           │ PostgreSQL │
└───────┘      └───────────┘           └────────────┘
```

---

## 📁 Project Structure

```
Ecommerce_microservices/
├── eCommerceSolution.ProductsService/
│   ├── ProductsMicroService.API/
│   ├── ProductsUnitTests/
│   └── Dockerfile
├── eCommerceSolution.OrdersService/
│   ├── OrdersMicroservice.API/
│   ├── ApiGatway/
│   └── Dockerfile
├── eCommerceSolution.UsersService/
│   ├── eCommerce.API/
│   └── Dockerfile
├── frontend/
│   └── React/Angular application
├── mysql/
│   └── Dockerfile (with initialization scripts)
├── postgres/
│   └── Dockerfile (with initialization scripts)
├── mongodb/
│   └── Dockerfile (with initialization scripts)
├── redis-cache/
│   └── Configuration
├── aks/
│   └── Kubernetes deployment manifests
├── docker-compose.build.yaml
├── Complete Steps.yaml
└── README.md
```

---

## 🛠️ Tech Stack

### Backend Services
- **.NET Core** - Framework for microservices
- **C#** - Primary programming language
- **ASP.NET Core** - Web API framework
- **Entity Framework Core** - ORM for database operations

### Databases
- **MySQL** - Products microservice database
- **PostgreSQL** - Users microservice database
- **MongoDB** - Orders microservice database

### Infrastructure & Messaging
- **RabbitMQ** - Message broker for async communication
- **Redis** - Distributed cache
- **Docker** - Containerization
- **Kubernetes (AKS)** - Container orchestration

### Cloud & DevOps
- **Microsoft Azure** - Cloud platform
- **Azure Kubernetes Service (AKS)** - Kubernetes cluster
- **Azure Container Registry (ACR)** - Docker image registry
- **Azure Key Vault** - Secrets management
- **Azure DevOps** - CI/CD pipelines
- **Azure API Management** - API gateway and management

### Frontend
- **React/Angular** - Web application framework
- **Node.js** - Runtime environment

---

## 📋 Prerequisites

### Local Development
- **.NET SDK 6.0 or higher**
- **Docker Desktop** (with Docker Compose)
- **Node.js 16+** (for frontend)
- **Visual Studio 2022** or **Visual Studio Code**
- **MongoDB Client Tools** (optional)
- **MySQL Client** (optional)
- **PostgreSQL Client** (optional)

### Azure Deployment
- **Azure Subscription**
- **Azure CLI** installed and configured
- **kubectl** (Kubernetes CLI)
- **Azure DevOps Account**
- **Service Connections configured** in Azure DevOps

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/motsem021/Ecommerce_microservices.git
cd Ecommerce_microservices
```

### 2. Local Development with Docker Compose

#### Build All Services
```bash
docker-compose -f docker-compose.build.yaml build
```

#### Start All Services
```bash
docker-compose -f docker-compose.build.yaml up -d
```

#### Verify Services
```bash
docker-compose -f docker-compose.build.yaml ps
```

### 3. Access the Services

| Service | URL | Port |
|---------|-----|------|
| API Gateway | http://localhost:5000 | 5000 |
| Products Microservice | http://localhost:5000/gateway/products | 8080 |
| Orders Microservice | http://localhost:5000/gateway/orders | 8080 |
| Users Microservice | http://localhost:5000/gateway/users | 8080 |
| RabbitMQ Management | http://localhost:15672 | 15672 |
| MongoDB | localhost:27017 | 27017 |
| MySQL | localhost:3306 | 3306 |
| PostgreSQL | localhost:5432 | 5432 |
| Redis | localhost:6379 | 6379 |

---

## 🐳 Docker Compose Services

### API Gateway
```yaml
- Service: apigateway
- Port: 5000
- Routes requests to all microservices
- Depends on: All microservices
```

### Products Microservice
```yaml
- Service: products-microservice
- Database: MySQL (ecommerceproductsdatabase)
- Port: 8080
- Messaging: RabbitMQ (products.exchange)
```

### Orders Microservice
```yaml
- Service: orders-microservice
- Database: MongoDB (OrdersDatabase)
- Cache: Redis
- Port: 8080
- Messaging: RabbitMQ (products.exchange, users.exchange)
```

### Users Microservice
```yaml
- Service: users-microservice
- Database: PostgreSQL (eCommerceUsers)
- Port: 8080
- Messaging: RabbitMQ (users.exchange)
```

---

## 📚 Microservices Details

### Products Microservice
**Responsibilities:**
- Product catalog management
- Product inventory
- Product search and filtering
- Price management

**Database:** MySQL
**Technologies:** .NET Core, Entity Framework Core

### Orders Microservice
**Responsibilities:**
- Order creation and management
- Order tracking
- Payment processing
- Order status notifications

**Database:** MongoDB
**Cache:** Redis (for order caching)
**Technologies:** .NET Core, MongoDB Driver

### Users Microservice
**Responsibilities:**
- User registration and authentication
- User profile management
- User preferences
- Account management

**Database:** PostgreSQL
**Technologies:** .NET Core, Entity Framework Core

---

## 🔌 Microservices Communication

### Synchronous Communication
- Services communicate via REST APIs
- Direct HTTP calls through API Gateway
- Used for immediate responses

### Asynchronous Communication
- **RabbitMQ** for event-driven architecture
- **Products Exchange** - Product-related events
- **Users Exchange** - User-related events
- **Orders Exchange** - Order-related events

### Example Event Flow
```
1. User places order (Orders Microservice)
2. Order event published to RabbitMQ
3. Products Microservice listens and updates inventory
4. Users Microservice listens and updates user stats
```

---

## ☸️ Kubernetes Deployment

### Prerequisites
```bash
# Azure Login
az login --tenant YOUR_TENANT_ID

# Create Resource Group
az group create --name yourname-resource-group --location "South India"

# Create Container Registry
az acr create --resource-group yourname-resource-group \
  --name youracrname --sku Basic

# Create AKS Cluster
az aks create --resource-group yourname-resource-group \
  --name ecommerce-aks-cluster \
  --node-count 1 \
  --node-vm-size Standard_B2s \
  --enable-addons monitoring

# Get Credentials
az aks get-credentials --resource-group yourname-resource-group \
  --name ecommerce-aks-cluster
```

### Deploy to Kubernetes
```bash
# Apply deployments for each namespace
kubectl apply -f aks/dev/
kubectl apply -f aks/qa/
kubectl apply -f aks/staging/
kubectl apply -f aks/prod/

# Verify deployments
kubectl get all --namespace dev
kubectl get pods --namespace dev
kubectl get svc --namespace dev
```

### Check Cluster Status
```bash
kubectl cluster-info
kubectl get nodes
kubectl get all -A
```

---

## 🔐 Azure Key Vault Setup

### Create Key Vault
```bash
az keyvault create --name products-pipeline-kv3 \
  --resource-group yourname-resource-group \
  --location "South India"
```

### Store Secrets
```bash
# Store image repository name
az keyvault secret set --vault-name products-pipeline-kv3 \
  --name imageRepository \
  --value "products-microservice"

# Retrieve secret
az keyvault secret show --vault-name products-pipeline-kv3 \
  --name imageRepository
```

---

## 🔄 Azure DevOps CI/CD Pipeline

### Pipeline Stages

#### 1. Initialize Key Vault
- Fetch secrets from Azure Key Vault
- Prepare credentials for deployment

#### 2. Build
- Build Docker images
- Push to Azure Container Registry (ACR)

#### 3. Test
- Run unit tests
- Code coverage analysis
- Build solution

#### 4. Deploy to Dev
- Triggered on `dev` branch
- Deploys to dev namespace
- Uses variable group: `products-microservice-dev`

#### 5. Deploy to QA
- Triggered on `qa` branch
- Deploys to qa namespace
- Uses variable group: `products-microservice-qa`

#### 6. Deploy to UAT
- Triggered on `uat` branch
- Deploys to uat namespace
- Uses variable group: `products-microservice-uat`

#### 7. Deploy to Staging
- Triggered on `staging` branch
- Deploys to staging namespace
- Uses variable group: `products-microservice-staging`

#### 8. Deploy to Production
- Triggered on `prod` branch
- Deploys to prod namespace
- Uses variable group: `products-microservice-prod`

### Branch Strategy
```
main
├── dev        → Dev environment
├── qa         → QA environment
├── uat        → UAT environment
├── staging    → Staging environment
└── prod       → Production environment
```

---

## 📊 Docker Images

### Building Images
```bash
# Build all services
docker-compose -f docker-compose.build.yaml build

# Push to Azure Container Registry
az acr login --name youracrname

docker tag products-microservice:latest youracrname.azurecr.io/products-microservice:latest
docker tag orders-microservice:latest youracrname.azurecr.io/orders-microservice:latest
docker tag users-microservice:latest youracrname.azurecr.io/users-microservice:latest
docker tag apigateway:latest youracrname.azurecr.io/apigateway:latest

# Push images
docker push youracrname.azurecr.io/products-microservice:latest
docker push youracrname.azurecr.io/orders-microservice:latest
docker push youracrname.azurecr.io/users-microservice:latest
docker push youracrname.azurecr.io/apigateway:latest
```

---

## 🔌 API Gateway Configuration

### Add APIs to Azure API Management

#### Products API
```
Display name: Products Microservice API
Name: products-microservice-api
Web service URL: http://13.71.113.200:8080/gateway/products
API URL suffix: gateway/products
```

#### Orders API
```
Display name: Orders Microservice API
Name: orders-microservice-api
Web service URL: http://13.71.113.200:8080/gateway/orders
API URL suffix: gateway/orders
```

#### Users API
```
Display name: Users Microservice API
Name: users-microservice-api
Web service URL: http://13.71.113.200:8080/gateway/users
API URL suffix: gateway/users
```

---

## 🗄️ Database Configuration

### MySQL (Products)
```
Host: mysql-container (Docker) / RDS Endpoint (Azure)
Port: 3306
Database: ecommerceproductsdatabase
User: root
Password: admin
```

### PostgreSQL (Users)
```
Host: postgres-container (Docker) / Azure Database for PostgreSQL
Port: 5432
Database: eCommerceUsers
User: postgres
Password: admin
```

### MongoDB (Orders)
```
Host: mongodb-container (Docker) / Azure Cosmos DB
Port: 27017
Database: OrdersDatabase
```

---

## 💬 RabbitMQ Messaging

### Exchanges
- **products.exchange** - Product-related events
- **users.exchange** - User-related events
- **orders.exchange** - Order-related events

### Connection Details
```
Hostname: rabbitmq
Port: 5672
Username: guest
Password: guest
Management UI: http://localhost:15672
```

---

## 📝 Unit Testing

### Run Tests
```bash
# Restore NuGet packages
nuget restore

# Build solution
dotnet build

# Run unit tests
dotnet test

# Run specific test project
dotnet test ProductsUnitTests.dll
```

---

## 🐛 Troubleshooting

### Docker Compose Issues
```bash
# Remove all containers and volumes
docker-compose -f docker-compose.build.yaml down -v

# Rebuild and start fresh
docker-compose -f docker-compose.build.yaml build --no-cache
docker-compose -f docker-compose.build.yaml up -d
```

### Kubernetes Issues
```bash
# Check pod logs
kubectl logs <pod-name> -n <namespace>

# Describe pod for events
kubectl describe pod <pod-name> -n <namespace>

# Port forward to test locally
kubectl port-forward svc/products-microservice 8080:8080 -n dev
```

### Database Connection Issues
```bash
# Test MySQL connection
mysql -h localhost -u root -padmin ecommerceproductsdatabase

# Test PostgreSQL connection
psql -h localhost -U postgres -d eCommerceUsers

# Test MongoDB connection
mongosh mongodb://localhost:27017
```

---

## 📖 Additional Resources

- [Microservices Architecture](https://microservices.io/)
- [Azure Kubernetes Service Docs](https://learn.microsoft.com/en-us/azure/aks/)
- [Azure DevOps Documentation](https://learn.microsoft.com/en-us/azure/devops/)
- [.NET Microservices Guide](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/)
- [RabbitMQ Documentation](https://www.rabbitmq.com/documentation.html)
- [Docker Documentation](https://docs.docker.com/)

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Code Standards
- Follow C# coding conventions
- Add unit tests for new features
- Ensure all tests pass before submitting PR
- Update documentation as needed

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👤 Author

**Mohit Sem** (@motsem021)

---

## 🙏 Acknowledgments

- Microsoft Azure for cloud infrastructure
- Azure Kubernetes Service (AKS) community
- Azure DevOps for CI/CD capabilities
- .NET community for excellent tools and documentation

---

## 📞 Support

For issues and questions:
1. Check the `Complete Steps.yaml` file for detailed deployment steps
2. Review Azure DevOps pipeline logs
3. Open an issue on GitHub
4. Contact the project maintainer

**Repository:** [github.com/motsem021/Ecommerce_microservices](https://github.com/motsem021/Ecommerce_microservices)

---

## 🔍 Quick Reference

| Component | Purpose | Technology |
|-----------|---------|-----------|
| API Gateway | Request routing | .NET Core |
| Products Service | Product management | .NET Core + MySQL |
| Orders Service | Order management | .NET Core + MongoDB |
| Users Service | User management | .NET Core + PostgreSQL |
| Message Queue | Async communication | RabbitMQ |
| Cache Layer | Performance | Redis |
| Container Orchestration | Service deployment | Kubernetes/AKS |
| CI/CD | Automated pipelines | Azure DevOps |
