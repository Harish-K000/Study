# AWS, Monitoring, CI/CD, Docker, and Kubernetes

[Master index](../README.md) · [Enterprise Java playbook](../FINAL-INTERVIEW-HANDBOOK.md) · [Answer book](../FINAL-INTERVIEW-ANSWER-BOOK.md) · Previous: [Database](../03-database/README.md) · Next: [System design](../05-system-design/README.md)

## AWS service map

| Service | Interview-ready explanation |
|---|---|
| EC2 | Managed virtual-server capacity where you control the OS and runtime |
| Lambda | Event-driven compute that runs code without managing servers |
| S3 | Durable object storage for files, raw data, backups, and artifacts |
| RDS | Managed relational databases |
| ECR | Managed registry that stores container images |
| ECS | AWS container orchestration |
| EKS | Managed Kubernetes on AWS |
| EventBridge | Event routing and scheduled/event-driven integration |
| IAM | Identity and permission control using roles and policies |
| CloudWatch | AWS metrics, logs, dashboards, and alarms |
| SNS | Pub/sub notifications, often used as an alarm destination |

## Lambda story

### Interview answer

AWS Lambda is a serverless compute service that executes code when an event occurs. In the Siemens platform, a practical use was asynchronous alert processing: after the ingestion API validated a sensor reading, it published an event; EventBridge triggered Lambda to evaluate alert rules and persist or dispatch a maintenance alert. This kept background work out of the synchronous API path. Lambda is like an on-call worker who starts only when a job arrives rather than sitting at a desk all day.

Do not confuse Lambda with the AWS CLI. The CLI is a command-line management tool; Lambda is the runtime that executes application code.

If a Lambda is packaged as a container:

```text
Code → Docker image → Amazon ECR → Lambda function → event trigger
```

Long-running Spring Boot APIs are more naturally hosted on ECS, EKS, or EC2; Lambda is best justified by event-driven, bounded work.

## Spring Boot API on AWS

```text
Client → Load Balancer/API Gateway → ECS/EKS/EC2
→ Spring Boot API → RDS/other services
```

### Interview answer

My main responsibility was the Spring Boot API layer: authentication, validation, business logic, database integration, and service-to-service calls. AWS provided the runtime environment. Container images were stored in ECR and deployed to the selected runtime, while Lambda handled specific event-driven background processing.

## Production monitoring

### Interview answer

I monitor infrastructure and application health together. CloudWatch provides metrics and logs for AWS resources; the CloudWatch Agent can collect memory, disk, and application log files from EC2. Spring Boot Actuator exposes health and metrics, while structured logs and correlation IDs make individual requests traceable. Alarms notify the team when sustained thresholds are breached. Think of a car dashboard: CPU and memory show engine condition, while latency, error rate, and booking success show whether the journey is actually working.

```text
Spring Boot/EC2
  → Actuator + structured logs
  → CloudWatch Agent
  → CloudWatch Logs and Metrics
  → Dashboard and Alarm
  → SNS/on-call notification
```

Important signals:

- Latency, traffic, errors, and saturation
- p50/p95/p99 API latency
- 4xx and 5xx rates
- CPU, memory, disk, network, and JVM/GC pressure
- Database pool saturation and downstream latency
- Business metrics such as booking or payment success

### Actuator configuration

Expose only required endpoints and secure them:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  endpoint:
    health:
      probes:
        enabled: true
      show-details: when_authorized
```

Never expose every actuator endpoint publicly.

### Structured logging

```java
log.info(
        "Device lookup completed deviceId={} plantId={} correlationId={}",
        deviceId,
        plantId,
        correlationId);
```

Do not wrap every repository call in a broad catch merely to re-log the same exception. Log once at the appropriate boundary with context and preserve the cause.

## Incident response

### Interview answer

I first assess customer impact and severity, then compare infrastructure metrics, application error rates, recent deployments, logs, and traces. I use a correlation ID to reconstruct the request across services, stabilize the system through rollback, traffic shift, or feature isolation when necessary, and then complete root-cause analysis and prevention work. The goal is first to restore safe service, then to learn why it failed.

### ELK versus CloudWatch

ELK centralizes, searches, and analyzes logs across services. CloudWatch is AWS-native and combines AWS metrics, logs, alarms, and dashboards. At Cognizant, ELK supported payload and correlation-ID tracing; in AWS, CloudWatch connected application signals with infrastructure health.

## CI/CD

### Interview answer

CI/CD automates integration, testing, packaging, and delivery. At Siemens, GitHub Actions or Jenkins orchestrated the pipeline; Maven built and tested the Java application; Docker packaged it; ECR stored the image; and the AWS runtime deployed it. AWS was the target environment, not automatically the CI/CD tool.

```text
Pull request/merge
  → Maven compile and tests
  → quality/security scans
  → build versioned Docker image
  → push to ECR
  → deploy to QA
  → smoke/health tests
  → approval/UAT
  → controlled production rollout
```

“Provisioning reduced from two hours to under twenty minutes” should mean repeated manual environment/deployment preparation steps were automated. Be ready to identify the exact baseline steps, pipeline steps, measurement source, and whether the number refers to infrastructure provisioning or application deployment.

## Docker

### Interview answer

Docker packages an application, runtime, and dependencies into a portable image. An image is an immutable blueprint; a container is a running instance. Docker solves “works on my machine” by making execution consistent. It is like a sealed shipping container: the contents remain predictable even when the ship or destination changes.

```dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/booking-service.jar app.jar
USER 10001
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Production refinements include a small runtime image, non-root user, versioned immutable tags, vulnerability scanning, resource limits, external configuration, and no embedded secrets.

## Kubernetes

### Interview answer

Kubernetes orchestrates containers by maintaining desired state. A Deployment declares how many application replicas should run; Pods host containers; a Service gives them stable discovery; readiness determines whether a Pod receives traffic; liveness determines whether it should restart. Kubernetes is a fleet manager: Docker packages each vehicle, while Kubernetes schedules, replaces, scales, and routes the fleet.

```text
ECR image → EKS Deployment → Pods → Service → Ingress/Load Balancer
```

### Liveness versus readiness

- Liveness asks: should this process be restarted?
- Readiness asks: should this instance receive traffic now?

Do not make liveness depend on every downstream service; a temporary database failure could otherwise restart every healthy application Pod.

## Blue-green and canary releases

### Interview answer

Blue-green deployment keeps the stable and new environments side by side, validates the new one, and switches traffic when ready; rollback is a traffic switch. Canary deployment sends a small percentage of traffic to the new version and expands exposure only when technical and business metrics remain healthy. Blue-green is like opening a fully prepared replacement terminal, while canary is testing a new process with a small passenger group first.

Monitor health checks, 5xx errors, latency, JVM resources, and critical business flows such as cart, checkout, and payment. Database schema compatibility must also support rollback.

### Follow-ups

- ECS versus EKS versus Lambda?
- ECR versus S3?
- How does IAM least privilege work?
- What would trigger rollback?
- How do you manage secrets?
- What happens when an EC2 instance or Pod fails?
