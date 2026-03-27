# LAB 5 - Network Automation & Microservices

## 📋 Overview
Implementation of network automation using microservices architecture with Docker containers and Python services.

## 📄 Lab Files
- **[Lab05_Report.pdf](./Lab05_Report.pdf)** - Complete lab report and documentation
- **[docker-compose.yml](./docker-compose.yml)** - Docker Compose configuration for microservices
- **[automation/](./automation/)** - Python automation scripts

## 🎯 Objectives
- Implement microservices architecture for network automation
- Deploy containerized services using Docker
- Create automated network service workflows
- Integrate AI/ML services with network infrastructure
- Build scalable and maintainable network automation solutions

## 🐳 Microservices Architecture

### Services Included:
1. **Gateway Service** ([gateway_service.py](./automation/gateway_service.py))
   - API gateway for routing requests
   - Service orchestration
   - Load balancing

2. **Upload Service** ([upload_service.py](./automation/upload_service.py))
   - File upload handling
   - Data ingestion
   - Storage management

3. **Processing Service** ([processing_service.py](./automation/processing_service.py))
   - Data processing pipeline
   - Network data analysis
   - Transformation logic

4. **AI Service** ([ai_service.py](./automation/ai_service.py))
   - Machine learning integration
   - Network prediction models
   - Intelligent decision making

5. **Service Manager** ([start_services.py](./automation/start_services.py))
   - Service lifecycle management
   - Health monitoring
   - Auto-scaling coordination

## 🚀 Deployment

### Using Docker Compose:
```bash
docker-compose up -d
```

### Service Ports:
- Gateway: Port 8000
- Upload: Port 8001
- Processing: Port 8002
- AI Service: Port 8003

## 🔧 Technologies Used
- **Containerization:** Docker, Docker Compose
- **Programming:** Python 3.x
- **API Framework:** Flask/FastAPI
- **Orchestration:** Service mesh architecture
- **Automation:** Python automation scripts

## 📊 Features
- RESTful API endpoints
- Service discovery and registration
- Automated deployment pipeline
- Monitoring and logging
- Fault tolerance and recovery

## ✅ Completion Status
- [x] Microservices designed and implemented
- [x] Docker containers configured
- [x] Services deployed and tested
- [x] Automation scripts working
- [x] Complete documentation

---
📅 **Course:** Intergalactic Communications  
🎓 **Student ID:** 673380427-8
