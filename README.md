# Doroi ID

**Doroi ID** is the central identity and authentication service for Doroi applications.

The project uses [Keycloak](https://www.keycloak.org/) and OpenID Connect (OIDC). Apps should use Authorization Code Flow with PKCE; passwords stay with the identity provider and are never handled by client apps.

## Status

Early development — not ready for production or real user accounts.

## Planned features

- Central Doroi account registration and login
- Email verification and password reset
- OpenID Connect / OAuth 2.0
- One account across Doroi apps and websites
- Application/client management
- Account profile and session management
- Kotlin Android integration using AppAuth

## Architecture

- Identity provider: Keycloak
- Database: PostgreSQL
- Mobile integration: AppAuth for Android
- Protocol: OpenID Connect (OAuth 2.0 Authorization Code Flow + PKCE)

## Local development

Requires Docker.

1. Copy `.env.example` to `.env`.
2. Set strong local-only values for `KC_BOOTSTRAP_ADMIN_USERNAME` and `KC_BOOTSTRAP_ADMIN_PASSWORD`.
3. Run `docker compose up -d`.
4. Open `http://localhost:8080` and sign in to the Keycloak administration console.
5. Create a realm named `doroi`.

The local Compose setup uses Keycloak development mode. It is for local testing only and must not be exposed to the public internet.

## Deployment

The Dockerfile can be used by a container host such as Render. Before deploying:

1. Create a persistent PostgreSQL database.
2. Create a web service from this repository using the Dockerfile.
3. Set `KC_DB_URL`, `KC_DB_USERNAME`, and `KC_DB_PASSWORD` as private environment variables in the hosting dashboard.
4. Set `KC_HOSTNAME` to the exact public HTTPS URL assigned to the service.
5. Set a strong, unique bootstrap admin password.
6. Configure HTTPS, health checks, email delivery, backups, and restricted admin access before real users register.

Free hosting can sleep, have limited resources, or change its terms. Free databases may expire or lack reliable backups. Do not store important user identities on a temporary database.

## Security requirements

- Use HTTPS for every public login and token endpoint.
- Use Authorization Code Flow with PKCE for Android and browser-based public clients.
- Never embed a client secret in an Android app.
- Never commit passwords, API keys, database URLs, or private keys.
- Restrict access to the Keycloak administration interface.
- Configure allowed redirect URIs narrowly for each app.
- Back up PostgreSQL and test restoring it.
- Do not use `start-dev` in production.

Repository: https://github.com/darshanghimire03-lgtm/doroi-id
