<a id="readme-top"></a>

<div align="center">
  <a href="https://exchangeit.app">
    <img src="https://exchangeit.app/logo.svg" alt="Exchange It" width="480">
  </a>

  <h3>Free exchange rates API — currencies, crypto and 30 years of history</h3>

  <p>
    <a href="https://api.exchangeit.app/v3/docs"><img alt="API v3" src="https://img.shields.io/badge/API-v3-2f6feb?style=flat-square"></a>
    <a href="spec/latest.yaml"><img alt="OpenAPI 3.0" src="https://img.shields.io/badge/OpenAPI-3.0.3-6ba539?style=flat-square&logo=openapiinitiative&logoColor=white"></a>
    <img alt="No API key" src="https://img.shields.io/badge/API%20key-not%20required-brightgreen?style=flat-square">
    <a href="https://creativecommons.org/licenses/by/4.0/"><img alt="Data: CC BY 4.0" src="https://img.shields.io/badge/data-CC%20BY%204.0-lightgrey?style=flat-square"></a>
    <a href="https://github.com/VoirDev/exchangeit/actions/workflows/spec.yml"><img alt="Spec" src="https://img.shields.io/github/actions/workflow/status/VoirDev/exchangeit/spec.yml?branch=main&style=flat-square&label=spec"></a>
  </p>

  <p>
    <a href="https://api.exchangeit.app/v3/docs"><strong>API reference »</strong></a>
    <br /><br />
    <a href="https://exchangeit.app">Website</a>
    ·
    <a href="https://apps.apple.com/us/app/exchange-it-currency-rates/id6503626689">App Store</a>
    ·
    <a href="https://play.google.com/store/apps/details?id=dev.voir.anyexchange.android">Google Play</a>
    ·
    <a href="https://github.com/VoirDev/exchangeit/issues/new">Report a bug</a>
    ·
    <a href="https://github.com/VoirDev/exchangeit/issues/new">Request a feature</a>
  </p>
</div>

---

Exchange It is a currency converter app and the open REST API behind it. This repository holds the
API's public [OpenAPI document](spec/latest.yaml) and is the place for issues and feature requests.

- **300+ currencies** — 160+ fiat currencies, precious metals and SDR, plus 150 crypto assets
- **Latest rates** refreshed several times a day, with the change since the previous close
- **History back to 1996** — any day, or weekly and monthly open / close / average / min / max
- **Cross rates** between any two currencies, conversions into up to 50 currencies at once
- **Names in 17 languages**, flags, logos, countries and central banks
- **No sign-up, no key** — free, cache-friendly, CORS-enabled

## Contents

