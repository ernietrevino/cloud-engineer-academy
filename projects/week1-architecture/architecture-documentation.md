# Week 1 — Cloud Infrastructure Architecture

## Architecture Overview

This architecture supports a scalable, highly available web application 
using a layered approach. Each layer is separated by function — 
presentation, application logic, data, and storage — so that failures 
are isolated, scaling is targeted, and troubleshooting is surgical.

## Components

### Load Balancer
- **Scaling:** Vertical
- Distributes all incoming traffic across frontend servers
- Prevents overload from traffic spikes like Black Friday or DDoS attacks

### Frontend Servers (x3)
- **Scaling:** Horizontal
- Handle all user-facing requests and web page delivery
- Three instances provide redundancy and high availability
- If one goes down the other two continue serving users

### Backend Servers (x2)
- **Scaling:** Horizontal
- Process all business logic — orders, authentication, payments
- Separated from frontend for fault isolation and targeted scaling
- A backend issue does not take down the user-facing layer

### Primary Database
- **Scaling:** Vertical
- Stores all persistent data — users, orders, products, transactions
- Single source of truth for all application data

### Replica Database
- **Scaling:** Read only
- Synchronized copy of primary database
- Handles read traffic to reduce load on primary
- Deployed in separate AWS Availability Zone for maximum redundancy
- Can be promoted to primary if primary fails

### S3 Storage
- **Scaling:** Automatic
- Stores all large files — images, videos, documents, backups
- Fully decoupled from backend servers
- Cost effective with multiple storage tiers
- Files remain accessible even during server outages

## Architectural Decisions

### Why Horizontal Scaling For Servers?
Adding more frontend and backend instances handles traffic spikes 
flexibly and cost effectively. We only scale what needs scaling — 
if backend is overwhelmed but frontend is fine, we add backend 
servers only.

### Why Separate Frontend and Backend?
Separation means fault isolation. A backend bug doesn't crash the 
website. A frontend issue doesn't corrupt data. Each layer can be 
diagnosed, fixed, and scaled independently.

### Why A Database Replica?
Redundancy and fault tolerance. If the primary database fails, the 
replica takes over with minimal downtime. Multi-AZ deployment means 
even a full data center outage cannot take down both databases.

### Why S3 For File Storage?
Decoupling storage from servers means files survive server crashes. 
S3 is infinitely scalable, cost effective, and secure — with no 
infrastructure to manage.

## Security Measures
- SSL encryption on all user traffic
- Firewall rules between layers
- IAM least privilege policies
- Database not exposed to public internet
- S3 bucket policies restricting public access

## Redundancy Plan
- Every critical layer has minimum two instances
- Database replica in separate Availability Zone
- No single point of failure in the architecture

## Performance Optimization
- Load balancer prevents bottlenecks
- Database replica handles read traffic
- S3 serves static files directly
- Independent scaling per layer