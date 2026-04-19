## Plan: Convert Inngest Docker Compose to Coolify-Compatible Format

Transform the current Inngest Docker Compose file into a Coolify-compatible template with auto-generated hex credentials, proper health checks, and the custom logo.

### Steps

1. **Add Coolify header metadata** at top of [`inngest.yaml`](c:\Users\richa\projects\coolify\templates\compose\inngest.yaml) with documentation URL, slogan, `category: automation`, tags, `logo: svgs/inngest-dark.svg`, `port: 8288`

2. **Update `inngest` service**: Replace hardcoded keys with `SERVICE_HEX_32_EVENTKEY`/`SERVICE_HEX_32_SIGNINGKEY`, use Coolify Postgres URI pattern `postgres://$SERVICE_USER_POSTGRES:$SERVICE_PASSWORD_POSTGRES@postgres:5432/${POSTGRES_DB:-inngest}`, Redis URI `redis://:$SERVICE_PASSWORD_REDIS@redis:6379`, add `SERVICE_URL_INNGEST_8288`, change healthcheck to `127.0.0.1`, remove `ports:` and `networks:`

3. **Update `postgres` service**: Use `postgres:17-alpine`, replace credentials with `$SERVICE_USER_POSTGRES`/`$SERVICE_PASSWORD_POSTGRES`/`${POSTGRES_DB:-inngest}`, fix healthcheck with `$${POSTGRES_USER}`/`$${POSTGRES_DB}` escaping, remove `ports:` and `networks:`

4. **Update `redis` service**: Keep `redis:7-alpine`, add `command: ["sh", "-c", "redis-server --requirepass \"$SERVICE_PASSWORD_REDIS\""]`, add `SERVICE_PASSWORD_REDIS` env var, update healthcheck to `redis-cli -a $SERVICE_PASSWORD_REDIS PING`, remove `ports:` and `networks:`

5. **Remove `networks:` block** at bottom of file (Coolify manages networking)
