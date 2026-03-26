# WebPulse GitHub Action

Automatically analyze your website's performance, SEO, security, and accessibility in your GitHub workflows.

## Features

- ⚡ **Performance Analysis** - TTFB, compression, render-blocking resources
- 📈 **SEO Audit** - Title, meta description, headings, Open Graph
- 🔒 **Security Check** - HTTPS, HSTS, CSP, security headers
- ♿ **Accessibility** - Alt text, form labels, semantic HTML

## Usage

### Basic Example

```yaml
name: Website Health Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  health-check:
    runs-on: ubuntu-latest
    steps:
      - name: Check website health
        uses: batxufeng/webpulse-github-action@v1
        with:
          url: 'https://example.com'
```

### Fail on Low Score

```yaml
- name: Check website health
  uses: batxufeng/webpulse-github-action@v1
  with:
    url: 'https://example.com'
    fail-on-score: 70  # Fail if score < 70
```

### Comment on PR

```yaml
- name: Check website health
  uses: batxufeng/webpulse-github-action@v1
  with:
    url: 'https://example.com'
    comment-pr: 'true'
```

### With API Key (for higher rate limits)

```yaml
- name: Check website health
  uses: batxufeng/webpulse-github-action@v1
  with:
    url: 'https://example.com'
    api-key: ${{ secrets.WEBPULSE_API_KEY }}
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `url` | URL to analyze | **required** |
| `api-key` | WebPulse API key | optional |
| `fail-on-score` | Fail workflow if score below this (0-100) | `0` |
| `comment-pr` | Comment on PR with results | `false` |

## Outputs

| Output | Description |
|--------|-------------|
| `overall-score` | Overall health score (0-100) |
| `performance-score` | Performance score |
| `seo-score` | SEO score |
| `security-score` | Security score |
| `accessibility-score` | Accessibility score |
| `result-json` | Full scan result as JSON |

## Example Workflow

```yaml
name: CI/CD with WebPulse

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  webpulse:
    runs-on: ubuntu-latest
    steps:
      - name: Analyze website
        id: webpulse
        uses: batxufeng/webpulse-github-action@v1
        with:
          url: 'https://mywebsite.com'
          fail-on-score: 60

      - name: Upload results
        if: success()
        run: |
          echo "Overall Score: ${{ steps.webpulse.outputs.overall-score }}"
          echo "Full result: ${{ steps.webpulse.outputs.result-json }}"
```

## Use Cases

### 1. Monitor Production Website

```yaml
name: Production Health Check

on:
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: batxufeng/webpulse-github-action@v1
        with:
          url: 'https://my-production-site.com'
          fail-on-score: 80
```

### 2. PR Website Changes

```yaml
name: PR Website Review

on:
  pull_request:
    paths:
      - 'src/**/*.html'
      - 'src/**/*.css'
      - 'src/**/*.js'

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: batxufeng/webpulse-github-action@v1
        with:
          url: 'https://preview.my-site.com'
          comment-pr: 'true'
```

### 3. Pre-deployment Gate

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  health-gate:
    runs-on: ubuntu-latest
    steps:
      - name: Health check
        uses: batxufeng/webpulse-github-action@v1
        with:
          url: 'https://staging.my-site.com'
          fail-on-score: 75

  deploy:
    needs: health-gate
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying..."
```

## API Rate Limits

- **Free tier**: 5 scans/month
- **Pro tier**: 50 scans/month
- **Business**: Unlimited

Get your API key at [winwebpulse.com](https://winwebpulse.com)

## Contributing

PRs welcome! See [CONTRIBUTING.md](../CONTRIBUTING.md)

## License

MIT

## Related

- [WebPulse Web App](../webpulse) - Full web application
- [WebPulse Core](../webpulse-core) - Analysis library
- [WebPulse Chrome Extension](../webpulse-chrome-extension) - Browser extension
