# MAS Compiler Service: Architecture & System Design

**Service Mission**: Transform natural language and structured inputs into MAS v2.0-compliant Mermaid diagrams at scale

---

## 🏗️ High-Level Architecture

### System Context (C4 Level 1)

```mermaid
C4Context
    title MAS Compiler Service - System Context
    
    Person(agent, "AI Agent", "Backend/Frontend/Product agents")
    Person(developer, "Developer", "Agent developers")
    
    System(mas_compiler, "MAS Compiler Service", "Validates and generates MAS v2.0-compliant diagrams")
    
    System_Ext(llm, "LLM Service", "GPT-4 for semantic understanding")
    System_Ext(monitoring, "Monitoring", "CloudWatch/Datadog")
    System_Ext(auth, "Auth Service", "API key management")
    
    Rel(agent, mas_compiler, "Generate diagrams", "HTTPS/REST")
    Rel(developer, mas_compiler, "Validate diagrams", "HTTPS/REST")
    Rel(mas_compiler, llm, "Semantic parsing", "HTTPS")
    Rel(mas_compiler, monitoring, "Logs & metrics", "CloudWatch API")
    Rel(mas_compiler, auth, "Verify API keys", "HTTPS")
    
    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

---

## 🔧 Container Architecture (C4 Level 2)

```mermaid
C4Container
    title MAS Compiler Service - Container Diagram
    
    Person(agent, "AI Agent", "Requests diagram generation")
    
    Container_Boundary(mas_service, "MAS Compiler Service") {
        Container(api, "API Gateway", "AWS API Gateway", "Rate limiting, auth, routing")
        Container(compiler, "Compiler Lambda", "Python/FastAPI", "Core compilation logic")
        Container(validator, "Validator Lambda", "Python", "MAS v2.0 validation")
        Container(cache, "Redis Cache", "ElastiCache", "Caches validated diagrams")
        ContainerDb(db, "DynamoDB", "NoSQL", "Stores conformance reports")
    }
    
    System_Ext(llm, "GPT-4 API", "Azure OpenAI")
    
    Rel(agent, api, "POST /compile", "HTTPS/JSON")
    Rel(api, compiler, "Invoke", "Lambda event")
    Rel(compiler, llm, "Semantic parsing", "HTTPS")
    Rel(compiler, validator, "Validate output", "Direct call")
    Rel(compiler, cache, "Check cache", "Redis protocol")
    Rel(compiler, db, "Store report", "DynamoDB API")
    Rel(validator, cache, "Cache results", "Redis protocol")
    
    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```

---

## ⚙️ Component Architecture (C4 Level 3)

### Compiler Lambda Components

```mermaid
graph TB
    subgraph "API Layer"
        REST[REST Endpoints<br/>FastAPI]
        VALIDATOR_INPUT[Input Validator<br/>JSON Schema]
        AUTH[Auth Middleware<br/>API Key Verification]
    end
    
    subgraph "Business Logic Layer"
        PARSER[Semantic Parser<br/>Intent Extraction]
        TYPE_SELECTOR[Diagram Type Selector<br/>Topology/Sequence/Class]
        GENERATOR[Code Generator<br/>Mermaid Syntax]
        OPTIMIZER[Optimizer<br/>Best Practices]
    end
    
    subgraph "Validation Layer"
        SYNTAX_VAL[Syntax Validator<br/>Mermaid.js Parser]
        SEMANTIC_VAL[Semantic Validator<br/>MAS v2.0 Rules]
        SECURITY_VAL[Security Validator<br/>Injection Prevention]
        CONFORMANCE[Conformance Checker<br/>Minimal/Standard/Full]
    end
    
    subgraph "Integration Layer"
        LLM_CLIENT[LLM Client<br/>GPT-4 Integration]
        CACHE_CLIENT[Cache Client<br/>Redis Connection]
        DB_CLIENT[DB Client<br/>DynamoDB Access]
        METRICS[Metrics Collector<br/>CloudWatch]
    end
    
    REST --> VALIDATOR_INPUT
    REST --> AUTH
    VALIDATOR_INPUT --> PARSER
    
    PARSER --> LLM_CLIENT
    PARSER --> TYPE_SELECTOR
    TYPE_SELECTOR --> GENERATOR
    GENERATOR --> OPTIMIZER
    
    OPTIMIZER --> SYNTAX_VAL
    SYNTAX_VAL --> SEMANTIC_VAL
    SEMANTIC_VAL --> SECURITY_VAL
    SECURITY_VAL --> CONFORMANCE
    
    CACHE_CLIENT -.->|Cache Check| PARSER
    CACHE_CLIENT -.->|Cache Store| CONFORMANCE
    DB_CLIENT -.->|Store Report| CONFORMANCE
    METRICS -.->|Track Events| REST
    
    style REST fill:#e1f5ff
    style PARSER fill:#fff4e1
    style GENERATOR fill:#e1ffe1
    style SYNTAX_VAL fill:#ffe1f5
    style CONFORMANCE fill:#ccffcc,stroke:#00aa00,stroke-width:3px
