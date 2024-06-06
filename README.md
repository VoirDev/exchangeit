<br />
<div align="center">
  <a href="https://github.com/github_username/repo_name">
    <img src="https://exchangeit.app/logo.svg" alt="Logo" width="600" height="200">
  </a>

<h1 align="center">Exchange It</h1>
  <p align="center">
    Free currency exchange rates API and Mobile App.<br/>
    700+ currencies and cryptos, up-to-date rates
    <br/>
    <br />
    <a href="https://exchangeit.app/docs"><strong>Explore the API docs »</strong></a>
    <br />
    <br />
    <a href="https://apps.apple.com/us/app/exchange-it-currency-rates/id6503626689">App Store</a>
    ·
    <a href="https://play.google.com/store/apps/details?id=dev.voir.anyexchange.android&pli=1">Google Play</a>
    ·
    <a href="https://github.com/VoirDev/exchangeit/issues">Report Bug</a>
    ·
    <a href="https://github.com/VoirDev/exchangeit/issues">Request Feature</a>
  </p>
</div>

## About The Project

The primary goal of the Exchange It project is to create a user-friendly mobile
application along with a versatile RESTful API for accessing current and historical currency
exchange rates. This initiative aims to provide developers with a seamless toolset to integrate
accurate and up-to-date exchange rate information into their applications.

### Built With

![Android](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/android.svg)
![Kotlin](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/kotlin.svg)
![React](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/react.svg)
![JavaScript](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/javascript.svg)
![TypeScript](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/typescript.svg)
![NodeJS](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/nodejs.svg)
![NestJs](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/nestjs.svg)
![NextJs](https://s3.timeweb.com/890fdb6a-sanity/static/sanity/github/assets/nextjs.svg)

<!-- -->
## API

<u>No authorization needed.</u> Rates are collected from several *FREE* sources and <u>provided as
it is.</u>

### Usage

1. Getting list of available currencies

```bash
curl --request GET \
  --url https://api.exchangeit.app/v1/currencies \
  --header 'Accept: application/json'
```

2. Get latest rates for selected currency

```bash
curl --request GET \
  --url https://api.exchangeit.app/v1/currencies/AED/latest \
  --header 'Accept: application/json'
```

or

_Refer to the [Open API documentation](https://exchangeit.app/docs) for other available end-points
and responses_

<!-- -->

### Integration

Currently, API has Kotlin Multiplatform SDK and support JVM, Android and iOS platforms

_Please refer to the [Github repository](https://github.com/VoirDev/exchangeit-kmm-sdk)_

<!-- -->

## Mobile App

You can download the app from [Google Play](https://play.google.com/store/apps/details?id=dev.voir.anyexchange.android&pli=1) or [App Store](https://apps.apple.com/us/app/exchange-it-currency-rates/id6503626689)

## Contact

See the [open issues](https://github.com/VoirDev/exchangeit/issues) for a full list of
proposed features (and known issues).

Gary Bezruchko - [@checksanity](https://t.me/checksanity) - hello@exchangeit.app

Website Link: [exchangeit.app](https://exchangeit.app/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
