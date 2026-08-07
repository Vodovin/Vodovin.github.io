---
title: 'Distributed Data Center Management System'
description: A microservices-based distributed application to manage virtual instances and networks in a Data Center, featuring asynchronous messaging and container orchestration.
publishDate: 'May 23 2026'
isFeatured: true
seo:
  image:
    src: '../../assets/images/project-3.jpg'
---

![Project preview](../../assets/images/project-3.jpg)

**Project Overview:**
This project consists of a distributed Java-based web application designed to manage the lifecycle of virtual machines (Instances) and private subnets (Networks) within a Data Center (CPD). Developed with a professional approach, the architecture embraces microservices patterns, asynchronous communication, and scalable deployment strategies.

## Design Objectives

The core goal was to build a robust, decoupled, and highly available distributed system. The monolithic approach was discarded in favor of independent services that communicate either via HTTP REST or asynchronously through a message broker, ensuring fault tolerance and scalability.

## Main Features

1. **Microservices Architecture:**
   * The logic is strictly divided into three specialized services: `api-service` (the entry point), `instance-service` (managing VM lifecycles), and `net-service` (handling IP assignments and subnet routing).
   * Implements the **Database-per-Service pattern**, completely isolating the persistence layer: `instance-service` connects exclusively to the Instances database, while `api-service` and `net-service` access the Networks database.

2. **Asynchronous Messaging & Event-Driven Logic:**
   * Utilizes **RabbitMQ** to decouple heavy or inter-dependent tasks.
   * When a new Instance is created, `instance-service` sends a request to the `ip-assign-requests` queue. The `net-service` processes this, assigns an available IP, and replies via `ip-assign-responses` without blocking the main execution thread.
   * Similar asynchronous flows govern network creation and deletion.

3. **High Availability and Load Balancing:**
   * Both `api-service`, `instance-service` and `net-service` are horizontally scaled, running at least two replicas each.
   * A Load Balancer (`HAProxy`) sits in front of the `api-service` replicas to efficiently distribute incoming REST API traffic from clients.

4. **Containerization and DevOps:**
   * The entire ecosystem (custom Java Spring Boot applications, MySQL databases, RabbitMQ, and HAProxy) is fully containerized using **Docker**.
   * Orchestrated via `docker-compose`, enabling seamless deployment of the whole architecture with a single command.
   * Strict security boundaries: internal service ports are completely isolated within the Docker network; only the entry-point load balancer exposes ports to the host machine.

5. **RESTful API Design:**
   * Strict adherence to REST principles, mapping operations to appropriate HTTP methods (GET, POST, PUT, DELETE) and returning standard status codes (e.g., `202 ACCEPTED` for asynchronous requests, `400 Bad Request` for logical errors).

## Technology Stack

- **Backend Framework:** Java, Spring Boot (REST Controllers, Services, Repositories).
- **Message Broker:** RabbitMQ (AMQP protocol) for asynchronous service-to-service communication.
- **Database:** MySQL, utilizing JDBC for efficient data retrieval and storage across isolated databases.
- **DevOps & Infrastructure:** Docker, Docker Compose, HAProxy for load balancing.
- **Version Control & Collaboration:** GitHub, applying professional Git workflows (atomic commits, clear messaging) for team collaboration.

## Test Cases

The code was subjected to rigorous testing using Postman collections to ensure robust functionality and adherence to the specified requirements:

*   **Asynchronous Network Creation and Validation:** Verified that requesting a new network (e.g., ID 1, mask 172.1.0.0) returns a `202 ACCEPTED` status. Subsequent queries confirmed successful asynchronous creation.
*   **Instance Lifecycle and Inter-service Communication:** Created an instance ("db"). Verified its initial status as `PENDING` with no IP. Confirmed that after a short delay, asynchronous IP assignment via RabbitMQ updated the status to `RUNNING` and assigned a valid IP.
*   **Instance Dependency Management:** Created a "web" instance dependent on the "db" instance. Verified that the "web" instance correctly retrieved and displayed the full data of its dependency, demonstrating accurate handling of relationships between instances.
*   **Asynchronous Instance Updates:** Tested updating an instance's hardware specifications (CPU, memory, disk). Verified that the status temporarily changed to `RESTARTING` while a new IP was negotiated asynchronously, eventually returning to `RUNNING` with the updated specs.
*   **Cascading Deletion and Integrity Constraints:** 
    *   Attempted to delete a network with active instances, verifying that the system rejected the operation to maintain integrity.
    *   Attempted to delete the "db" instance while the "web" instance depended on it, verifying that the system correctly returned a `409 Conflict` error.
    *   Verified the successful "soft deletion" of instances (status changed to `DELETED`, IP unassigned, dependencies cleared) when deleted in the correct order ("web" then "db").
    *   Confirmed the successful asynchronous deletion of a network only after all associated instances were removed.