```

---

## 🔄 Data Flow Architecture

### Request Processing Pipeline

```mermaid
flowchart LR
    subgraph "Input Processing"
        INPUT[HTTP Request<br/>Natural Language or JSON]
        VALIDATE[Validate Request<br/>Schema Check]
        CACHE_CHECK{Cache<br/>Hit?}
    end
    
    subgraph "Core Processing"
        PARSE[Parse Intent<br/>LLM + Rules]
        SELECT[Select Diagram Type<br/>Heuristics]
        GENERATE[Generate Mermaid<br/>Template Engine]
    end
    
    subgraph "Validation Pipeline"
        SYNTAX{Syntax<br/>Valid?}
        SEMANTIC{Semantic<br/>Valid?}
        SECURITY{Security<br/>Valid?}
    end
    
    subgraph "Output Processing"
        REPORT[Build Conformance Report<br/>Metadata]
        CACHE_STORE[Store in Cache<br/>TTL: 1 hour]
        DB_STORE[Store Report<br/>Audit Trail]
        RESPONSE[HTTP Response<br/>JSON]
    end
    
    INPUT --> VALIDATE
    VALIDATE --> CACHE_CHECK
    CACHE_CHECK -->|Hit| RESPONSE
    CACHE_CHECK -->|Miss| PARSE
    
    PARSE --> SELECT
    SELECT --> GENERATE
    
    GENERATE --> SYNTAX
    SYNTAX -->|Pass| SEMANTIC
    SYNTAX -->|Fail| REPORT
    SEMANTIC -->|Pass| SECURITY
    SEMANTIC -->|Fail| REPORT
    SECURITY -->|Pass| REPORT
    SECURITY -->|Fail| REPORT
    
    REPORT --> CACHE_STORE
    REPORT --> DB_STORE
    REPORT --> RESPONSE
    
    style CACHE_CHECK fill:#fff4e1
    style SYNTAX fill:#ffe1f5
    style SEMANTIC fill:#ffe1f5
    style SECURITY fill:#ffe1f5
    style RESPONSE fill:#ccffcc,stroke:#00aa00,stroke-width:3px
```

---

## 🎯 API Endpoint Architecture

### REST API Design

```mermaid
classDiagram
    class CompileRequest {
        +String input_type
        +String description
        +String diagram_type_hint
        +String conformance_level
        +Dict options
        +validate()
    }
    
    class CompileResponse {
        +String status
        +String diagram_code
        +ConformanceReport conformance_report
        +Metadata metadata
        +to_json()
    }
    
    class ValidateRequest {
        +String diagram_code
        +Boolean validate_only
        +String conformance_level
        +validate()
    }
    
    class ConformanceReport {
        +String level
        +Boolean syntax_valid
        +Boolean semantic_valid
        +Boolean security_valid
        +List~Issue~ issues
        +generate_report()
    }
    
    class Issue {
        +String severity
        +Integer line
        +String message
        +String suggestion
    }
    
    class Metadata {
        +String diagram_type
        +Integer node_count
        +Integer edge_count
        +Integer generation_time_ms
    }
    
    class HealthResponse {
        +String status
        +String version
        +Dict dependencies
        +check_health()
    }
    
    CompileResponse --> ConformanceReport
    CompileResponse --> Metadata
    ConformanceReport --> Issue
    
    CompileRequest ..> CompileResponse : generates
    ValidateRequest ..> CompileResponse : generates
