# ErnieMart — Architecture Comparison

## Traditional vs Serverless Analysis

## Cost Model

### Traditional
Servers run 24/7 regardless of traffic. At 3am with zero
visitors you still pay full server costs. Predictable
monthly bill but includes significant idle cost.

### Serverless
Pay only when code runs. Zero traffic means zero compute
cost. On Black Friday scales to 500,000 users at a
fraction of traditional cost.

## Scaling Behavior

### Traditional
Auto Scaling Groups add servers when CPU thresholds are
crossed. Takes 2-5 minutes to spin up new instances.
Requires pre-configured scaling policies.

### Serverless
Scales instantly from 0 to thousands of concurrent
executions in seconds. No pre-configuration needed.
AWS manages all scaling automatically.

## Operational Overhead

### Traditional
Engineers manage server provisioning, OS patching,
security updates, database backups, and capacity
planning. Requires dedicated DevOps resources.

### Serverless
AWS handles all infrastructure. Small teams move fast
without infrastructure overhead. Engineers focus on
building ErnieMart features instead of managing servers.

## Limitations

### Traditional Limitations
- Higher idle costs
- Slower scaling response time
- More operational burden
- Requires capacity planning

### Serverless Limitations
- Cold starts cause brief delays after idle periods
- 15-minute Lambda timeout — not suitable for long tasks
- Vendor lock-in makes switching providers difficult
- Harder to debug and test locally

## When To Choose Each

### Choose Traditional When
- Traffic is constant and high
- Long-running processes exceed Lambda limits
- Team has deep infrastructure expertise
- Avoiding vendor lock-in is a priority

### Choose Serverless When
- Traffic is variable or unpredictable
- Small team needs to move fast
- Event-driven workflows are central to the app
- Time to market is critical

## Recommendation For ErnieMart
Start with serverless. Reduced operational burden lets
a small team move fast. Cost model matches variable
shopping traffic. Automatic scaling handles Black Friday
without manual intervention. As traffic grows to a
consistently high baseline, evaluate migrating core
browsing to traditional servers for better cost
efficiency while keeping event-driven workflows serverless.