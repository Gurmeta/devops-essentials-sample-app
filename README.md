# DevOps Essentials Sample App

[![CI](https://github.com/Gurmeta/devops-essentials-sample-app/actions/workflows/ci.yml/badge.svg)](https://github.com/Gurmeta/devops-essentials-sample-app/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A small static website used to practise continuous delivery. Every change is built and verified with Gradle, then promoted through staging to production by a Jenkins pipeline. GitHub Actions runs the same checks on each pull request.

## Pipeline

```
commit -> build -> verify -> staging -> manual approval -> production
```

| Stage | Tool | What happens |
| --- | --- | --- |
| Build | Gradle | Verifies required files and packages `src/` into a zip |
| Verify | GitHub Actions | Builds on every PR and validates the HTML |
| Pages | GitHub Actions | Publishes `src/` to GitHub Pages on every merge to `master` |
| Staging | Jenkins | Publishes `src/` to the staging host over SSH |
| Production | Jenkins | Same publish step, gated by a manual approval |

## Project layout

```
src/
  index.html      Page markup, architecture diagram and config excerpts
  css/styles.css  Styles, with light and dark themes
  js/main.js      Theme toggle and scroll animations
  favicon.svg
build.gradle      verifySite and siteZip tasks
Jenkinsfile       Build and deploy pipeline
.github/workflows CI
```

## Run locally

The site has no runtime dependencies. Open `src/index.html` in a browser, or serve the folder:

```bash
python3 -m http.server --directory src 8080
```

Build and verify with Gradle (requires JDK 17+):

```bash
./gradlew build
```

The deployable archive is written to `build/distributions/`.

## Deployment

The `Jenkinsfile` deploys with the *Publish over SSH* plugin. Define two servers named `staging` and `production` in Jenkins; their credentials are stored there and never in this repository.

## License

Released under the [MIT License](LICENSE).
