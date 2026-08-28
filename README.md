# Smart Campus Help Desk

## Overview

**Smart Campus Help Desk** is an ASP.NET Core web application developed as part of an **AZ-400 – Designing and Implementing Microsoft DevOps Solutions** practical project.

The project demonstrates how DevOps practices can be used to manage an application from development through Continuous Integration (CI), deployment, validation, production release, and rollback.

The main deployment strategy used in this project is **Blue-Green Deployment**, which allows a new version of the application to be tested separately before becoming the production version.

---

## Project Objectives

The project demonstrates the following DevOps workflow:

**Plan → Develop → Commit → Review → Build → Test → Deploy → Validate → Switch → Rollback**

The main objectives are to:

* Manage project work using Azure Boards
* Store source code using Git and Azure Repos
* Use feature branches for development
* Implement Pull Requests and code reviews
* Create a Continuous Integration pipeline
* Automatically Restore, Build, Test and Publish the application
* Deploy Version 1.0 to the Blue environment
* Develop and deploy Version 2.0 to the Green environment
* Validate Version 2.0 before production
* Perform a Blue-Green production switch
* Demonstrate rollback to Version 1.0

---

## Technologies Used

* **ASP.NET Core**
* **C#**
* **Git**
* **Azure DevOps**
* **Azure Boards**
* **Azure Repos**
* **Azure Pipelines**
* **Microsoft Azure**

---

## Application

The Smart Campus Help Desk allows students to submit technical support requests.

### Support Request Fields

The application captures:

* Student Name
* Issue Category
* Description
* Status
* Date

### Issue Categories

Users can select from categories such as:

* Network
* Computer
* Software
* Account
* Other

The application also allows submitted support requests to be viewed.

---

# DevOps Architecture

```text
Azure Boards
     ↓
Azure Repos
     ↓
Feature Branch
     ↓
Pull Request & Review
     ↓
Continuous Integration
     ↓
Restore → Build → Test → Publish
     ↓
Blue Environment
Version 1.0
     ↓
Develop Version 2.0
     ↓
Green Environment
Version 2.0
     ↓
Validate Green
     ↓
Approve Release
     ↓
Blue-Green Switch
     ↓
Green Environment
Version 2.0 Production
     ↓
Rollback when required
     ↓
Blue Environment
Version 1.0
```

---

# Branching Strategy

The project uses the following Git branches:

```text
main

HomeTest
blue
green
dev1
dev2
dev3
dev4/*
```

### `main`

Contains the production-ready application.


# Version 1.0 – Blue Environment

Version **1.0** represents the stable production release.

The application displays:

```text
Application Version: 1.0
Environment: BLUE
```

The initial deployment architecture is:

```text
Users
  ↓
Blue Environment
  ↓
Version 1.0
```

The Blue environment remains available even after Version 2.0 is released so that it can be used for rollback.

---

# Continuous Integration

The Azure Pipelines CI process follows these stages:

```text
Restore
   ↓
Build
   ↓
Test
   ↓
Publish
```

A successful pipeline produces a deployable application artifact.

The CI pipeline should prevent an unsuccessful build or test from progressing towards deployment.

---

# Version 2.0 – Green Environment

Version **2.0** introduces an identifiable improvement to the Smart Campus Help Desk application.

The application displays:

```text
Application Version: 2.0
Environment: GREEN
```

Version 2.0 is deployed separately from Version 1.0.

Before the production switch:

```text
BLUE  → Version 1.0 → Current Production

GREEN → Version 2.0 → Candidate Release
```

This allows Version 2.0 to be tested without affecting the current production application.

---

# Blue-Green Deployment

Blue-Green deployment uses two separate environments.

### Blue

The Blue environment contains the current stable production version.

```text
BLUE → Version 1.0
```

### Green

The Green environment contains the new candidate version.

```text
GREEN → Version 2.0
```

After Version 2.0 has passed validation, production traffic is switched to Green.

```text
Before:

Users → BLUE → Version 1.0
        GREEN → Version 2.0


After:

Users → GREEN → Version 2.0
        BLUE → Version 1.0
```

The Blue environment is kept available to support rollback.

---

# Validation

Before Version 2.0 becomes production, the Green environment is tested.

The following areas are validated:

