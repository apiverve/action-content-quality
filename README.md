# APIVerve Content Quality Action

> Check content for profanity, analyze sentiment, and measure readability

> **Beta Release** - This action is in beta. We'd love your feedback! [Open an issue](https://github.com/apiverve/action-content-quality/issues) if you encounter any problems.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Content Quality-blue?logo=github)](https://github.com/marketplace/actions/apiverve-content-quality)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=content-quality)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=content-quality)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=content-quality)**

---

## What does this action do?

This action provides access to APIVerve's Content Quality APIs directly in your GitHub workflows:

- Filter profanity from user-generated content
- Analyze sentiment of text
- Measure content readability scores
- Detect gibberish or spam content

### Available APIs

| API | Description |
|-----|-------------|
| `contentfilter` | Content Filter checks URLs against a comprehensive blocklist of 200,000+ domains categorized as ads-malware, fake news, gambling, adult content, or social media. Returns the specific category for blocked domains. |
| `profanityfilter` | Profanity Filter is a simple tool for filtering out profanity words from a text. It returns the text with the profanity words replaced by placeholders. |
| `sentimentanalysis` | Sentiment Analysis is a simple tool for analyzing the sentiment of a text. It returns the sentiment score and the sentiment label. |
| `readabilityscore` | Readability Score is a simple tool for calculating the readability score of text. It returns the readability score based on various readability formulas. |
| `gibberishdetector` | Gibberish Detector analyzes text using bigram frequency and vowel ratios to identify nonsensical or randomly generated content. |

---

## Quick Start

```yaml
- name: Content Quality
  uses: apiverve/action-content-quality@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: contentfilter
    params: '{&quot;text&quot;: &quot;Your content here&quot;}'
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=content-quality) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: Content Quality
  uses: apiverve/action-content-quality@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: contentfilter
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `contentfilter`, `profanityfilter`, `sentimentanalysis`, `readabilityscore`, `gibberishdetector` | No | `contentfilter` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |

*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |

---

## Examples

### Profanity Check

Check text for profanity

```yaml
- name: Profanity Check
  id: content-quality-0
  uses: apiverve/action-content-quality@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: profanityfilter
    params: '{&quot;text&quot;: &quot;Your content here&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.content-quality-0.outputs.data }}"
```

### Sentiment Analysis

Analyze the sentiment of text

```yaml
- name: Sentiment Analysis
  id: content-quality-1
  uses: apiverve/action-content-quality@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: sentimentanalysis
    params: '{&quot;text&quot;: &quot;This is a great product!&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.content-quality-1.outputs.data }}"
```

### Readability Score

Calculate readability metrics

```yaml
- name: Readability Score
  id: content-quality-2
  uses: apiverve/action-content-quality@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: readabilityscore
    params: '{&quot;text&quot;: &quot;Your content here&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.content-quality-2.outputs.data }}"
```


---

## Full Workflow Example

```yaml
name: Content Quality Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  content-quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Content Quality
        id: result
        uses: apiverve/action-content-quality@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: contentfilter
          params: '{&quot;text&quot;: &quot;Your content here&quot;}'

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=content-quality).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=content-quality)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=content-quality)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-content-quality/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=content-quality) - 350+ APIs for developers
