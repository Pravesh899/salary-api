# salary-api

# Purpose
Salary API is a Java based microservice which is responsible for all the salary related transactions and records in OT-Microservices stack. The application is platform independent and can be run on multiple operating system. Java Runtime would be required to run this application.

Supported features of the Salary API are:-

Spring boot based web framework, which uses tomcat as webserver.
ScyllaDB is used as primary database for storing all the salary data.
Redis as cache manager to store the cache response.
Prometheus and Open-telemetry metrics support for monitoring and observability
Swagger integration for the API documentation of endpoints and payloads.
Database migration using the tool called migrate.

# Pre-Requisites

The Salary API application have some database, cache manager and package dependencies. Some of the dependencies are optional and some are mandatory. To compile the application, we only need maven as build tool, but for running the application following things are required:-

ScyllaDB
Redis
Migrate
Maven
Maven will be used as package manager to down# Step1: Installation of software Dependenciesload specific version of dependencies to run the Salary API.

# System Requirements

# Architecture

# Step-by-step installation of [application]

## After creating an instance, first is to update the packages (instance type t2.medium volume 30GB)

sudo apt update

## Clone the salary-api repo in your instance

git clone https://github.com/OT-MICROSERVICES/salary-api.git

# Instalation of prerequisites required for salary-api

## Scylladb Installation and configuration

## Install a repo file and add the ScyllaDB APT repository to your system:

sudo mkdir -p /etc/apt/keyrings

sudo gpg --homedir /tmp --no-default-keyring --keyring /etc/apt/keyrings/scylladb.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys A43E06657BAC99E3

sudo wget -O /etc/apt/sources.list.d/scylla.list http://downloads.scylladb.com/deb/debian/scylla-6.2.list

## Install ScyllaDB packages.

sudo apt-get update

sudo apt-get install -y scylla

