# salary-api

# Purpose
Salary API is a Java based microservice which is responsible for all the salary related transactions and records in OT-Microservices stack. The application is platform independent and can be run on multiple operating system. Java Runtime would be required to run this application.

Supported features of the Salary API are:

Spring boot based web framework, which uses tomcat as webserver.

ScyllaDB is used as primary database for storing all the salary data.

Redis as cache manager to store the cache response

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

### Clone the salary-api repo in your instance

git clone https://github.com/OT-MICROSERVICES/salary-api.git

# Instalation of prerequisites required for salary-api

## Scylladb Installation and configuration

### Install a repo file and add the ScyllaDB APT repository to your system:

sudo mkdir -p /etc/apt/keyrings

sudo gpg --homedir /tmp --no-default-keyring --keyring /etc/apt/keyrings/scylladb.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys A43E06657BAC99E3

sudo wget -O /etc/apt/sources.list.d/scylla.list http://downloads.scylladb.com/deb/debian/scylla-6.2.list

### Install ScyllaDB packages.

sudo apt-get update

sudo apt-get install -y scylla

### Configure I/O settings for ScyllaDB on your VM

sudo /opt/scylladb/scripts/scylla_io_setup

### Update configuration file of scylla

sudo vi /etc/scylla/scylla.yaml

#### Added the below lines in the config file:

authenticator: PasswordAuthenticator
authorizer: CassandraAuthorizer

#### Updated the rpc_address with server's private IP:

rpc_address: 192.168.0.96

### Restart the scylla-server service and check the status

sudo systemctl restart scylla-server.service

sudo systemctl status scylla-server

### Used below command to get into scylladb

cqlsh 192.168.0.96 9042 -u cassandra -p cassandra

### Created user ‘scylladb’ with password as ‘password

CREATE USER scylladb WITH PASSWORD 'password' SUPERUSER;

### Created keyspace employee_db

CREATE KEYSPACE employee_db WITH REPLICATION = { 'class': 'SimpleStrategy', 'replication_factor': 1 };

### Verify the empolyee_db

DESCRIBE KEYSPACES;


# Redis Installation and Configuration

sudo apt update
sudo apt install redis-server -y

## Configuration of redis : Enter into redis

redis-cli

### Configure user permissions and authentication settings in redis

ACL SETUSER scylla on >password ~* +@all

### List the acl

ACL LIST

## Install Maven & Java depndancy

sudo apt install openjdk-17-jre

sudo apt install maven -y

## Instalation of swagger

sudo apt  install jq -y

download_url=$(curl -s https://api.github.com/repos/go-swagger/go-swagger/releases/latest | \jq -r '.assets[] | select(.name | contains("'"$(uname | tr '[:upper:]' '[:lower:]')"'_amd64")) | .browser_download_url')

sudo curl -o /usr/local/bin/swagger -L'#' "$download_url"

sudo chmod +x /usr/local/bin/swagger

## Enter into salary-api directory

cd salary-api/

### update the contact-points & host in application.yaml to your private ip address from below paths

sudo vi src/main/resources/application.yml

sudo vi src/test/resources/application.yml

### Create the service file for salary-api service

sudo vi /etc/systemd/system/salary-api.service

### Content of salary-api service:
"""
[Unit]

Description=salary api
After=network.target

[Service]

Type=simple
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/salary-api/
ExecStart=java -jar /home/ubuntu/salary-api/target/salary-0.1.0-RELEASE.jar --server.port=8081
Restart=on-failure
RestartSec=5
StandardOutput=journal
StandardError=journal

[Install]

WantedBy=multi-user.target

"""
### Enable and start the salary-api service

sudo systemctl enable salary-api.service
sudo systemctl start salary-api.service

### Restart the salary-api service
sudo systemctl restart salary-api.service

### 
# Contact Information
