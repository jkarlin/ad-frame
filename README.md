# AdFrame

Developers can tune performance and behavior for first party content, but they have less control over third-party content, such as ads, on their site. The AdFrame policy helps site owners specify resource and feature constraints that are enforced on ads by the browser.

APIs (e.g., [FeaturePolicy](https://github.com/WICG/feature-policy), [TransferSizePolicy](https://github.com/WICG/transfer-size), and CPUPolicy) are being developed to help developers control the impact that cross-origin frames can have on user experience. They limit the resources that frames have access to (CPU and network) and they limit access to APIs (vibrate, navigation, downloads) that can be used to annoy users. When used together to form a cohesive frame policy, these controls can help to return control over user experience back to the content creators.

Many sites will need a frame policy that governs the page's advertisements. How much data should ads use? How much CPU is too much? When should an ad be able to navigate the top frame? These are questions that are difficult to answer on a site-by-site basis, but when collectively agreed upon as a web standard, can help to bring about a reasonable and predictable experience to the entire ads ecosystem. 

With a standard ads policy (called the AdFrame policy), users, publishers, and ad networks will know what to expect. Publishers can take comfort in knowing that the ads on their page won't ruin the page experience. Users can relax knowing that the ads aren't using egregious battery, CPU, or mobile data. Finally, advertisers will be aware of the constraints, so they can pull in the long tail of heavy ads and only serve compliant ones.

To apply the AdFrame policy, web developers will simply need to label frames (via attributes or response headers) as ads and the browser will take care of applying and enforcing the AdFrame policy. Examples are provided below.

# Marking an AdFrame

AdFrames can be marked with an attribute on the iframe:
```html
<iframe src='http://example.com/' adframe=enforce>
```
To run in report-only mode, instead of ‘enforce’ use ‘report’, e.g.,
```html
<iframe src=’http://example.com/’ adframe=report>
```
Or via a response header using JSON formatting:

```http
AdFrame: {'default':’report’, ‘example.com’:’enforce’, ‘*.self':’none’, 'self':’none’, ‘report-to’: [‘endpoint’]}
```

Use the response header if third-party scripts create the frames that the ads are contained within.

# Reporting
AdFrame is a frame policy. It utilizes other APIs (e.g., TransferSizePolicy and FeaturePolicy) for enforcement. As such, any reported violations will happen through those features via the Reporting API.

# Report Only Mode
If ad-frame is set to ‘report’, then only the subset of policies that can run in report-only mode are applied, and they’re applied in report-only mode. At the moment, only TransferSizePolicy can run in report-only mode.

# The AdFrame Policy
The specifics of the AdFrame Policy are still evolving as it’s currently in development. But so far we have:
TransferSizePolicy
Ads should not consume a lot of data, especially on mobile networks. In some markets, the cost to the user to download an ad is greater than the value of the ad to the publisher! 

On mobile networks: capped at 300KB
On other networks: capped at 500KB

# Future Growth
Over time we intend to add additional policies such as FeaturePolicy and CPUPolicy.
