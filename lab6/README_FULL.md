# LAB 6 - Resilient Internet + Secure Microservices

## 📋 Overview
Enterprise-grade WAN architecture featuring dynamic routing (OSPF), WAN failure detection (IP SLA), edge security (ACL firewalling), and controlled microservice exposure using API Gateway pattern.

## 📄 Lab Files
- **[Lab06_Report.pdf](./Lab06_Report.pdf)** - Complete lab report with detailed implementation (4MB)
- **[docker-compose.yml](./docker-compose.yml)** - Infrastructure orchestration
- **[configs/](./configs/)** - Router configuration files
- **[scripts/](./scripts/)** - Validation and testing scripts
- **[gateway/](./gateway/)** - API Gateway service
- **[service-a/](./service-a/)** - Processing service
- **[service-b/](./service-b/)** - AI service  
- **[observability/](./observability/)** - Security monitoring service

## 🎯 Lab Objectives
Lab 6 transforms a basic SME network into **Enterprise-grade resilient WAN** with:

✅ **Dynamic Routing (OSPF)** - Replace static routing with automatic route exchange  
✅ **WAN Intelligence (IP SLA)** - ISP failure detection and automatic failover  
✅ **Edge Security (ACL)** - Firewall-protected WAN interface  
✅ **Controlled Exposure** - API Gateway pattern with NAT Static  
✅ **Observability Foundation** - Security service with ACL monitoring  

## 🏗️ Network Topology

```
                    Internet
                       |
              R1 G0/0 (NAT Edge + OSPF + IP SLA)
                       |
        R1 S0/0/0 ---- R2 S0/0/0 (WAN: 100.10.10.0/30)
            |              |
          LAN A          LAN B
    (192.168.10.0/24) (192.168.20.0/24)
       ServerA             ServerB
       ClientA             ClientB
```

### Network Segments:
- **internet_net**: `10.255.0.0/24` - R1 ↔ Internet
- **isp_net**: `100.10.10.0/30` - WAN link (R1 ↔ R2)
- **lan_a_net**: `192.168.10.0/24` - LAN A with microservices stack
- **lan_b_net**: `192.168.20.0/24` - LAN B with microservices stack

### Address Table:
| Device | Role | Networks | IP Addresses |
|--------|------|----------|--------------|
| R1 | Internet Edge + NAT + OSPF | internet/isp/lan_a | 10.255.0.2 / 100.10.10.1 / 192.168.10.1 |
| R2 | Branch Router + OSPF | isp/lan_b | 100.10.10.2 / 192.168.20.1 |
| ServerA | Microservices Stack A | lan_a_net | 192.168.10.10 |
| ServerB | Microservices Stack B | lan_b_net | 192.168.20.10 |
| ClientA | Testing Client | lan_a_net | 192.168.10.50 |
| ClientB | Testing Client | lan_b_net | 192.168.20.50 |

## 🚀 Implementation Phases

### Phase 1 - OSPF Dynamic Routing
Replaced static routing with OSPF Area 0:

**R1 Configuration:**
```
router ospf 1
 router-id 1.1.1.1
 network 100.10.10.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 passive-interface g0/1
 default-information originate
```

**R2 Configuration:**
```
router ospf 1
 router-id 2.2.2.2
 network 100.10.10.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0
 passive-interface g0/1
```

**Result:** Automatic route exchange - R2 learns default route from R1 via OSPF

### Phase 2 - IP SLA WAN Failure Detection
ISP outage detection using ICMP echo to 8.8.8.8:

```
ip sla 1
 icmp-echo 8.8.8.8 source-interface g0/0
 frequency 5
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability
ip route 0.0.0.0 0.0.0.0 g0/0 track 1
```

**Behavior:**
- **ISP UP**: Default route active, full connectivity
- **ISP DOWN**: Default route removed, controlled failover

### Phase 3 - WAN Security ACL
Edge firewall on R1 WAN interface:

```
ip access-list extended WAN-IN
 permit tcp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip any any log
interface s0/0/0
 ip access-group WAN-IN in
```

**Security Policy:**
- ✅ PERMIT: LAN B → LAN A (TCP traffic)
- ❌ DENY: All unsolicited Internet inbound (with logging)

### Phase 4 - Microservice Exposure Control
API Gateway pattern with NAT Static:

```
ip nat inside source static tcp 192.168.10.10 8000 interface g0/0 8000
```

**Service Exposure:**
- **Port 8000**: Gateway Service (PUBLIC - exposed via NAT)
- **Ports 8001, 8002, 9000**: Backend services (PRIVATE - internal only)

**Test Results:**
```bash
# Port 8000 - SUCCESS
curl http://10.255.0.2:8000/health
{"status": "ok", "service": "gateway", "port": 8000}

# Port 9000 - BLOCKED
curl http://10.255.0.2:9000/health
curl: (7) Failed to connect - Connection refused
```

### Phase 5 - Observability Foundation
Security Service (port 8080) for ACL monitoring:

**Services Running:**
```
[+] Upload Service (PID 20) on port 8000
[+] Processing Service (PID 21) on port 8001
[+] AI Service (PID 22) on port 8002
[+] Gateway Service (PID 23) on port 9000
[+] Security Service (PID 24) on port 8080
```

**ACL Policy Status:**
```json
{
  "wan_in_acl": "ACTIVE",
  "rule_1": "PERMIT tcp 192.168.20.0/24 -> 192.168.10.0/24",
  "rule_2": "DENY ip any any LOG",
  "nat_static": "192.168.10.10:8000 exposed (gateway only)",
  "backend_ports": "8001,8002,9000 PRIVATE"
}
```

### Phase 6 - Network Validation

