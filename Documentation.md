# Salary API Documentation

| **Author** | **Created on** | **Last updated by** | **Last edited on** | **Reviwer** |
|------------|----------------|----------------------|---------------------|---------------|
| Pravesh Kumar      | 12-11-24      | Pravesh Kumar             | 12-11-24           | Nikhil |

## Salary-API overview

The Salary API is a core component of the OT-Microservices project, providing functionality to handle employee salary-related transactions and information. It operates as a standalone microservice within a distributed system, allowing seamless integration with other services like Employee and Attendance APIs. Designed to meet high-performance, reliability, and scalability requirements, the Salary API is optimized for cloud environments and uses modern development and deployment practices.

## Purpose of the Salary API

### The primary purpose of the Salary API is to:

- Manage and process salary records for employees.

- Integrate easily with other microservices within the OT-Microservices ecosystem.
  
- Support efficient data storage, retrieval, and calculation related to employee compensation.
  
- It is essential in automating payroll processes, enhancing accuracy in employee compensation records, and simplifying the financial operations within the organization.

### Supported features of the Salary API are:-

- Spring boot based web framework, which uses tomcat as webserver.
- ScyllaDB is used as primary database for storing all the salary data.
- Redis as cache manager to store the cache response.
- Prometheus and Open-telemetry metrics support for monitoring and observability
- Swagger integration for the API documentation of endpoints and payloads.
- Database migration using the tool called migrate.

### Components of the Salary API

- [ScyllaDB](https://www.scylladb.com/)

- [Redis](https://redis.io/)

- [Migrate](https://github.com/golang-migrate/migrate)

- [Maven](https://maven.apache.org/)



