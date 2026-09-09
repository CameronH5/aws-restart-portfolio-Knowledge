# Amazon EventBridge Explained Like You're Five

Imagine your school has a big message board in the hallway. Lots of things happen around school: the bell rings, a teacher posts a note about homework, someone wins a prize, or the lunch menu changes. Each of these is like an "event" – something that happened.

Now, you want different people to know about different events automatically. For example, when the bell rings, the students should go to class. When the lunch menu changes, the cooks should update the kitchen. When a student wins a prize, their parents should get an email. Instead of everyone listening to everything and sorting it out themselves, your school could have a magical mailroom.

This mailroom receives all the event notes, reads them, and sends each note to the right people based on rules you set up. You write a rule like "If the note says 'bell rang', send a copy to all classroom speakers" or "If the note says 'prize won', send an email to the student's parents." The mailroom does this instantly, and you don't have to watch the board yourself.

AWS EventBridge is exactly that magical mailroom for computer programs. It receives **events** (things that happen, like a file being uploaded, a user signing up, a server error) from many sources – your own apps, AWS services, or partner apps (like Zendesk or Datadog). Then it looks at **rules** you define and sends each event to the right **targets** (like a Lambda function to process it, an SNS topic to notify people, or an SQS queue to handle it later). It can even transform the event before sending it, and keep a record for later if needed.

So EventBridge helps different parts of your system talk to each other without being directly connected, making everything more flexible and easier to manage.

---

# Amazon EventBridge: The Complete Technical Guide for Adults

## 1. What is Amazon EventBridge?

Amazon EventBridge is a **serverless event bus service** that makes it easy to connect applications using events. An event bus receives events from various sources and routes them to targets based on rules. It evolved from **Amazon CloudWatch Events** (now part of EventBridge) and extends its capabilities with features like custom event buses, partner event sources, event archive/replay, schema registry, and API destinations.

EventBridge is designed for building **event-driven architectures (EDA)**. It decouples event producers from event consumers, allowing independent scaling and evolution. It is fully managed, highly available, and scales automatically to handle high throughput.

EventBridge supports three types of event buses:
- **Default bus**: Automatically receives events from AWS services (e.g., EC2 state changes, S3 events, CloudTrail API calls).
- **Custom buses**: You create these to receive events from your own applications or other sources.
- **Partner buses**: Receive events from SaaS partners (e.g., Zendesk, Datadog, PagerDuty) via integrations.

## 2. Core Concepts and Architecture

### 2.1 Event
An event is a JSON object that describes a change in state. It has a standard structure:
```json
{
  "version": "0",
  "id": "unique-id",
  "detail-type": "order-placed",
  "source": "my-app.orders",
  "account": "123456789012",
  "time": "2025-01-01T12:00:00Z",
  "region": "us-east-1",
  "resources": [],
  "detail": {
    "orderId": "12345",
    "amount": 100
  }
}
```
Key fields:
- **source**: Identifies the service or application that generated the event.
- **detail-type**: Describes the event kind.
- **detail**: A JSON object with event-specific data.
- Other fields are metadata (version, id, account, time, region, resources).

### 2.2 Event Bus
An event bus is a pipeline that receives events. The default bus receives events from AWS services. Custom buses receive events from your applications via `PutEvents` API. Partner buses receive events from SaaS providers.

### 2.3 Rule
A rule matches incoming events and routes them to targets. A rule has:
- **Event pattern**: A JSON object that specifies which events match. You can match on fields like `source`, `detail-type`, or specific values in `detail`. Patterns support:
  - Exact matching
  - Prefix/suffix matching
  - Numeric ranges
  - Existence checks
  - Logical OR/AND
- **Targets**: One or more destinations for matched events (up to 5 per rule).
- **Event bus**: The bus the rule is associated with.

### 2.4 Targets
A target is a destination where EventBridge sends matched events. Supported targets include:
- AWS Lambda functions
- Amazon SNS topics
- Amazon SQS queues
- Amazon Kinesis Data Streams
- AWS Step Functions state machines
- Amazon ECS tasks (via RunTask)
- Amazon API Gateway endpoints (API destinations)
- Amazon Redshift clusters (via Data API)
- Amazon S3 buckets (via PutObject, but primarily for archive)
- And many more AWS services

### 2.5 Event Transformation
EventBridge can transform an event before delivering to a target using **Input Transformer**. You define a template in JSONPath to extract fields from the original event and create a new payload. This allows targets to receive only the data they need.

### 2.6 Event Archive and Replay
EventBridge can archive events to a central store (S3-based) for a configurable retention period (up to forever). You can then **replay** archived events back onto the same or a different bus, which is useful for recovering from failures or testing new logic against historical data.

### 2.7 Schema Registry
EventBridge automatically discovers schemas from events flowing through your buses and stores them in a **schema registry**. Schemas help you understand event structure, generate code bindings (Java, Python, TypeScript), and validate events. You can also define custom schemas manually.

