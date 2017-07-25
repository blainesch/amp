## AMP test app

This app generates test data for Browser AMP support.

The AMP page of this app can be viewed by searching Google for "Bookstein AMP" and looking for the hit with the small lightning bolt icon. That page, when found in that manner, is actually served from the Google AMP CDN.

However, you can also view this app in a web browser and generate test data.

### Registering a page with Google AMP

The "analytics" page of this site is served from the Google AMP CDN because it was validated and submitted through this site:

https://search.google.com/test/amp

Enter the URL of your AMP page for validation, and then click "Submit to Google."

### Monitoring

The HTML page of this app is monitored using the New Relic agent.
https://staging.newrelic.com/accounts/550352/browser/875789

The AMP page of this app is monitored using the `amp-analytics` component that sends data to the Browser pipeline.

### How to send test AMP data to New Relic

We can now point the `amp-analytics` component towards the [staging BAM router](staging-bam.nr-data.net). This sends data that will show up in Insights, both in [Page View](https://staging-insights.newrelic.com/accounts/550352/query?hello=overview&query=SELECT%20mobileOptimized%20from%20PageView%20where%20mobileOptimized%20IS%20NOT%20NULL%20since%2060%20minutes%20ago) and [BrowserInteraction](https://staging-insights.newrelic.com/accounts/550352/query?query=SELECT%20mobileOptimized%20FROM%20BrowserInteraction%20WHERE%20%60appId%60%20%3D%20875789%20%20where%20mobileOptimized%20IS%20NOT%20NULL%20SINCE%2030%20minutes%20AGO%20LIMIT%20500) tables with the `mobileOptimized` attribute set to `amp`.

#### Using localhost

Run the app locally and load the AMP page. Apparently, the `amp-analytics` component sends data to the BAM router's `amp` endpoint even if the page isn't loaded using Google's AMP CDN.

View data in Insights:
https://staging-insights.newrelic.com/accounts/550352/query?query=SELECT%20mobileOptimized%20FROM%20BrowserInteraction%20WHERE%20%60appId%60%20%3D%20875789%20%20where%20mobileOptimized%20IS%20NOT%20NULL%20SINCE%2030%20minutes%20AGO%20LIMIT%20500

#### Using a deployed site

A deployed site also sends data to the router's `amp` endpoint even if not loaded from Google's AMP CDN.

View data in Insights:
https://staging-insights.newrelic.com/accounts/550352/query?query=SELECT%20mobileOptimized%20FROM%20BrowserInteraction%20WHERE%20%60appId%60%20%3D%20875789%20%20where%20mobileOptimized%20IS%20NOT%20NULL%20SINCE%2030%20minutes%20AGO%20LIMIT%20500

## Developing this test app

### Local dev
 - From the root directory run `python -m SimpleHTTPServer`
 - Go to [localhost:8000]() in the browser
 - Add `#development=1` to the end of the URL to get AMP validation

### Publish to GH-pages
`git push origin master`

 ### Visit the deployed site at https://bookstein.github.io/amp/ !