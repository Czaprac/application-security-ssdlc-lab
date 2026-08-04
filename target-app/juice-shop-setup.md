# OWASP Juice Shop Setup

This document describes the target application setup for my Application Security / SSDLC learning lab.

## Purpose

The purpose of this step was to prepare a local, deliberately vulnerable web application that can be used for security testing practice.

The target application is OWASP Juice Shop.

## Environment

- Host system: Windows
- Container platform: Docker Desktop
- Target application: OWASP Juice Shop
- Docker image: bkimminich/juice-shop:latest
- Container name: juice-shop
- Local URL: http://127.0.0.1:3000
- Exposure: localhost only

## Docker Verification

Before running the target application, Docker was verified with:

```cmd
docker run hello-world
```

The command completed successfully and confirmed that Docker was working correctly.

## Running Juice Shop

OWASP Juice Shop was started with the following command:

```cmd
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

## Container Status Check

The running container was verified with:

```cmd
docker ps
```

Observed result:

```text
IMAGE                         STATUS         PORTS                    NAMES
bkimminich/juice-shop          Up             127.0.0.1:3000->3000/tcp juice-shop
```

## Docker Image Check

The downloaded Docker image was verified with:

```cmd
docker images
```

Observed image:

```text
bkimminich/juice-shop:latest
```

## Browser Access

The application was opened locally in a browser:

```text
http://127.0.0.1:3000
```

The application loaded successfully.

## Scope Note

This application is intentionally vulnerable and is used only in a local lab environment.

Testing is limited to the local instance running on `127.0.0.1`.

No public websites, third-party systems, or production applications are included in the scope of this lab.

## Current Status

Completed:

- Docker Desktop installed
- Docker engine verified
- OWASP Juice Shop container started
- Local access confirmed
- Target application scope defined

Next step:

- Perform a basic DAST scan against the local Juice Shop instance using OWASP ZAP
