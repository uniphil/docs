---
title: "Fly Machines API"
layout: docs
nav: machines
version: 1.0
spec: OAS 3.0.1
toc: false
---

## Introduction

Fly Machines are the compute behind the Fly.io platform. They are fast-launching VMs that can be started and stopped at subsecond speeds. A Machine is the configuration and state for a single VM running on our platform. Every Machine will belong to a Fly App; Apps can have more than one Machine. Read more [here](/docs/machines/).

The Machines REST API allows you to provison and manage Apps, Machines and Volumes on the Fly.io platform. To manage other Fly.io resources like organizations, use the [GraphQL API](/docs/networking/custom-domains-with-fly/#graphql-api-notes).

## Authentication

All requests must include the Fly API Token in the HTTP Headers as follows:

```
Authorization: Bearer [TOKEN]
```

You can get your API token using [flyctl](/docs/hands-on/install-flyctl/) by running `fly auth token`

## Base URL

The easiest (and recommended) way to connect to the Machines API is to use the `public api.machines.dev` endpoint, a simpler and more performant alternative to connecting over WireGuard. You can still access your Machines directly over a WireGuard VPN, and use the private Machines API endpoint: `http://_api.internal:4280`. This method requires more setup.

Follow the [instructions](/docs/networking/private-networking/#private-network-vpn) to set up a permanent WireGuard connection to your Fly.io [IPv6 private network](/docs/networking/private-networking/). Once you’re connected, Fly internal DNS should expose the Machines API endpoint at: `http://_api.internal:4280`

## Response Codes

The API uses conventional HTTP status codes to signal whether a request was successful or not.

Typically, 2xx HTTP status codes denote successful operations, 4xx codes imply failures related to the user, and 5xx codes suggest problems with the infrastructure.

| Status | Description                                 |
|--------|---------------------------------------------|
| `200`  | Successful request.                         |
| `201`  | Created successfully.                       |
| `202`  | Successful request. No content.             |
| `400`  | Check that the parameters were correct.     |
| `401`  | The API key used was missing or invalid.    |
| `404`  | The resource was not found.                 |
| `5xx`  | Indicates an error with Fly.io API servers. |

---

## Apps​

<div class="code-section" data-exclude-render>
  <p class="code-description">
    A Fly App is an abstraction for a group of Machines running your code on Fly.io, along with the configuration, provisioned resources, and data we need to keep track of to run and route to your Machines. Read more <a href="/docs/reference/apps/">here</a>.
  </p>
  <div class="endpoints highlight">
    <div class="highlight-header">Endpoints</div>
    <a class="endpoint">
      <span class="get">get</span>
      <span>/apps</span>
    </a>
    <a class="endpoint">
      <span class="post">post</span>
      <span>/apps</span>
    </a>
    <a class="endpoint">
      <span class="get">get</span>
      <span>/apps/{app_name}</span>
    </a>
    <a class="endpoint">
      <span class="delete">delete</span>
      <span>/apps/{app_name}</span>
    </a>
  </div>
</div>

## List Apps

List all apps with the ability to filter by organization slug.

<div class="code-section">
  <div class="left-column">
    <h3>Query Parameters</h3>
    <code>org_slug</code>
    <span>REQUIRED</span>
    <span>string</span>
    The org slug, or 'personal', to filter apps
  </div>
  <div class="highlight">
    <div class="highlight-header">Endpoints</div>
```shell
curl --request GET \
  --url 'https://api.machines.dev/v1/apps?org_slug='
```
    </pre>
  </div>
</div>