| Test                           | Expected Result |
| ------------------------------ | --------------- |
| Application starts             | Pass            |
| Homepage loads                 | Pass            |
| Report Issue functionality     | Pass            |
| Submitted issues can be viewed | Pass            |
| Application version            | 2.0             |
| Environment                    | GREEN           |

Version 2.0 should not be promoted if a critical validation test fails.

---

# Rollback Strategy

If a critical problem is discovered after Version 2.0 becomes production, the application can be switched back to the previous stable environment.

```text
Green Version 2.0
        ↓
Critical Problem Identified
        ↓
Rollback
        ↓
Blue Version 1.0
```

After rollback:

```text
Users → BLUE → Version 1.0
```

Keeping Version 1.0 available makes rollback faster and reduces potential application downtime.

---

# CI Pipeline Failure Test

As part of the practical project, a controlled development error is introduced to demonstrate pipeline troubleshooting.

The process is:

```text
Introduce Error
      ↓
Run CI Pipeline
      ↓
Pipeline Fails
      ↓
Check Pipeline Logs
      ↓
Identify Error
      ↓
Correct Error
      ↓
Commit Correction
      ↓
Run Pipeline Again
      ↓
Pipeline Succeeds
```

The error should only be introduced in the practical development environment and must not affect production.

---

# Azure DevOps

The Azure DevOps project is named:

```text
SmartCampus-DevOps
```

Azure Boards is used to manage:

* Epic
* Feature
* User Stories
* Tasks

Azure Repos is used to store and manage the application's source code.

Azure Pipelines is used to automate the Continuous Integration process.

---

# Getting Started

## Prerequisites

Before running the application, make sure the following are installed:

* .NET SDK
* Visual Studio or Visual Studio Code
* Git
* An Azure DevOps account

## Clone the Repository

```bash
git clone <YOUR-AZURE-REPOS-URL>
```

Navigate into the project:

```bash
cd SmartCampusHelpDesk
```

## Restore Dependencies

```bash
dotnet restore
```

## Build the Application

```bash
dotnet build
```

## Run the Application

```bash
dotnet run
```

Open the URL provided by ASP.NET Core in your browser.

---

# Testing

Run the automated tests using:

```bash
dotnet test
```

The application should successfully build and pass its configured tests before deployment.

---

# Deployment Process

The overall deployment process is:

### 1. Develop

Create a feature branch and implement the required change.

### 2. Commit

Commit the changes using Git.

```bash
git add .
git commit -m "Add Version 2 improvements"
```

### 3. Push

```bash
git push origin feature/version-2
```

### 4. Pull Request

Create a Pull Request from the feature branch into `develop`.

### 5. CI

The pipeline performs:

```text
Restore → Build → Test → Publish
```

### 6. Deploy

Deploy the application to the appropriate environment.

### 7. Validate

Test the Green environment before production.

### 8. Switch

Move production from Blue to Green.

### 9. Rollback

If required, switch production back to Blue.

---

# Project Evidence

The practical project requires evidence of the following:

* Azure Boards project planning
* Azure Repos repository
* Git branches
* Completed Pull Request
* Failed CI pipeline
* Successful CI pipeline
* Version 1.0 in Blue
* Version 2.0 in Green
* Green environment validation
* Blue-Green production switch
* Successful rollback

Screenshots should be clearly labelled and accompanied by a short explanation.

---

# Project Structure

A typical ASP.NET Core project structure may look like:

```text
SmartCampusHelpDesk/
│
├── Controllers/
├── Models/
├── Views/
├── wwwroot/
│
├── Properties/
│
├── Program.cs
├── appsettings.json
├── appsettings.Development.json
│
├── SmartCampusHelpDesk.csproj
└── README.md
```

The exact structure may differ depending on the implementation.

---

# Learning Outcomes

This project demonstrates an understanding of:

* Source control management
* Git branching
* Pull Requests
* Code review
* Continuous Integration
* Automated builds
* Automated testing
* Application artifacts
* Controlled deployment
* Blue-Green deployment
* Production validation
* Deployment rollback
* Azure DevOps

---

# Conclusion

The Smart Campus Help Desk project demonstrates how Azure DevOps and DevOps practices can be used to manage application changes safely from planning through production deployment.

The Blue-Green deployment strategy allows Version 2.0 to be deployed and validated independently while Version 1.0 remains available as the stable production version. This provides a safer release process and allows the application to be quickly rolled back if a critical problem occurs.
