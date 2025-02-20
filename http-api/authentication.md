---
description: Premium Only
---

# Authentication

In order to authenticate requests to our HTTP API you will need an API key, this can be found in the game settings page of your game dashboard. If your API key is empty, you will need to [reset](authentication.md#resetting-your-api-key) it.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## Using Your API Key

In order to use your API key, you'll need to provide it in requests to our HTTP API as a header called `X-API-KEY`. The API key will not expire unless it is reset (see below).

## Resetting Your API Key

In order to reset your API key (or set it for the first time), you will need to go to the Admin tab of your [game settings](https://panel.planetaryprocessing.io/games) and click "Reset API Key".

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
