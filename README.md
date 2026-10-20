# App Store & Google Play Reviews Scraper — Python, Node.js & cURL examples

Get **reviews from the Apple App Store and Google Play** — rating, title, text, date, app version, helpful votes and **developer replies** — plus an app info row (rating, installs, ratings count). Any country, star filter. **$0.10 per 1,000 reviews.**

Hosted on Apify: **[App Store & Google Play Reviews Scraper](https://apify.com/tidydata/app-reviews-scraper)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/app-reviews-scraper").call(run_input={
    "apps": [
        "https://apps.apple.com/us/app/whatsapp-messenger/id310633997",
        "com.whatsapp"
    ],
    "countries": [
        "us",
        "gb"
    ],
    "maxReviewsPerApp": 500
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['store', 'rating', 'date', 'text']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/app-reviews-scraper').call({"apps": ["com.duolingo"], "ratings": ["1", "2"], "maxReviewsPerApp": 300});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~app-reviews-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"apps": ["com.duolingo"], "ratings": ["1", "2"], "maxReviewsPerApp": 300}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
