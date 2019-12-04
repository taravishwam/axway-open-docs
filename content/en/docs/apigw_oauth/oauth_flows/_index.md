---
title: API Gateway OAuth 2.0 authentication flows
linkTitle: OAuth 2.0 authentication flows
weight: 20
date: 2019-11-18
description: Learn about the supported OAuth 2.0 flows in detail, and how to run sample scripts demonstrating the flows.
---

API Gateway can use the OAuth 2.0 protocol for authentication and authorization. API Gateway can act as an OAuth 2.0 authorization server and supports several OAuth 2.0 flows that cover common web server, JavaScript, device, installed application, and server-to-server scenarios.

API Gateway includes sample Jython scripts for all supported OAuth flows. For example, to run a sample script for the implicit grant flow:

```
cd INSTALL_DIR/samples/scripts/oauth
sh run.sh oauth/implicit_grant.py
```