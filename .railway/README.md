# Railway deployment

`railway.ts` defines the 9router service and its persistent `9router-data` volume. The volume mounts at `/app/data`, where the SQLite database and backups live. This is Railway's project-level Infrastructure as Code format; `railway.json` and `railway.toml` are deprecated.

The configuration follows `eladrave/9router` on `codex/codex-live-model-discovery` and uses the repository Dockerfile, port 20128, and `/api/health` check. Change the branch in `railway.ts` if the release branch changes. The generated Railway domain is managed in Railway because the IaC importer does not include generated domains.

## Review and apply

Install dependencies with `npm install` and authenticate with the Railway CLI. Link the intended project and environment with `railway link`, then run:

```sh
railway config plan
railway config apply
```

Alternatively, set `RAILWAY_PROJECT_ID` and `RAILWAY_ENVIRONMENT_ID` for the intended environment before running those commands. Review the plan before applying it. This file describes the whole Railway project, so deleting the service or volume from it can delete the live resource and its data.

`preserve()` keeps the current Railway values for `JWT_SECRET`, `INITIAL_PASSWORD`, `API_KEY_SECRET`, `MACHINE_ID_SALT`, `BASE_URL`, and `NEXT_PUBLIC_BASE_URL` out of Git. For a new installation, provision strong secrets in Railway and set both URL variables to its public HTTPS URL. Create a Railway domain on port 20128 before using the public service.
