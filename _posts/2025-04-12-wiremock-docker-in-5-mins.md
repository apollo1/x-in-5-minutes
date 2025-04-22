---
layout: posts
title: "Setting up a WireMock Docker Container in 5 Minutes"
date: 2025-04-12
tags: [wiremock, docker, api, testing]
canonical_url: https://yourdomain.com/wiremock-in-5-mins/
---

Need a quick and dependable mock server for your HTTP APIs? **WireMock** is a powerful tool for mocking REST APIs—perfect for development, testing, and even contract-driven workflows. In this 5-minute guide, you’ll learn how to spin up WireMock in a Docker container, configure it with static stubs, and have it serving fake responses in no time.

---

## What You’ll Get

By the end of this guide, you’ll have:

- A **running WireMock container**  
- A stubbed endpoint responding to a predefined request  
- A simple folder structure to extend your mock API setup

---

## Prerequisites

- Docker installed and running  
- Basic knowledge of HTTP APIs and JSON

---

## Step 1: Create a Project Directory

```bash
mkdir wiremock-demo
cd wiremock-demo
mkdir -p mappings __files
```

WireMock expects stub mappings to live in a `/mappings` directory and static response bodies in `/__files`.

---

## Step 2: Define a Mapping

Inside `mappings`, create a file called `hello-world.json`:

```json
{
  "request": {
    "method": "GET",
    "url": "/hello"
  },
  "response": {
    "status": 200,
    "body": "Hello, WireMock!",
    "headers": {
      "Content-Type": "text/plain"
    }
  }
}
```

This stub tells WireMock to respond with `"Hello, WireMock!"` when a `GET /hello` request is received.

---

## Step 3: Run WireMock with Docker

Run the container, mounting your local `mappings` and `__files` directories:

```bash
docker run --rm -it -p 8080:8080   -v "$PWD/mappings:/home/wiremock/mappings"   -v "$PWD/__files:/home/wiremock/__files"   wiremock/wiremock:latest
```

WireMock will now be available at [http://localhost:8080](http://localhost:8080).

---

## Step 4: Test It

In a separate terminal, run:

```bash
curl http://localhost:8080/hello
```

You should get:

```
Hello, WireMock!
```

---

## Bonus: Simulate Delays or Errors

WireMock supports delays, headers, different response codes, and much more. Try modifying the response section like this:

```json
"response": {
  "status": 503,
  "body": "Service unavailable",
  "fixedDelayMilliseconds": 1500
}
```

---

## Next Steps

- Try **recording traffic** with WireMock and saving real interactions as stubs  
- Integrate it into **automated tests**  
- Pair with **Testcontainers** for ephemeral, programmatic setup

---

## TL;DR

```bash
# Create project dirs
mkdir -p wiremock-demo/mappings wiremock-demo/__files

# Add a stub (GET /hello)
echo '{ "request": { "method": "GET", "url": "/hello" }, "response": { "status": 200, "body": "Hello, WireMock!", "headers": { "Content-Type": "text/plain" } } }' > wiremock-demo/mappings/hello-world.json

# Run WireMock
cd wiremock-demo
docker run --rm -it -p 8080:8080   -v "$PWD/mappings:/home/wiremock/mappings"   -v "$PWD/__files:/home/wiremock/__files"   wiremock/wiremock:latest

# Test it
curl http://localhost:8080/hello
```

---