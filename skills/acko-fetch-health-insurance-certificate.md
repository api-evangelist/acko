---
name: Fetch an Acko health insurance certificate
description: Retrieve a citizen's Acko health insurance policy certificate through India's API Setu / DigiLocker platform.
api: openapi/acko-apisetu-openapi-original.json
operations: [hlipc]
---

# Fetch an Acko health insurance certificate

Use this skill to pull an Acko health insurance policy certificate for a
consenting citizen via the government API Setu gateway.

## Auth
Send both API Setu credentials as headers on every request:
- `X-APISETU-APIKEY: <your api setu api key>`
- `X-APISETU-CLIENTID: <your api setu client id>`

## Steps
1. Collect a signed consent artifact from the citizen (the `ConsentArtifactSchema`
   in the request body — `consent` + `signature`).
2. `POST /hlipc/certificate` (operationId `hlipc`) with the JSON request body and
   the desired `format` (PDF, XML or JSON).
3. On `200`, read the certificate document from the response.

## Errors
- `400` invalid request/consent — validate the consent artifact.
- `401` bad credentials — check the API key + client id headers.
- `404` no matching policy — verify the citizen identifiers.
- `500/502/503/504` upstream issuer error — retry with backoff.
See `errors/acko-problem-types.yml`.