### 2.8 API Destinations
API destinations allow EventBridge to send events to **external HTTP endpoints** (e.g., third-party APIs) using an HTTP invocation. You define a connection with authentication (OAuth, API key) and then target an API destination from a rule. This enables event-driven integration with SaaS or on-prem systems over HTTPS.

### 2.9 Pipes (EventBridge Pipes)
EventBridge Pipes is a feature that provides a simpler way to create point-to-point integrations between sources and targets, with optional filtering and enrichment. It's a fully managed, serverless integration service that reduces the need for custom Lambda code for simple event pipelines.

## 3. Key Features of AWS EventBridge

- **Serverless and scalable**: No infrastructure to manage; automatically scales with event volume.
- **Multiple bus types**: Default, custom, partner for different event sources.
- **Powerful filtering**: Rich event patterns to match events precisely, reducing unnecessary target invocations.
- **Event transformation**: Input Transformer to reshape events before delivery.
- **Archive and replay**: Store events for later analysis or reprocessing.
- **Schema registry**: Automatically capture schemas for event discovery and code generation.
- **API destinations**: Send events to external HTTP endpoints.
- **Cross-account and cross-region**: Send events to buses in other accounts or regions using resource policies.
- **Integration with AWS services**: Built-in integration with CloudTrail, S3, EC2, etc.
- **Security**: IAM policies, resource-based policies for buses, encryption at rest and in transit, VPC endpoints.
- **SaaS integrations**: Partner event sources bring events from third-party apps.
- **Event replay**: Re-process past events.
- **EventBridge Pipes**: Simplified point-to-point integrations.
- **Global endpoints**: Support for event replication across regions for disaster recovery (recent feature).

## 4. How to Use EventBridge: Typical Workflow

1. **Choose an event bus**: Use the default bus for AWS service events, or create a custom bus for your application events.
2. **Define a rule**: Specify an event pattern to match events, and select one or more targets.
3. **Configure targets**: For each target, set permissions (IAM role) and optional input transformation.
4. **Publish events**: Use `PutEvents` API from your applications, or rely on AWS services that automatically send events to the default bus.
5. **Monitor**: View metrics in CloudWatch, set up DLQ for failed deliveries, and use EventBridge's event history for debugging.
6. **Optionally archive and replay**: Enable archive on a bus, and replay events when needed.

## 5. Use Cases for EventBridge

- **Application integration**: Decouple microservices by having them communicate via events (e.g., order service emits `order-created`, inventory service listens and updates stock).
- **Automation and ops**: React to AWS resource changes (e.g., EC2 instance terminated -> trigger Lambda to clean up resources).
- **SaaS integration**: Ingest events from partner apps (e.g., Zendesk ticket created -> send to SQS for processing).
- **Cross-account event routing**: Central event bus in a hub account receives events from multiple accounts for centralized processing.
- **Event replay for debugging**: Replay past events through new code to test behavior.
- **Schema discovery**: Automatically capture schemas to generate client code and documentation.
- **API orchestration**: Use API destinations to call third-party APIs when events occur (e.g., on new user signup, send data to external CRM).
- **EventBridge Pipes**: Simple data pipelines (e.g., S3 -> Lambda -> SQS) without writing orchestration code.

## 6. What EventBridge Can and Cannot Do

**Can do**:
- Route events from many sources to many targets based on rules.
- Filter events with complex patterns.
- Transform events before delivery.
- Archive and replay events.
- Discover schemas automatically.
- Send events to external HTTP endpoints (API destinations).
- Integrate with SaaS partners.
- Operate cross-account and cross-region.
- Scale automatically to high throughput (thousands of events per second per bus).
- Provide at-least-once delivery (by default; can be exactly-once for some targets? Actually EventBridge provides at-least-once, but for SQS FIFO you can get exactly-once if configured).

