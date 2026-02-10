# grafana-ldap

## Overview

This project demonstrates a complete authentication stack using **Grafana**, **Keycloak**, and **OpenLDAP** running in Docker.

It showcases how to:
1.  Spin up an **OpenLDAP** server with pre-seeded users.
2.  Configure **Keycloak** to use OpenLDAP as a User Federation provider (via realm import).
3.  Configure **Grafana** to use Keycloak as an OAuth2 / OpenID Connect provider.
4.  Map LDAP groups to Grafana roles (Admin, Editor, Viewer) using JMESPath.
5.  Use **Nginx** as ingress controller
6.  Add **Loki** as log aggregator server and as datasource in Grafana
7.  Ship logs in Loki via **Promtail** agent 

## Services

| Service | URL | Credentials (User/Pass) | Description |
|---------|-----|-------------------------|-------------|
| **Grafana** | `http://localhost:3000` | Login via Keycloak | Visualization platform. |
| **Keycloak** | `http://localhost:8080` | `admin` / `password` | Identity and Access Management. |
| **phpLDAPadmin** | `http://localhost:8081` | DN: `cn=admin,dc=mycompany,dc=com` / `adminpassword` | Web UI for OpenLDAP. |
| **OpenLDAP** | `localhost:389` | N/A | LDAP Directory Service. |
| **Nginx** | `http://localhost:3000` | Login via Keycloak | Ingress controller |
| **Loki** | `http://localhost:3100` | As defined in .htpasswd file | Log aggregator |
| **Promtail** | N/A | As defined in .htpasswd file | Loki log agent |

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Running the Stack

1.  Start the containers:
    ```bash
    docker-compose up -d
    ```

2.  Access **Grafana** at http://localhost:3000.
3.  Click **Sign in with Keycloak**.

## Configuration Details

- **LDAP**: Configured with domain `mycompany.com`. Seeds data from `./users.ldif`.
- **Keycloak**: Imports configuration from `./realm-export.json` on startup.
- **Grafana**: Configured via environment variables in `docker-compose.yml` to enable Generic OAuth.

### Role Mapping

Grafana is configured to map Keycloak/LDAP groups to Grafana roles:

- `SG_ADMIN` -> **Admin**
- `SG_EDITOR` -> **Editor**
- Others -> **Viewer**
