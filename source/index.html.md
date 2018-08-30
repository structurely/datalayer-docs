---
title: API Reference

language_tabs:
  - curl

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - leads
  - errors

search: true
---

# Introduction

Welcome to the Structurely Holmes API! You can use our API to view, update, query both leads and agent's accounts. This API is a work in progress and is not in a final state. API endpoints, authentication flows, and availability may be subject to change at any time. If you are intestested in working with us via our API please contact us at info@structurely.com

# Authentication

All requests to the API in must include the header X-Api-Authorization with your API key. There will soon be a OAuth2 flow for obtaining bearer tokens to use in the Authorization header. To get an API key please contact us.

# Overview

## Endpoints

All endpoints are hosted by the domain `api.structurely.com`. All requests must use `https`. Any request with the protocol `http` will be redirected to `https` with the status code 301. The path for the api will always start with `/v1`. Every resource endpoint will start with `https://api.structurely.com/v1`.

## Types

Type | Description
---- | -----------
`ObjectId` | A 24 byte hexadecimal string. See [ObjectId](https://docs.mongodb.com/manual/reference/method/ObjectId/).
`Boolean` | A JSON boolean represented by either `true` or `false`.
