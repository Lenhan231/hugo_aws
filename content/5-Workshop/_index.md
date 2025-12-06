---
title: "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Serverless Workshop: AWS Lambda & API Gateway

## Overview

Hands-on workshop introducing serverless fundamentals, AWS Lambda programming model, and exposing functions via API Gateway. Participants build Hello World functions (Node.js/Python), add parameters, and deploy a sample in-app purchase suggestion API. Focus on delivering quickly without server management, cost awareness, and solid operational practices.

![image](/images/5-Workshop/1_D5zce2gCG9qajjMA_uoslg.webp)

## Agenda

- Serverless foundations: why Lambda, when to choose it, regional considerations.  
- Hello World fast path: create/test Lambda from console (Node.js/Python); adjust memory/timeouts.  
- Handling inputs: events, query strings, request body parsing, common error patterns.  
- API Gateway integration: create resources/methods, map to Lambda, deploy stages, test via curl/Postman.  
- Sample app: in-app purchase recommender (purchased vs. available items).  
- Cost & ops: memory tuning, cold-start awareness, logging, basic error handling.  
- Next steps: custom runtimes, serverless frameworks, edge deployments.

![image](/images/5-Workshop/Api_lambda.png)

## What You’ll Build

- Two Lambda functions (Node.js and Python) with parameterized responses.  
- One Lambda-backed REST endpoint via API Gateway (GET/POST).  
- A simple business-logic function (in-app purchase suggestions) returning JSON.

## Prerequisites

- AWS account with console access.  
- Basic familiarity with JavaScript or Python.  
- curl or Postman for API testing.

## References

- Console examples: Node.js & Python Hello World  
- API Gateway setup steps  
- Sample in-app purchase logic  
- Cleanup checklist
