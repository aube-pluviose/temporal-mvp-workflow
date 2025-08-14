---
authors: Aube Paul
state: draft - pending approval
---


RFD 0 - Temporal Workflow to create auth0 User Identities
------

## Required Approvers

* Engineering: @russjones , @oeric , @r0mant


## What

This RFD explains utilizing Temporal workflows and python, to create Auth0 tenants, web applications, and user identities. 

## Scope of Work

The areas of scope of the project will be the following:
- CLI tool creation to be able to trigger Temporal workflows
- Secrets storage of API credentials 
- Test user is created in auth0
- Product Upkeep
- Creating a web application utilizing OSS Grafana



## Tool Kit + Configuration

Development will be conducted using the following OS: Windows 11 Home 24H2 // Ubuntu 24.04.3 LTS (WSL)

Docker Compose version v2.39.1
Python 3.13.6 
Terraform 1.12.2

### APIs
- Temporal Auto-Setup: https://hub.docker.com/r/temporalio/auto-setup
- Temporal CLI  Docker: https://hub.docker.com/r/temporalio/temporal?uuid=1BDC3332-BEE1-441D-86A9-21994BF51A57
- Temporal Docker Compose Repo: https://github.com/temporalio/docker-compose
- PostgreSQL
- Auth0 to manage with Terraform
- Terraform CLI
- Temporal SDK w/ Python

###  Infrastructure Configuration

In order to run Temporal locally via Docker Compose stack will be utilized with the following plugins:
- Temporal Server connection - https://github.com/temporalio/docker-compose
- Temporal Worker :Uses python temporal sdk
- Temporal WebUI
- PostgreSQL - database to store Temporal info


### Implementation Plan

This document is going based on the assumption that the tools mentioned in the above section have been appropriately installed and configured. 

Will be using the following flow:
-Temporal SDK Python
The following Temporal Workflows will be created:
- Auth0TenantSetupWorkflow - https://auth0.com/docs/get-started/auth0-overview/create-tenants
- Terraform will create the auth0 session
- WebAppRegistrationWorkflow
- Auth0 will create a web application
- Configure OIDC and OSS Grafana (based off of OIDC info)
- UserRegistrationWorkflow


#### Security 

The storage of API keys will be held in .env file for the sake of simplicity for the MVP, for a full scale production environment would utilize a secret management solution such as Vault. Please see draft below of security proposal that would be added as part of implementation for Prod:
- Auth0, API credentials will load from Vault for a more secure management
- Enable MFA
- Least Privilege access for auth0 users
- Terraform information would be stored in a secure backend (Terraform Cloud for feasibility and ease of access for remote engineers)


