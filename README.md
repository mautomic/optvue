# optvue

Optvue is a dashboard for viewing options volume and open interest for a selected ticker and expiration date.
The implementation of fetching and parsing data is tied to TD Ameritrade's API. 

## Usage

General setup requires node and npm installed. 

Create config.json in the __src__ directory and add your TD Ameritrade API key.
```json
{
  "apiKey": "YOUR_KEY"
}
```

In order to run this web app, you must first have an API key. This requires both an account with the
[TDAmeritrade](https://www.tdameritrade.com/home.page), and an account setup on the [developer page](https://developer.tdameritrade.com).