- [Quick start](#quick-start)
- [Endpoints](#endpoints)
- [Concepts](#concepts)
- [Generating a client](#generating-a-client)
- [Migrating from v1 / v2](#migrating-from-v1--v2)
- [Repository layout](#repository-layout)
- [Mobile app](#mobile-app)
- [Contact](#contact)

## Quick start

The base URL is `https://api.exchangeit.app/v3`. Every endpoint is a plain `GET`.

**Latest rates** — how much of each currency one US dollar buys:

```bash
curl "https://api.exchangeit.app/v3/rates/latest?symbols=EUR,GBP,btc"
```

```json
{
  "base": "USD",
  "updatedAt": "2026-10-21T12:23:27Z",
  "rates": {
    "EUR": { "rate": 0.890532, "date": "2026-10-21", "observedAt": "2026-10-21T12:00:00Z", "previousClose": 0.890329, "change": 0.000203, "changePercent": 0.0228 },
    "GBP": { "rate": 0.746496, "date": "2026-10-21", "observedAt": "2026-10-21T12:00:00Z", "previousClose": 0.747587, "change": -0.001091, "changePercent": -0.1459 },
    "btc": { "rate": 0.0000119868, "date": "2026-10-21", "observedAt": "2026-10-21T12:00:00Z", "previousClose": 0.0000121228, "change": -0.000000136, "changePercent": -1.1216 }
  }
}
```

**Convert** an amount, now or at a past date:

```bash
curl "https://api.exchangeit.app/v3/convert?from=USD&to=EUR,JPY&amount=100"
```

```json
{
  "from": "USD",
  "amount": 100.0,
  "date": "2026-10-21",
  "results": [
    { "to": "EUR", "rate": 0.890532, "amount": 89.0532 },
    { "to": "JPY", "rate": 157.888154, "amount": 15788.8154 }
  ]
}
```

**A chart series** — monthly euro rates against the dollar:

```bash
curl "https://api.exchangeit.app/v3/rates/timeseries?symbols=EUR&from=2026-07-01&interval=month"
```

```json
{
  "base": "USD",
  "from": "2026-07-01",
  "to": "2026-10-21",
  "interval": "month",
  "series": {
    "EUR": [
      { "date": "2026-07-01", "rate": 0.868280, "open": 0.878362, "average": 0.875554, "min": 0.867929, "max": 0.879418 },
      { "date": "2026-08-01", "rate": 0.860814, "open": 0.867118, "average": 0.863115, "min": 0.855793, "max": 0.868851 }
    ]
  }
}
```

**Everything about a pair** — the rate, its day / week / month / year change and the 52-week range:

```bash
curl "https://api.exchangeit.app/v3/pairs/EUR/USD"
```

> [!TIP]
> Try every endpoint straight from the browser in the
> [interactive reference](https://api.exchangeit.app/v3/docs).

## Endpoints

| | Endpoint | What it returns |
| --- | --- | --- |
| **Currencies** | [`GET /currencies`](https://api.exchangeit.app/v3/docs#tag/currencies/GET/currencies) | Search and filter currencies — by kind, status, country, central bank, popularity |
| | [`GET /currencies/{id}`](https://api.exchangeit.app/v3/docs#tag/currencies/GET/currencies/{id}) | One currency, with countries, issuers, history and rate coverage |
| **Rates** | [`GET /rates/latest`](https://api.exchangeit.app/v3/docs#tag/rates/GET/rates/latest) | Latest rates with the change since the previous close |
| | [`GET /rates/historical/{date}`](https://api.exchangeit.app/v3/docs#tag/rates/GET/rates/historical/{date}) | Rates on any date since 1996 |
| | [`GET /rates/timeseries`](https://api.exchangeit.app/v3/docs#tag/rates/GET/rates/timeseries) | Daily, weekly or monthly series for charts |
| **Conversion** | [`GET /convert`](https://api.exchangeit.app/v3/docs#tag/conversion/GET/convert) | An amount converted into up to 50 currencies |
| | [`GET /pairs/{base}/{quote}`](https://api.exchangeit.app/v3/docs#tag/conversion/GET/pairs/{base}/{quote}) | One currency pair at a glance |
| **Reference** | [`GET /countries`](https://api.exchangeit.app/v3/docs#tag/reference/GET/countries) | Countries and the currencies they use |
| | [`GET /central-banks`](https://api.exchangeit.app/v3/docs#tag/reference/GET/central-banks), [`/central-banks/{id}`](https://api.exchangeit.app/v3/docs#tag/reference/GET/central-banks/{id}) | Central banks and the currencies they issue |
| | [`GET /locales`](https://api.exchangeit.app/v3/docs#tag/reference/GET/locales) | Languages names are available in |
| **Meta** | [`GET /meta`](https://api.exchangeit.app/v3/docs#tag/meta/GET/meta) | Data freshness, request bounds and your current allowance |

## Concepts

<details open>
<summary><strong>Currency identifiers</strong></summary>
<br>

Every currency has a single `id`, used in paths, in `base`, `symbols`, `from` and `to`, and as the key
of every rate map.

| Kind | `id` | Examples |
| --- | --- | --- |
| Fiat currency | ISO 4217 code, upper case | `EUR`, `USD`, `MNT` |
| Crypto asset | Alias, lower case | `btc`, `eth`, `mnt` |

Case matters when a fiat code and a crypto alias coincide — `MNT` is the Mongolian tögrög, `mnt` is
Mantle. Anything else is matched ignoring case, preferring fiat, so `eur` still finds `EUR`.
</details>

<details>
<summary><strong>Rates</strong></summary>
<br>

A rate is how many units of the quoted currency one unit of `base` buys. `base` defaults to `USD`,
and any currency with rates can be the base — cross rates are computed from USD rates of the same
date. All rates are mid-market. Latest rates are refreshed several times a day (`observedAt` says
when); historical and time-series data has one rate per currency per day.
</details>

<details>
<summary><strong>Localization</strong></summary>
<br>

Names follow `Accept-Language`, or the `lang` query parameter, which overrides it and gives each
language its own cacheable URL. `GET /locales` lists what is available; `Content-Language` says which
language a response is in.
</details>

<details>
<summary><strong>Caching</strong></summary>
<br>

Every successful response has a strong `ETag` and a `Cache-Control` lifetime matched to the data —
one minute for latest rates, one hour for reference data, one day for past dates. Send
`If-None-Match` to get a cheap `304 Not Modified`.
</details>

<details>
<summary><strong>Limits and API keys</strong></summary>
<br>

No key is needed. Each caller — an API key, or the caller's address without one — gets an allowance of
request units that refills continuously (120 units per minute anonymously). Most requests cost one
unit; a time series costs one unit per 1,000 points returned. Every response reports the allowance in
`RateLimit-Policy` and what is left in `RateLimit`; once it is spent the API answers `429` with
`Retry-After`. `GET /meta` is free and shows your allowance.

Need more? Ask for a key at [hello@exchangeit.app](mailto:hello@exchangeit.app) and send it as
`X-API-Key`.

Responses are bounded too: 100 items per page, 50 symbols per request, 10 symbols per time series,
and up to 5 years of daily or 20 years of weekly points — `interval=month` has no limit.
</details>

<details>
<summary><strong>Errors</strong></summary>
<br>

Errors are [RFC 9457](https://www.rfc-editor.org/rfc/rfc9457) problem documents:

```json
{
  "type": "https://exchangeit.app/problems/invalid-parameter",
  "title": "Invalid parameter",
  "status": 400,
  "detail": "symbols lists 60 currencies; at most 50 are allowed",
  "instance": "/v3/rates/latest",
  "parameter": "symbols"
}
```
</details>

## Generating a client

[`spec/latest.yaml`](spec/latest.yaml) is the same document the API serves at
[`/v3/openapi.yaml`](https://api.exchangeit.app/v3/openapi.yaml), so any OpenAPI 3 tool works with it:

```bash
npx @openapitools/openapi-generator-cli generate -g typescript-fetch -o exchangeit-client \
  -i https://raw.githubusercontent.com/VoirDev/exchangeit/main/spec/latest.yaml
```

The [Kotlin Multiplatform SDK](https://github.com/VoirDev/exchangeit-kmm-sdk) (JVM, Android, iOS)
currently wraps the v1 / v2 API.

## Migrating from v1 / v2

> [!WARNING]
> The v1 and v2 endpoints are **deprecated** since 20 October 2026. They still work and return
> `Deprecation` and `Link: rel="successor-version"` headers, but get no new features and will be
> removed. Their document is kept in [`spec/legacy.yaml`](spec/legacy.yaml). The `/v1/sources`
> endpoints are already gone.

| v1 / v2 | v3 |
| --- | --- |
| `GET /v1/currencies?crypto&search&withObsolete&withNoRates` | `GET /v3/currencies?kind&query&status=all&hasRates` |
| `GET /v1/currencies/{alias}` | `GET /v3/currencies/{id}` |
| `GET /v2/currencies/{alias}/latest?for=…` | `GET /v3/rates/latest?base={id}&symbols=…` |
| `GET /v2/currencies/{alias}/historical?date=…` | `GET /v3/rates/historical/{date}?base={id}` |
| `GET /v2/currencies/{alias}/range?start&end` | `GET /v3/rates/timeseries?base={id}&from&to` |
| `GET /v2/currencies/{alias}/monthly?start&end` | `GET /v3/rates/timeseries?base={id}&from&to&interval=month` |
| `GET /v2/rates/latest?aliases=…` | `GET /v3/rates/latest?base={id}`, once per base |
| `GET /v1/converter?amount&from&to` | `GET /v3/convert?amount&from&to` — `to` takes a list |
| `GET /v1/sources`, `/v1/sources/{alias}` | Removed |

Besides the paths, v3 differs in a few ways worth knowing:

- Fiat ids are **upper case** ISO codes (`EUR`), crypto ids stay lower case (`btc`).
- Responses are no longer wrapped in `data` / `copyright`; lists are `{ "items": [...], "page": {...} }`.
- Rate maps are keyed by currency id instead of being arrays.
- Flags and logos are absolute URLs in SVG, PNG, WebP and JPG.
- Errors are `application/problem+json` instead of `{ "message": "..." }`.

## Repository layout

```text
spec/
  latest.yaml    OpenAPI 3.0 document of the current API (v3)
  legacy.yaml    OpenAPI 3.0 document of the deprecated v1 / v2 API
redocly.yaml     Lint configuration for both documents
```

The [`Spec`](.github/workflows/spec.yml) workflow lints both documents and checks that
`spec/latest.yaml` matches what production serves:

```bash
npx @redocly/cli lint
```

## Mobile app

Exchange It for iOS and Android: a converter for every currency and crypto asset, built on this API.

<p>
  <a href="https://apps.apple.com/us/app/exchange-it-currency-rates/id6503626689"><img alt="Download on the App Store" src="https://img.shields.io/badge/App%20Store-000000?style=for-the-badge&logo=apple&logoColor=white"></a>
  <a href="https://play.google.com/store/apps/details?id=dev.voir.anyexchange.android"><img alt="Get it on Google Play" src="https://img.shields.io/badge/Google%20Play-414141?style=for-the-badge&logo=googleplay&logoColor=white"></a>
</p>

## Contact

Found a bug or missing a currency? [Open an issue](https://github.com/VoirDev/exchangeit/issues).

Gary Bezruchko — [@checksanity](https://t.me/checksanity) — [hello@exchangeit.app](mailto:hello@exchangeit.app)

Exchange rate data is available under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/);
please credit [Exchange It](https://exchangeit.app).

<p align="right"><a href="#readme-top">back to top ↑</a></p>
