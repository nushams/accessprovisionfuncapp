# User Management Function App

This application is built using .NET and leverages Azure Functions to automate user group management based on specific criteria. The application includes both HTTP and timer-triggered functions to manage user access efficiently.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Setup](#setup)
- [Configuration](#configuration)
- [Usage](#usage)
- [Security](#security)
- [Monitoring](#monitoring)

## Overview

The User Management Function App is designed to handle user access to a specific group (e.g., LAN or cloud group) by automating the process of creating add/remove requests in ServiceNow and managing approvals. It performs the following tasks:

- Adds users to the group based on criteria.
- Sends a welcome email and grants owner access.
- Periodically checks if users still meet the criteria and removes those who don't.

## Architecture

The application consists of two main components:

1. **HTTP Triggered Function**: Activated by a user clicking a button on a website.
   - Authenticates the user via SSO.
   - Checks if the user is a full-time employee, on a regular schedule, and belongs to the IT department.
   - Creates a request in ServiceNow to add the user to the group.
   - Sends a welcome email and grants owner access upon approval.

2. **Timer Triggered Function**: Runs weekly.
   - Scans the group to ensure all members still meet the criteria.
   - Creates a request in ServiceNow to remove users who no longer qualify, pending approval from the group owner.

![Architecture Diagram](docs\architecturediagram.png)

## Features

- **Automated User Management**: Automatically adds and removes users based on predefined criteria.
- **Integration with ServiceNow**: Creates and manages approval requests in ServiceNow.
- **Email Notifications**: Sends emails to users when they are added to the group.
- **Secure Configuration**: Utilizes secure storage for secrets and keys.
- **Monitoring and Logging**: Integrated with Azure Application Insights for monitoring and logging.

## Setup

### Prerequisites

- .NET SDK
- Azure Subscription
- ServiceNow Instance
- Email Service Configuration

## Configuration

### Azure Function App Settings

- **App Settings**: Configure the following settings in the Azure Function App configuration.

  | Setting Name           | Description                        |
  |------------------------|------------------------------------|
  | `ClientId`             | SSO Client ID                      |
  | `ClientSecret`         | SSO Client Secret                  |
  | `ServiceNowInstance`   | ServiceNow Instance URL            |
  | `ServiceNowUsername`   | ServiceNow API Username            |
  | `ServiceNowPassword`   | ServiceNow API Password            |
  | `EmailServiceApiKey`   | API Key for the email service      |

- **Environment Secrets**: Store sensitive information in GitHub Secrets or Azure Key Vault.

### Application Insights

- **Instrumentation Key**: Add the Application Insights instrumentation key to monitor the function app.

## Usage

### HTTP Triggered Function

- **Endpoint**: `/api/addUserToGroup`
- **Method**: POST
- **Payload**: User details (automatically retrieved via SSO).

### Timer Triggered Function

- **Schedule**: Every week (configurable via CRON expression).
- **Action**: Scans the group and removes users who no longer meet the criteria.

## Security

- **Secrets Management**: Store secrets in secure locations such as GitHub Secrets or Azure Key Vault.
- **Authentication**: Ensure all endpoints are secured and authenticated using industry best practices.

## Monitoring

- **Application Insights**: Monitor the function app for performance, errors, and usage.
  - Configure alerts and dashboards in Azure Portal.