```

### API Endpoints

```mermaid
graph LR
    subgraph "Public API Endpoints"
        COMPILE[POST /api/v1/compile<br/>Generate diagram from NL]
        VALIDATE[POST /api/v1/validate<br/>Validate existing diagram]
        HEALTH[GET /api/v1/health<br/>Service health check]
        SPEC[GET /api/v1/spec<br/>OpenAPI specification]
    end
    
    subgraph "Internal Endpoints"
        METRICS[GET /internal/metrics<br/>Prometheus metrics]
        CACHE_CLEAR[POST /internal/cache/clear<br/>Clear cache]
    end
    
    CLIENT[AI Agent Client]
    ADMIN[Admin Console]
    
    CLIENT --> COMPILE
    CLIENT --> VALIDATE
    CLIENT --> HEALTH
    CLIENT --> SPEC
    
    ADMIN --> METRICS
    ADMIN --> CACHE_CLEAR
    
    style COMPILE fill:#e1ffe1,stroke:#00aa00,stroke-width:3px
    style VALIDATE fill:#e1ffe1,stroke:#00aa00,stroke-width:3px
```

---

## 🔐 Security Architecture

### Multi-Layer Security Design

```mermaid
graph TB
    subgraph "Security Layers"
        subgraph "Layer 1: Network Security"
            WAF[AWS WAF<br/>DDoS Protection]
            RATE[Rate Limiting<br/>100 req/min per API key]
        end
        
        subgraph "Layer 2: Authentication & Authorization"
            API_KEY[API Key Validation<br/>Header: X-API-Key]
            RBAC[Role-Based Access<br/>Free/Pro/Enterprise]
        end
        
        subgraph "Layer 3: Input Security"
            SCHEMA[JSON Schema Validation<br/>Strict Type Checking]
            SANITIZE[Input Sanitization<br/>Escape Special Chars]
            SIZE[Size Limits<br/>Max 10KB input]
        end
        
        subgraph "Layer 4: Processing Security"
            SANDBOX[Sandboxed Execution<br/>No Code Eval]
            TIMEOUT[Timeout Protection<br/>Max 10s per request]
            MEM_LIMIT[Memory Limits<br/>Max 512MB]
        end
        
        subgraph "Layer 5: Output Security"
            XSS[XSS Prevention<br/>Escape HTML/JS]
            INJECT[Injection Prevention<br/>MAS v2.0 Security Spec]
        end
    end
    
    REQUEST[Incoming Request]
    RESPONSE[Secure Response]
    
    REQUEST --> WAF
    WAF --> RATE
    RATE --> API_KEY
    API_KEY --> RBAC
    RBAC --> SCHEMA
    SCHEMA --> SANITIZE
    SANITIZE --> SIZE
    SIZE --> SANDBOX
    SANDBOX --> TIMEOUT
    TIMEOUT --> MEM_LIMIT
    MEM_LIMIT --> XSS
    XSS --> INJECT
    INJECT --> RESPONSE
    
    style WAF fill:#ffcccc
    style API_KEY fill:#ffddcc
    style SANITIZE fill:#ffeecc
    style SANDBOX fill:#ffffcc
    style INJECT fill:#ccffcc,stroke:#00aa00,stroke-width:3px
