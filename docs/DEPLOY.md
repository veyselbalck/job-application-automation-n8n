# Running 24/7 without your own computer

The schedule only fires while n8n is running, and the 2-minute waits need it to stay up for about 10–60 minutes each morning.

| Option | Cost (check current prices) | Effort | Notes |
|---|---|---|---|
| **Small VPS + Docker** (Hetzner, Hostinger, …) | roughly $4–7/month | medium | Use `docker-compose.yml`. Needs HTTPS for Google OAuth (reverse proxy such as Caddy, or a tunnel). |
| **Managed n8n hosting** (various providers) | from about $3–7/month | low | They handle HTTPS and updates. |
| **n8n Cloud** | from about $20–24/month | lowest | 2,500 executions/month on the entry plan; this workflow uses few executions but each mail loop counts. |
| **Free tiers** (e.g. Oracle Always Free) | $0 | high | Possible, but capacity and sign-up vary; verify before relying on it. |

## Google OAuth on a server
Google only accepts `https://` redirect URLs (localhost excepted). Put n8n behind a domain with HTTPS, set `N8N_HOST`, `N8N_PROTOCOL=https`, `WEBHOOK_URL`, and add `https://YOUR-DOMAIN/rest/oauth2-credential/callback` as an authorised redirect URI in Google Cloud.

## Alternative
Google Apps Script triggers run on Google's servers for free and don't need a PC — a good option for the sending part if you don't want to pay for hosting.