**Cannot do**:
- **Guarantee exactly-once delivery** for all targets (it's at-least-once; duplicates possible). Use idempotent targets.
- **Order events** across multiple producers (only best-effort; no total ordering). For ordering, use Kinesis or SQS FIFO.
- **Store events indefinitely** without enabling archive; default bus does not store events after they are routed (archive is optional).
- **Process events with complex business logic**; it's a routing service, not a compute service. You need Lambda or other targets for processing.
- **Replace a full workflow engine**; while it can trigger Step Functions, EventBridge itself doesn't manage stateful workflows.
- **Act as a database**; events are transient unless archived; querying archived events is limited (replay only).
- **Support all AWS services as targets**; only a defined list of services can be direct targets, though you can use Lambda as a bridge to others.
- **Guarantee low latency under all conditions**; typical latency is sub-second but not hard real-time.

## 7. How EventBridge Differs from Other AWS Services

| Service | Primary Use Case | Key Differences from EventBridge |
|---------|------------------|-----------------------------------|
| **Amazon SNS** | Pub/sub messaging with push to many subscribers | SNS is simpler, focuses on fan-out to many endpoints (email, HTTP, SQS, Lambda). EventBridge has richer filtering (patterns), transformation, archive/replay, and many targets. SNS does not have archive/replay or schema registry. |
| **Amazon SQS** | Message queuing for decoupling and buffering | SQS is a queue: messages are pulled and processed by consumers, with exactly-once (FIFO) and dead-letter queues. EventBridge pushes events to targets, does not store them unless archive is enabled, and has no message visibility timeout. SQS is better for workload buffering; EventBridge for event routing. |
| **AWS Step Functions** | Orchestration of multi-step workflows | Step Functions manages state and sequence of steps; EventBridge is stateless event routing. They can be combined: EventBridge triggers Step Functions. |
| **Amazon Kinesis** | Real-time streaming data pipeline | Kinesis is for continuous, ordered, high-volume data streams. EventBridge is for discrete events and routing, not for retaining a long log (unless archive, but replay is not the same as continuous stream). |
| **Amazon CloudWatch Events (legacy)** | Same as EventBridge default bus | CloudWatch Events is now integrated into EventBridge. EventBridge adds custom buses, partner events, archive/replay, schema registry, and API destinations. |
| **AWS AppSync** | GraphQL API for real-time data | AppSync can push events to clients via subscriptions, but it's an API service; EventBridge is a backend event bus. |
| **Amazon EventBridge Pipes** | Point-to-point integration | Pipes is a subset of EventBridge functionality focused on simple source-to-target pipelines with filtering/enrichment, whereas EventBridge buses are more general-purpose with rules and multiple targets. |

## 8. Technical Terms Explained

- **Event**: A JSON record representing a change in state.
- **Event bus**: A central pipeline that receives and routes events.
- **Rule**: A filter and routing configuration that matches events and sends them to targets.
- **Event pattern**: A JSON object defining conditions for matching events.
- **Target**: A destination for matched events.
- **Input Transformer**: A feature that modifies event payload before sending to target.
- **Schema**: The structure of an event (fields and types).
- **Schema registry**: A repository of event schemas.
- **Archive**: A stored copy of events for later replay.
- **Replay**: Re-sending archived events through the bus.
- **API destination**: An HTTP endpoint that EventBridge can call as a target.
- **Connection**: Authentication configuration for API destinations.
- **Partner event source**: A SaaS-provided event source that integrates with EventBridge.
- **EventBridge Pipes**: A feature for simple source-to-target pipelines with optional filtering/enrichment.
- **At-least-once delivery**: Guarantee that events are delivered, but may be delivered more than once.
- **Exactly-once**: Guarantee of single delivery (only available for certain targets like SQS FIFO when using EventBridge Pipes? Actually EventBridge itself is at-least-once).
- **DLQ (Dead Letter Queue)**: A queue for events that failed to be delivered to a target.

## 9. Best Practices for Using EventBridge

1. **Design for idempotency**: Since delivery is at-least-once, make targets idempotent to handle duplicates.
2. **Use specific event patterns** to avoid over-matching and unnecessary invocations.
3. **Set up DLQs for targets** to capture failed deliveries for reprocessing.
4. **Enable archive on important buses** for replay and debugging.
5. **Use schema registry** to document and validate events.
6. **Use Input Transformer** to shape events for targets, reducing custom code.
7. **Monitor with CloudWatch metrics** (Invocations, FailedInvocations, ThrottledRules) and set alarms.
8. **For high-throughput, consider event size limits** (max 256 KB per event) and batch where possible.
9. **Use resource policies** on custom buses to control cross-account access.
10. **Leverage EventBridge Pipes** for simple integrations instead of writing Lambda.
11. **Use API destinations with proper authentication** and VPC endpoints for security.
12. **Test replay** to ensure that reprocessing does not cause side effects.

## 10. Current Limitations and Considerations (As of 2025)

- **Event size limit**: 256 KB per event. Larger payloads need to be stored in S3 and referenced.
- **No built-in ordering**: Events are not guaranteed in order; for ordered processing use Kinesis or SQS FIFO.
- **At-least-once delivery**: Duplicates can occur; consumers must be idempotent.
- **Default bus limit**: 300 rules per region per account (can be increased). Custom bus limits vary.
- **Target delivery latency**: Typically under a second, but not guaranteed for real-time.
- **Archive costs**: Storing archived events incurs S3 charges.
- **Replay limitations**: Replay may not preserve original order perfectly and can cause duplicate processing.
- **API destinations**: Limited to HTTP/HTTPS, no support for other protocols.
- **Partner events**: Limited to listed partners; custom SaaS integration requires using API destinations or custom code.
- **VPC support**: EventBridge buses are not inside your VPC, but you can use VPC endpoints to call them privately.
- **Cross-region routing**: Global endpoints feature is relatively new and may have limitations.

## 11. Summary

Amazon EventBridge is a powerful serverless event bus that enables event-driven architectures on AWS. It provides robust routing with filtering, transformation, archive/replay, schema discovery, and integration with many AWS services and SaaS partners. Compared to SNS and SQS, EventBridge offers richer event matching and more target options, making it ideal for connecting decoupled services and automating responses to changes. For certification, focus on its key features: event buses (default, custom, partner), rules and patterns, targets, input transformers, archive/replay, schema registry, and differences from SNS/SQS. Understand its role in building modern, scalable applications.