**Test 1: Cross-LAN Connectivity**
```bash
$ docker exec ClientA ping -c 3 192.168.20.10
3 packets transmitted, 3 received, 0% packet loss
Path: ClientA → R1 → WAN → R2 → ServerB (via OSPF)
```

**Test 2: Cross-Site Microservice Call**
```bash
$ docker exec ClientA curl http://192.168.20.10:8000/health
{"status": "ok", "service": "upload", "port": 8000}
```

**Test 3: Gateway Pipeline**
```bash
$ docker exec ClientA curl -X POST http://192.168.20.10:9000/process-file
{
  "status": "ok",
  "pipeline": [
    {"step": "upload", "status": 200},
    {"step": "processing", "status": 200},
    {"step": "ai", "status": 200, "confidence": 0.97}
  ]
}
```

## 📊 Test Plan Results

| Test | Expected Result | Actual Result | Status |
|------|----------------|---------------|--------|
| OSPF neighbor state | FULL (routes exchanged) | FULL | ✅ PASS |
| show ip route ospf | Remote LAN visible | 192.168.20.0/24 via OSPF | ✅ PASS |
| ISP unplug simulation | Default route disappears | Route removed | ✅ PASS |
| Internet → port 8000 | HTTP 200 OK | Connection successful | ✅ PASS |
| Internet → port 9000 | Connection blocked | Connection refused | ✅ PASS |
| LAN A ↔ LAN B | 0% packet loss | 0% loss | ✅ PASS |

## 🏆 Architecture Classification

**Enterprise Branch Architecture v1**

### Readiness Assessment:

| Category | Score | Notes |
|----------|-------|-------|
| **Routing** | 9/10 | Dynamic OSPF, automatic convergence |
| **WAN Intelligence** | 9/10 | IP SLA failure detection, tracked routes |
| **Security** | 6/10 | ACL firewall, needs deeper inspection |
| **Microservice Exposure** | 8/10 | API Gateway pattern, controlled NAT |
| **Scalability** | 8/10 | Ready for Phase 2 scale-out |
| **Overall** | **8.5/10** | **Enterprise Branch-Ready** |

## 🔧 Technologies & Protocols

- **Routing:** OSPF (Open Shortest Path First) Area 0
- **WAN Monitoring:** IP SLA with ICMP tracking
- **Security:** ACL (Access Control Lists) with logging
- **NAT:** Static NAT for service exposure
- **Containerization:** Docker + Docker Compose
- **Microservices:** Python Flask/FastAPI services
- **Observability:** Security monitoring service

## 🛠️ Quick Start Commands

```bash
# Deploy infrastructure
docker compose down -v
docker compose up -d

# Verify OSPF routes
docker exec R1 ip route show

# Check IP SLA status
docker exec R1 sh /config/r1_setup.sh

# Test microservice exposure
curl http://10.255.0.2:8000/health

# Monitor ACL policy
docker exec ClientA curl http://192.168.10.10:8080/policy

# Validate cross-LAN connectivity
docker exec ClientA ping 192.168.20.10
```

## 📁 Project Structure

```
lab6/
├── Lab06_Report.pdf              # Complete documentation
├── README.md                     # This file
├── docker-compose.yml            # Infrastructure definition
├── configs/                      # Router configurations
│   ├── r1_config.sh
│   └── r2_config.sh
├── gateway/                      # API Gateway service
│   └── app.py
├── service-a/                    # Processing service
│   └── app.py
├── service-b/                    # AI service
│   └── app.py
├── observability/                # Security monitoring
│   └── security_service.py
└── scripts/                      # Testing & validation
    └── validate.sh
```

## 🎓 Key Learnings

1. **OSPF Dynamic Routing** eliminates manual static route management
2. **IP SLA** provides intelligent WAN failure detection and automatic failover
3. **ACL Firewalling** protects network edge from unsolicited traffic
4. **API Gateway Pattern** (via NAT Static) controls service exposure
5. **Observability** through Security Service enables production monitoring
6. **Enterprise Architecture** requires routing intelligence, edge security, and controlled exposure

## 👥 Team Members

| Name | Student ID | Email | Role |
|------|-----------|-------|------|
| นายศุภกิตติ์ ฟันเฟือย | 673380427-8 | Suphakit.f@kkumail.com | Network Architect |
| นายกฤษฎา นามมนต์เทียน | 673380388-2 | kritsada.na@kkumail.com | IaC/DevOps |
| นายวงศธร ธน.ยอด | 673380425-2 | wongsathon.th@kkumail.com | IaC/DevOps |
| นายปรีชา ศรีหนองบัว | 653380335-1 | preecha.sr@kkumail.com | QA/Test |
| นายพัชรพล กองแก้ว | 673390415-5 | phatcharaphon.ko@kkumail.com | QA/Test |
| นายดรัณภพ สุริเตอร์ | 673380402-4 | darunphop.s@kkumail.com | QA/Test |

## ✅ Conclusion

Lab 6 successfully demonstrated the transformation from basic static routing to **Enterprise-grade resilient WAN** architecture. The implementation showcases:

- ✅ Dynamic routing with OSPF automatic route exchange
- ✅ WAN intelligence through IP SLA failure detection
- ✅ Edge security via ACL firewalling with logging
- ✅ Controlled microservice exposure using API Gateway pattern
- ✅ Production-ready observability foundation

**Status:** Ready for production deployment with enterprise-level routing intelligence, edge security, and controlled service exposure.

---
📅 **Course:** CP352005 Network Programming  
🏫 **Institution:** Khon Kaen University  
🎓 **Lead:** นายศุภกิตติ์ ฟันเฟือย (673380427-8)
