---
authors: Aube Paul
state: draft - pending approval
---

## RFD 0 - Temporal Workflow to create Auth0 User Identities

## Required Approvers

* Engineering: @russjones , @oeric , @r0mant

## What

This RFD explains utilizing Temporal workflows and python, to create Auth0 tenants, web applications, and user identities.

## Scope of Work

The areas of scope of the project will be the following:

- Create a CLI tool using Python to be able to trigger workflows within Temporal:
    - Create Auth0 user with the following details called `UserRegister` (see implementation plan for more details)
    - Configuring Auth0 tenant
- Securely store API credentials
- Creating a web application utilizing OSS Grafana
    - Docker container will create the Grafana instance and configure it for OIDC authentication with Auth0.
- 

## Tool Kit + Configuration

Development will be conducted using the following OS: Windows 11 Home 24H2 // Ubuntu 24.04.3 LTS (WSL)

- Docker Compose version v2.39.1
- Python 3.13.2
- Terraform 1.12.2
- OSS Grafana 12.1.1

### APIs + Connectors 

- Temporal CLI  Docker: https://hub.docker.com/r/temporalio/temporal?uuid=1BDC3332-BEE1-441D-86A9-21994BF51A57
- Temporal Docker Compose Repo: https://github.com/temporalio/docker-compose
- Auth0 Terraform Provider https://github.com/auth0/terraform-provider-auth0
- Terraform CLI
- Temporal SDK w/ Python
- Grafana Docker image: https://grafana.com/docs/grafana/latest/setup-grafana/configure-docker/

### Infrastructure Configuration

In order to run Temporal locally via Docker Compose stack will be utilized with the following plugins:

- Temporal Server connection - https://github.com/temporalio/docker-compose
- Temporal Worker :Uses python temporal sdk
- Temporal WebUI
- `temporalio/temporal` docker image
    ``` 
    services:
     temporal:
        image: temporalio/temporal:latest
            ports:
             - "7233:7233"
             - "8233:8233"
        command : "server start-dev"
    ```

### Implementation Plan

This document is going based on the assumption that the tools mentioned in the above section have been appropriately installed and configured.

Docker will be creating the following containers:
    - Temporal Workflows
    - OSS Grafana

Will be using the following flow:
-Temporal SDK Python

The following Temporal Workflows will be created:
- `Auth0TenantSetup`
- Terraform will create the following:
    - Define auth0 tenant
    
- `WebAppRegister`
    Terraform will be creating the following:
    - auth0 Web Application that will direct to OSS Grafana portal
    - Authentication Method will be setup utilizing OIDC 
    
- `UserRegister`
    Terraform will be able to create auth0 users with the following information:
    - Email
    - Role
    ``` 
    input command: UserRegister
     --email "persona@xyz.com"
     --role "admin"

    output:
     The following user has been created:
      Username: "persona@xyz.com"
      Role: "Admin" 
    ```


#### Security

The storage of API keys will be held in .env file utilizing .gitignore file for the sake of simplicity for the MVP as well restricting access to local host using Docker. 