```

---

## 📊 Scalability Architecture

### Auto-Scaling Strategy

```mermaid
graph TB
    subgraph "Load Balancing"
        ALB[Application Load Balancer<br/>AWS ALB]
        API_GW[API Gateway<br/>Request Routing]
    end
    
    subgraph "Compute Layer (Auto-Scaling)"
        LAMBDA1[Compiler Lambda 1<br/>Instance]
        LAMBDA2[Compiler Lambda 2<br/>Instance]
        LAMBDA_N[Compiler Lambda N<br/>Instance]
        
        LAMBDA1 -.->|Scale| LAMBDA2
        LAMBDA2 -.->|Scale| LAMBDA_N
    end
    
    subgraph "Caching Layer"
        REDIS_MASTER[Redis Master<br/>Write]
        REDIS_REPLICA1[Redis Replica 1<br/>Read]
        REDIS_REPLICA2[Redis Replica 2<br/>Read]
        
        REDIS_MASTER -->|Replicate| REDIS_REPLICA1
        REDIS_MASTER -->|Replicate| REDIS_REPLICA2
    end
    
    subgraph "Data Layer"
        DYNAMO[DynamoDB<br/>On-Demand Scaling]
    end
    
    TRAFFIC[Client Traffic<br/>1000 req/min]
    
    TRAFFIC --> ALB
    ALB --> API_GW
    API_GW --> LAMBDA1
    API_GW --> LAMBDA2
    API_GW --> LAMBDA_N
    
    LAMBDA1 --> REDIS_REPLICA1
    LAMBDA2 --> REDIS_REPLICA2
    LAMBDA_N --> REDIS_MASTER
    
    LAMBDA1 --> DYNAMO
    LAMBDA2 --> DYNAMO
    LAMBDA_N --> DYNAMO
    
    style TRAFFIC fill:#ffcccc
    style ALB fill:#fff4e1
    style LAMBDA1 fill:#e1f5ff
    style LAMBDA2 fill:#e1f5ff
    style LAMBDA_N fill:#e1f5ff
    style REDIS_MASTER fill:#e1ffe1
    style DYNAMO fill:#ccffcc
```

### Scaling Metrics

```mermaid
graph LR
    subgraph "CloudWatch Metrics"
        CPU[Lambda Concurrent<br/>Executions]
        LATENCY[API Response Time<br/>p95, p99]
        ERROR[Error Rate<br/>4xx, 5xx]
        CACHE[Cache Hit Rate<br/>Target: >80%]
    end
    
    subgraph "Auto-Scaling Policies"
        SCALE_UP[Scale Up<br/>Concurrent > 50]
        SCALE_DOWN[Scale Down<br/>Concurrent < 10]
    end
    
    subgraph "Alerts"
        PAGER[PagerDuty<br/>Critical Alerts]
        SLACK[Slack<br/>Warning Alerts]
    end
    
    CPU --> SCALE_UP
    CPU --> SCALE_DOWN
    LATENCY --> PAGER
    ERROR --> PAGER
    CACHE --> SLACK
    
    style SCALE_UP fill:#ffcccc
    style SCALE_DOWN fill:#ccffcc
    style PAGER fill:#ff9999,stroke:#ff0000,stroke-width:3px
