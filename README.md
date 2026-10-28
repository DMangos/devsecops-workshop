# devsecops-workshop

The repo contains application code that represents frontend and backend

## CI/CD configuration

Configuration is read from GitHub Actions **variables** and **secrets**, not from a local `.env`
file. Set them under _Settings → Secrets and variables → Actions_.

Variables are unmasked and show up in job logs, which is what you want for
identifiers. Secrets are masked. Nothing below is needed to run the apps
locally — these are CI-only.

### Repository variables

| Name                             | Used by                             | Value                                                                                |
| -------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------ |
| `DOCKERHUB_USERNAME`             | `docker-publish.yml`                | Your Docker Hub namespace, e.g. `mikesindieiev`. Used as the image prefix and login. |
| `DEFECTDOJO_PRODUCT`             | `semgrep.yml`, `docker-publish.yml` | DefectDojo product everything files under, e.g. `devsecops-workshop`.                |
| `DEFECTDOJO_ENGAGEMENT`          | `semgrep.yml`                       | Engagement for Semgrep SAST findings, e.g. `security-scan`.                          |
| `DEFECTDOJO_ENGAGEMENT_API`      | `docker-publish.yml`                | Engagement for api container findings, e.g. `dt-api-image`.                          |
| `DEFECTDOJO_ENGAGEMENT_FRONTEND` | `docker-publish.yml`                | Engagement for frontend container findings, e.g. `dt-frontend-image`.                |
| `DT_PROJECT_API`                 | `app-sbom.yml`                      | Dependency-Track project UUID for the api **source** SBOM (cdxgen).                  |
| `DT_PROJECT_FRONTEND`            | `app-sbom.yml`                      | Dependency-Track project UUID for the frontend **source** SBOM (cdxgen).             |
| `DT_PROJECT_API_IMAGE`           | `docker-publish.yml`                | Dependency-Track project UUID for the api **image** SBOM (Trivy).                    |
| `DT_PROJECT_FRONTEND_IMAGE`      | `docker-publish.yml`                | Dependency-Track project UUID for the frontend **image** SBOM (Trivy).               |

Project UUIDs come from the Dependency-Track project page URL:
`/projects/<uuid>`.

### Repository secrets

| Name                      | Used by                              | Value                                                                                                                                    |
| ------------------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `DOCKERHUB_TOKEN`         | `docker-publish.yml`                 | Docker Hub access token with Read/Write scope. Create under _Account Settings → Personal access tokens_. Use a token, not your password. |
| `DEPENDENCYTRACK_URL`     | `app-sbom.yml`, `docker-publish.yml` | Base URL of the Dependency-Track API server, e.g. `https://dtrack.bashlab.org. Trailing slash is stripped.                               |
| `DEPENDENCYTRACK_API_KEY` | `app-sbom.yml`, `docker-publish.yml` | Dependency-Track API key. _Administration → Access Management → Teams → API Keys_. Needs `BOM_UPLOAD`.                                   |
| `DEFECTDOJO_URL`          | `semgrep.yml`, `docker-publish.yml`  | Base URL of DefectDojo, e.g. `https://defectdojo.bashlab.org`. Trailing slash is stripped.                                               |
| `DEFECTDOJO_TOKEN`        | `semgrep.yml`, `docker-publish.yml`  | DefectDojo API v2 key, from the user profile page.                                                                                       |
