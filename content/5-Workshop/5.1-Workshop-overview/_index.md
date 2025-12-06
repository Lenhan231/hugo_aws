---
title: "Introduction"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Goals

Introduce the serverless workshop: create Lambda (Node.js/Python), accept input parameters, integrate API Gateway, and deploy an in-app purchase suggestion API. Emphasize fast delivery with Lambda without managing servers.

## Architecture at a glance

- Lambda receives events, processes logic, and returns JSON.  
- API Gateway is the REST front door mapping methods to Lambda.  
- Optional: custom domain/stage for multiple environments.

## Expected outcomes

- Two Hello World functions (Node.js/Python) with parameters.  
- One REST endpoint (GET/POST) via API Gateway.  
- One business-logic function (purchase suggestions) returning JSON.

## Timing & structure

- Part 1: Lambda basics, testing in the console.  
- Part 2: API Gateway, input mapping, testing.  
- Part 3: In-app purchase exercise, clean up resources.

![image](/images/5-Workshop/Api_lambda.png)
