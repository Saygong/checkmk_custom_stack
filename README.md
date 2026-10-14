# Checkmk custom stack

This repository provides a Docker-based Checkmk environment for running multiple independent monitoring sites in a single stack. It is designed for internal IT and security monitoring scenarios where a company wants to separate monitoring contexts while keeping a common operating model for compliance activities such as ISO 27001 and NIS2.

## Overview

The stack uses Docker Compose to deploy:

- an NGINX reverse proxy in front of the Checkmk instances
- three Checkmk sites running independently in separate containers
- dedicated Docker volumes per site for persistence
- a shared backup directory for site data and operational recovery

This makes it easy to support different environments or business units while keeping the setup manageable from a single repository.

## Current architecture

The project currently provisions three sites, represented by the following environment variables:

- `${CMK_SITE_ID_1}`
- `${CMK_SITE_ID_2}`
- `${CMK_SITE_ID_3}`

All sites are exposed via a single NGINX entry point on port 80, which forwards traffic to the corresponding Checkmk site.

## Intended use

The repository is intended for:

- central monitoring of infrastructure and services
- multi-site or multi-environment deployments
- future distributed monitoring setups using Livestatus
- governance and security operations aligned with compliance requirements such as ISO 27001 and NIS2

The structure is deliberately simple so it can grow into a larger distributed monitoring architecture without rewriting the base deployment model.

## Repository layout

- `docker-compose.yml` – defines the Checkmk and NGINX services
- `nginx/` – reverse proxy configuration and log files
- `checkmk/backups/` – backup storage for Checkmk data
- `.env` – environment variables for site names, credentials, version, and ports

## Quick start

1. Ensure Docker and Docker Compose are installed.
2. Review the variables in `.env` and adjust site names, passwords, and version as needed.
3. Start the stack:

   ```bash
   docker compose up -d
   ```

4. Open the reverse-proxy URL in a browser, for example:

   - `http://localhost/${CMK_SITE_ID_1}/`
   - `http://localhost/${CMK_SITE_ID_2}/`
   - `http://localhost/${CMK_SITE_ID_3}/`

## Notes

- The project currently uses the Checkmk community edition container image.
- The configuration already includes Livestatus for selected sites, which is useful for future distributed-monitoring integrations.
- Backups and persistence are handled through named Docker volumes and the local `checkmk/backups` folder.

This repository is a practical foundation for a multi-site Checkmk stack tailored to enterprise security monitoring and compliance-oriented operations.
