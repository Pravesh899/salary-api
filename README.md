# salary-api

## Purpose
Salary API is a Java based microservice which is responsible for all the salary related transactions and records in OT-Microservices stack. The application is platform independent and can be run on multiple operating system. Java Runtime would be required to run this application.

Supported features of the Salary API are:

Spring boot based web framework, which uses tomcat as webserver.

ScyllaDB is used as primary database for storing all the salary data.

Redis as cache manager to store the cache response

Prometheus and Open-telemetry metrics support for monitoring and observability

Swagger integration for the API documentation of endpoints and payloads.

Database migration using the tool called migrate.

## Pre-Requisites

The Salary API application have some database, cache manager and package dependencies. Some of the dependencies are optional and some are mandatory. To compile the application, we only need maven as build tool, but for running the application following things are required:-

ScyllaDB

Redis

Migrate

Maven

Maven will be used as package manager to down# Step1: Installation of software Dependenciesload specific version of dependencies to run the Salary API.

## System Requirements

Processor	dual-core

RAM	4GB

Disk	20GB

OS	Ubuntu(22.04)

## Architecture

![Screenshot 2024-11-12 at 12 31 28 AM](https://github.com/user-attachments/assets/5e165de8-db61-4c61-a23d-c67027a0988e)

# Step-by-step installation of [application]

## After creating an instance, first is to update the packages (instance type t2.medium volume 30GB)

sudo apt update

### Clone the salary-api repo in your instance

git clone https://github.com/OT-MICROSERVICES/salary-api.git

![Screenshot 2024-11-11 at 10 31 42 PM](https://github.com/user-attachments/assets/37c08836-cf5d-4dc3-bf9f-99f5b2abe207)

# Instalation of prerequisites required for salary-api

## Scylladb Installation and configuration

### Install a repo file and add the ScyllaDB APT repository to your system:

sudo mkdir -p /etc/apt/keyrings

sudo gpg --homedir /tmp --no-default-keyring --keyring /etc/apt/keyrings/scylladb.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys A43E06657BAC99E3

sudo wget -O /etc/apt/sources.list.d/scylla.list http://downloads.scylladb.com/deb/debian/scylla-6.2.list

![Screenshot 2024-11-11 at 10 32 02 PM](https://github.com/user-attachments/assets/3a882821-e761-47e4-bfba-851539d93080)

### Install ScyllaDB packages.

sudo apt-get update

sudo apt-get install -y scylla

![Screenshot 2024-11-11 at 10 32 57 PM](https://github.com/user-attachments/assets/7226ce40-aabc-4f48-a682-878988c8ebed)

### Configure I/O settings for ScyllaDB on your VM

sudo /opt/scylladb/scripts/scylla_io_setup

![Screenshot 2024-11-11 at 10 33 25 PM](https://github.com/user-attachments/assets/c495a23a-fcf1-447f-a605-18957ceda9d9)

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

![Screenshot 2024-11-11 at 10 45 27 PM](https://github.com/user-attachments/assets/516b67a6-c2eb-4ca9-94e9-c24d7fb1d7b5)

### Used below command to get into scylladb

cqlsh 192.168.0.96 9042 -u cassandra -p cassandra

#### Created user ‘scylladb’ with password as ‘password

CREATE USER scylladb WITH PASSWORD 'password' SUPERUSER;

#### Created keyspace employee_db

CREATE KEYSPACE employee_db WITH REPLICATION = { 'class': 'SimpleStrategy', 'replication_factor': 1 };

#### Verify the empolyee_db

DESCRIBE KEYSPACES;

![Screenshot 2024-11-11 at 10 45 42 PM](https://github.com/user-attachments/assets/21686683-9817-40e8-9b24-117bb7f26ede)

# Redis Installation and Configuration

sudo apt update
sudo apt install redis-server -y

![Screenshot 2024-11-11 at 11 10 37 PM](https://github.com/user-attachments/assets/4dba6c46-3416-4015-941f-e24468f128c0)

## Configuration of redis : Enter into redis

redis-cli

#### Configure user permissions and authentication settings in redis

ACL SETUSER scylla on >password ~* +@all

#### List the acl

ACL LIST


### Update the redis config fike
sudo vi /etc/redis/redis.conf

#### update with private IP "bind 127.0.0.1 192.168.0.101"

sudo systemctl restart redis
redis-cli -h 192.168.0.101 -p 6379 ping

## Install Maven & Java depndancy

sudo apt install openjdk-17-jre

sudo apt install maven -y

![Screenshot 2024-11-11 at 11 16 34 PM](https://github.com/user-attachments/assets/b6d0eba1-636a-48a2-9104-2ede131c7f97)

## Instalation of swagger

sudo apt  install jq -y
![Screenshot 2024-11-11 at 11 40 58 PM](https://github.com/user-attachments/assets/c1321697-f6c8-48ed-b982-4a67fbc35bd4)

download_url=$(curl -s https://api.github.com/repos/go-swagger/go-swagger/releases/latest | \jq -r '.assets[] | select(.name | contains("'"$(uname | tr '[:upper:]' '[:lower:]')"'_amd64")) | .browser_download_url')

sudo curl -o /usr/local/bin/swagger -L'#' "$download_url"

sudo chmod +x /usr/local/bin/swagger

![Screenshot 2024-11-11 at 11 41 22 PM](https://github.com/user-attachments/assets/65bc8d96-40d0-4904-a4d6-cce0100d19ac)

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

## Install migration tool

### Download the zip file of the migration tool(migrate)
curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.2/migrate.linux-amd64.tar.gz | tar xvz

![Screenshot 2024-11-11 at 11 41 47 PM](https://github.com/user-attachments/assets/7820ab8d-81cd-4592-a814-1eef79804cc3)

### Move the file to below location
sudo mv migrate /usr/local/bin/migrate

### Check the version of migrate
migrate --version

### update below config files with public IP:
cd /salary-api

sudo vi src/main/java/com/opstree/microservice/salary/config/OpenAPIConfig.java 

//
devServer.setUrl("http://18.212.99.151:8080");
//

sudo vi src/test/java/com/opstree/microservice/salary/config/OpenAPIConfigTests.java 

//
assertEquals("http://18.212.99.151:8080", server.getUrl());
//

## Update the migration.json with private IP

sudo vi migration.json

## run the clean package inside the salary-api directory

mvn clean package
![Screenshot 2024-11-11 at 11 45 01 PM](https://github.com/user-attachments/assets/c35b4369-96c8-4658-b3ba-0ff202da5c01)
![Screenshot 2024-11-11 at 11 45 12 PM](https://github.com/user-attachments/assets/200cd88b-f05f-4fa5-b62b-ee652faf1de7)


### Install make command

sudo apt install make

## Run the migration command

make run-migrations

## Run the java runtime command

java -jar target/salary-0.1.0-RELEASE.jar

## Output

![Screenshot 2024-11-11 at 11 52 13 PM](https://github.com/user-attachments/assets/a990c12e-2c05-4765-b7c3-86900693e22e)

## Error ##

### Port is already is in use

sudo lsof -i :8080

sudo kill -9 <pid>

Run the command again "java -jar target/salary-0.1.0-RELEASE.jar"


# Contact Information