```

---

## 🗄️ Data Model

### DynamoDB Schema

```mermaid
erDiagram
    COMPILE_REQUEST {
        string request_id PK
        string agent_id
        string input_type
        string description
        timestamp created_at
        string diagram_type_hint
    }
    
    COMPILE_RESPONSE {
        string request_id PK
        string diagram_code
        string conformance_level
        json metadata
        timestamp generated_at
        int ttl
    }
    
    CONFORMANCE_REPORT {
        string report_id PK
        string request_id FK
        boolean syntax_valid
        boolean semantic_valid
        boolean security_valid
        json issues
        timestamp created_at
    }
    
    AGENT_USAGE {
        string agent_id PK
        string api_key FK
        int monthly_requests
        int total_requests
        timestamp last_request
        string tier
    }
    
    COMPILE_REQUEST ||--o| COMPILE_RESPONSE : generates
    COMPILE_RESPONSE ||--|| CONFORMANCE_REPORT : has
    AGENT_USAGE ||--o{ COMPILE_REQUEST : creates
```

---

## 🚀 Deployment Architecture

### Multi-Environment Strategy

```mermaid
graph TB
    subgraph "Development"
        DEV_CODE[GitHub Repo<br/>Feature Branches]
        DEV_CI[GitHub Actions<br/>Unit Tests]
        DEV_ENV[Dev Environment<br/>AWS Dev Account]
    end
    
    subgraph "Staging"
        STAGE_CI[Integration Tests<br/>E2E Tests]
        STAGE_ENV[Staging Environment<br/>AWS Staging Account]
    end
    
    subgraph "Production"
        PROD_APPROVAL[Manual Approval<br/>Release Manager]
        PROD_ENV[Production Environment<br/>AWS Prod Account]
        PROD_CANARY[Canary Deployment<br/>10% → 100%]
    end
    
    DEV_CODE --> DEV_CI
    DEV_CI -->|Pass| DEV_ENV
    DEV_ENV --> STAGE_CI
    STAGE_CI -->|Pass| STAGE_ENV
    STAGE_ENV --> PROD_APPROVAL
    PROD_APPROVAL -->|Approved| PROD_CANARY
    PROD_CANARY --> PROD_ENV
    
    style DEV_CODE fill:#e1f5ff
    style DEV_CI fill:#fff4e1
    style STAGE_CI fill:#ffe1f5
    style PROD_APPROVAL fill:#ffcccc,stroke:#ff0000,stroke-width:3px
    style PROD_ENV fill:#ccffcc,stroke:#00aa00,stroke-width:3px
```

---

## 📈 Monitoring & Observability

### Observability Stack

```mermaid
graph TB
    subgraph "Application Layer"
        LOGS[Application Logs<br/>Structured JSON]
        METRICS[Custom Metrics<br/>StatsD]
        TRACES[Distributed Traces<br/>X-Ray]
    end
    
    subgraph "Collection Layer"
        CLOUDWATCH[CloudWatch Logs<br/>Centralized Logging]
        PROMETHEUS[Prometheus<br/>Metrics Aggregation]
        XRAY[AWS X-Ray<br/>Trace Collector]
    end
    
    subgraph "Analysis Layer"
        KIBANA[Kibana<br/>Log Search]
        GRAFANA[Grafana<br/>Dashboards]
        XRAY_CONSOLE[X-Ray Console<br/>Trace Analysis]
    end
    
    subgraph "Alerting Layer"
        ALERTS[CloudWatch Alarms<br/>Threshold Alerts]
        PAGER[PagerDuty<br/>Incident Management]
    end
    
    LOGS --> CLOUDWATCH
    METRICS --> PROMETHEUS
    TRACES --> XRAY
    
    CLOUDWATCH --> KIBANA
    PROMETHEUS --> GRAFANA
    XRAY --> XRAY_CONSOLE
    
    CLOUDWATCH --> ALERTS
    PROMETHEUS --> ALERTS
    ALERTS --> PAGER
    
    style LOGS fill:#e1f5ff
    style CLOUDWATCH fill:#fff4e1
    style KIBANA fill:#e1ffe1
    style PAGER fill:#ffcccc,stroke:#ff0000,stroke-width:3px
```

---

## 🔄 Disaster Recovery

### Backup & Recovery Strategy

```mermaid
stateDiagram-v2
    [*] --> Normal_Operation
    
    Normal_Operation --> Continuous_Backup: Every 5 minutes
    Continuous_Backup --> Normal_Operation: Backup Successful
    
    Normal_Operation --> Failure_Detected: Service Down
    Failure_Detected --> Automatic_Failover: AWS Health Check
    
    Automatic_Failover --> Secondary_Region: Route53 DNS Switch
    Secondary_Region --> Restore_Service: Lambda Cold Start
    Restore_Service --> Normal_Operation: Service Restored
    
    Failure_Detected --> Manual_Recovery: Critical Data Loss
    Manual_Recovery --> Restore_From_Backup: DynamoDB PITR
    Restore_From_Backup --> Normal_Operation: Data Restored
    
    Normal_Operation --> [*]
```

**Recovery Targets**:
- **RTO** (Recovery Time Objective): < 5 minutes
- **RPO** (Recovery Point Objective): < 5 minutes
- **Availability**: 99.9% (43 minutes downtime/month max)

---

## 💰 Cost Optimization

### Cost Breakdown (Monthly at 1M Requests)

```mermaid
pie title Monthly Cost Breakdown ($500 total)
    "Lambda Compute (1M invocations)" : 200
    "API Gateway (1M requests)" : 35
    "DynamoDB (On-Demand)" : 50
    "ElastiCache (Redis)" : 100
    "CloudWatch Logs & Metrics" : 40
    "Data Transfer" : 30
    "LLM API (GPT-4)" : 45
```

**Cost Optimization Strategies**:
- ✅ Cache frequently requested diagrams (Redis)
- ✅ Use Lambda reserved concurrency to control costs
- ✅ Implement request batching for LLM calls
- ✅ Archive old conformance reports to S3 Glacier
- ✅ Use DynamoDB on-demand pricing for variable load

---

## 🎯 Performance Targets

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Response Time (p95)** | < 2 seconds | CloudWatch metrics |
| **Response Time (p99)** | < 5 seconds | CloudWatch metrics |
| **Throughput** | 100 concurrent requests | Load testing |
| **Cache Hit Rate** | > 80% | Redis metrics |
| **Error Rate** | < 0.1% | CloudWatch alarms |
| **Lambda Cold Start** | < 500ms | X-Ray traces |

---

## 📝 Technology Stack

```mermaid
graph TB
    subgraph "Frontend SDK"
        PY_SDK[Python SDK<br/>mas-compiler-py]
        JS_SDK[JavaScript SDK<br/>mas-compiler-js]
    end
    
    subgraph "API Layer"
        FASTAPI[FastAPI 0.104+<br/>Async ASGI]
        PYDANTIC[Pydantic 2.0<br/>Data Validation]
    end
    
    subgraph "Core Logic"
        PYTHON[Python 3.11<br/>Lambda Runtime]
        MERMAID[Mermaid.js 10.0+<br/>Syntax Validation]
        LLM_LIB[OpenAI SDK<br/>GPT-4 Client]
    end
    
    subgraph "Infrastructure"
        LAMBDA[AWS Lambda<br/>Serverless Compute]
        API_GW[AWS API Gateway<br/>HTTP API]
        DYNAMO[DynamoDB<br/>NoSQL Database]
        REDIS[ElastiCache Redis<br/>Caching]
    end
    
    subgraph "DevOps"
        TERRAFORM[Terraform<br/>IaC]
        GITHUB[GitHub Actions<br/>CI/CD]
        DOCKER[Docker<br/>Local Development]
    end
    
    PY_SDK --> FASTAPI
    JS_SDK --> FASTAPI
    FASTAPI --> PYTHON
    PYDANTIC --> FASTAPI
    PYTHON --> MERMAID
    PYTHON --> LLM_LIB
    
    FASTAPI --> LAMBDA
    LAMBDA --> API_GW
    LAMBDA --> DYNAMO
    LAMBDA --> REDIS
    
    TERRAFORM --> LAMBDA
    GITHUB --> DOCKER
    
    style FASTAPI fill:#e1ffe1,stroke:#00aa00,stroke-width:3px
    style PYTHON fill:#e1f5ff
    style LAMBDA fill:#fff4e1
```

---

*Created by: Alex Chen (Mermaid Guru)*  
*MAS v2.0 Specification Compliant*  
*Last Updated: 2025-11-27*
