# Product Requirements Document (PRD)
## AAA Server - Authentication, Authorization, & Accounting

### 1. Overview
Server AAA yang handal untuk autentikasi, otorisasi, dan akunting jaringan, 
mirip Kron AAA / RadiatorSoftware AAA, dibangun dengan Rust untuk performa tinggi dan 
keandalan maksimal.

### 2. Goals
- **Reliability**: Uptime 99.99%, crash-free operation
- **Performance**: Handle 100K+ concurrent RADIUS requests
- **Security**: Memory-safe, no UB, encrypted communications
- **Maintainability**: Clean code, comprehensive tests, documentation

### 3. Core Features

#### 3.1 Authentication
- [ ] RADIUS authentication (Access-Request)
- [ ] PAP, CHAP, MS-CHAPv2 support
- [ ] EAP-TLS, EAP-TTLS, PEAP support
- [ ] Multi-source user lookup (LDAP, SQL, file)
- [ ] Concurrent request processing
- [ ] Connection pooling for backends

#### 3.2 Authorization
- [ ] VLAN assignment per user/port
- [ ] Filter rules (match/reject)
- [ ] Attribute manipulation
- [ ] Policy engine
- [ ] Time-based access control

#### 3.3 Accounting
- [ ] RADIUS accounting (Accounting-Request)
- [ ] Start/Stop/Interim-Update handling
- [ ] Usage statistics collection
- [ ] Database storage (PostgreSQL/MySQL)
- [ ] Real-time monitoring dashboard

#### 3.4 Management
- [ ] Configuration management (YAML/TOML)
- [ ] Hot-reload configuration
- [ ] Logging (structured, multi-level)
- [ ] Metrics & monitoring (Prometheus/Grafana)
- [ ] API untuk management

### 4. Technical Requirements

#### 4.1 Architecture
```
┌─────────────────────────────────────────┐
│           AAA Server (Rust)              │
├─────────────────────────────────────────┤
│  RADIUS Daemon                            │
│  ├─ Authentication Handler                │
│  ├─ Authorization Engine                  │
│  ├─ Accounting Service                    │
│  └─ Connection Manager                    │
├─────────────────────────────────────────┤
│  Backend Services                         │
│  ├─ LDAP Client                           │
│  ├─ SQL Client                            │
│  └─ Certificate Manager                   │
├─────────────────────────────────────────┤
│  Storage                                  │
│  ├─ PostgreSQL / MySQL                     │
│  └─ File System (users)                   │
└─────────────────────────────────────────┘
```

#### 4.2 Technology Stack
- **Language**: Rust (nightly/stable)
- **RADIUS Protocol**: Custom implementation atau `freeradius-ng`
- **Database**: PostgreSQL (primary), Redis (cache)
- **Configuration**: TOML / YAML
- **Logging**: tracing / log
- **Serialization**: serde
- **HTTP Server**: Actix-web / Axum
- **Testing**: Integration + Unit tests

#### 4.3 Non-Functional Requirements
- **Memory Safety**: Zero data races, no undefined behavior
- **Performance**: Sub-millisecond auth response
- **Scalability**: Horizontal scaling support
- **Observability**: Full tracing, structured logging
- **Error Handling**: Graceful degradation, circuit breaker

### 5. Constraints
- **Anti-Hallucination**: Hanya gunakan library yang terverifikasi
- **Anti-Rewrite**: Jangan rewrite code yang sudah ada, tambah/improve
- **TDD**: Setiap feature harus ada test sebelum implementasi
- **Documentation**: Dokumentasi lengkap untuk setiap module
- **Security**: Audit code untuk vulnerability

### 6. Success Criteria
- [ ] Zero crashes dalam 30 hari produksi
- [ ] Test coverage > 90%
- [ ] Response time < 100ms untuk auth
- [ ] Documentation lengkap
- [ ] CI/CD pipeline berjalan
