# Enroll for the Privacy Sandbox

To access the Privacy Sandbox relevance and measurement APIs on [Chrome](https://developer.chrome.com/docs/privacy-sandbox/) and [Android](https://developer.android.com/design-for-safety/privacy-sandbox), developers need to enroll with the Privacy Sandbox. This includes Attribution Reporting, Protected Audience, Topics, Private Aggregation, and Shared Storage.

## What is enrollment?

[Developer enrollment](https://developer.chrome.com/blog/announce-enrollment-privacy-sandbox/) provides a mechanism to verify the entities that call these APIs, and to gather the developer-specific data needed to properly configure and use the Privacy Sandbox APIs. This enrollment process adds an additional layer of protections on top of the structural restrictions enforced within each API, by adding transparency to who is collecting data, and mitigating attempts to misuse the APIs to gather more data than intended. To provide auditable transparency, enrollment information about the company will be made public.

To ensure companies have ample time to complete the enrollment and attestation process, we now expect to **begin enforcement in mid-October, with the [Chrome 118 Stable release](https://chromiumdash.appspot.com/schedule)**. For the pre-Stable Chrome channels, enforcement will also begin with their respective 118 releases: late-August for Dev/Canary, mid-September for Beta.

This means **companies must complete enrollment and attestation by mid-October** in order to access the relevance and measurement APIs in Chrome Stable, and **by August in order to access the APIs in the pre-release Dev and Canary Chrome channels**.

Companies who previously enrolled via the original Android enrollment process must migrate to the unified Android and Chrome enrollment process by the mid-October in order to avoid any disruption in access to the APIs.

Companies should plan **at least five weeks to complete the enrollment process**, from the time they submit the enrollment form.  This includes time to address any issues with the form submission or other issues that may arise, and publish their attestation file. This does not include any additional lead time companies may need for internal preparation prior to submitting the form.

### How to enroll

To enroll, developers must [complete the enrollment form](https://goo.gle/privacy-sandbox-enroll-form). The form requests the following information:

*   Business contact information
*   [D-U-N-S number](https://www.dnb.com/duns.html) for your organization
*   APIs that you plan to use and API configuration information

Developers also need to agree to attestations about their usage of the enrolled Privacy Sandbox APIs.

After your enrollment form has been submitted, we'll review and process your application. Once the review is complete, you will be sent a confirmation email with a unique developer enrollment account ID and attestation file.  The file must  must made publicly available from the /`.well-known` path on the [site](https://web.dev/same-site-same-origin/) for which it was enrolled. [Android developers](https://github.com/privacysandbox/attestation/blob/main/how-to-enroll.md#android-developer-data) can provide the enrollment ID to app developers so apps can set granular access control: see Android's [Configure API-specific Ad Services](https://developer.android.com/design-for-safety/privacy-sandbox/setup-api-access?version=preview#configure-api-specific) documentation.

### Before you begin

Before starting the process, make sure you have a [D-U-N-S number](https://www.dnb.com/duns.html) for your organization. This is a unique nine digit number provided by Dun & Bradstreet that is used to identify your business, and is checked as part of the Privacy Sandbox enrollment verification process. If your company has been issued multiple D-U-N-S numbers, please provide the highest level one that represents your overall corporate entity. If your business is not a legal entity, you will not be able to obtain a D-U-N-S number.

To check if your company already has an assigned D-U-N-S number, or to obtain a new D-U-N-S number, make a request via the [enrollment form](https://goo.gle/privacy-sandbox-enroll-form). Google has a complimentary, expedited Dun & Bradstreet request process available for this purpose. Once you've made the request, we will send you an email with a unique, one-time-only link to visit the Google-specific landing page to submit your business details. If you don't complete submitting your information, you will need to request another link from Google.

Once completed, it takes approximately five to seven business days to receive your D-U-N-S number. As part of their process, Dun & Bradstreet may contact you to gather additional information.

If you are an individual and not affiliated with an organization, or your organization is not able to obtain a D-U-N-S number, you may enroll as an individual on the enrollment form.

Note: All information (excluding personally identifiable information) collected during developer enrollment may be included in Privacy Sandbox reports. These reports will be publicly available.

## Enroll your site, Android SDK, or Android app

During enrollment, you need to provide a site, SDK, or app package name that you'll use to call the APIs.

How you enroll depends on how the Privacy Sandbox APIs are called:

*   If you're a web developer and your site calls the Privacy Sandbox APIs directly, you should provide your site in enrollment.
*   If you're an Android SDK developer, provide your SDK name in enrollment. If your SDK uses the Attribution Reporting and/or Protected Audience APIs, provide your site in enrollment as well. Apps that use your SDK don't need to enroll separately, unless they call Privacy Sandbox APIs directly from their own code. If you’re testing Attribution Reporting APIs on Android at scale immediately, you will need to provide all the origins you use.
*   If you're an app developer and your app calls the Attribution Reporting and/or Protected Audience APIs directly, you should provide your site during enrollment.
*   If you're an app developer and your app calls the Topics API directly, you need to enroll your app package. You do not need to provide an SDK name unless you also own and use an SDK.
*   If you're an app developer and you delegate your ads functionality entirely to an SDK, you **do not** need to go through the enrollment process.

Every site or SDK that calls the Privacy Sandbox APIs requires a unique enrollment and needs to attest individually. Apps that call the Privacy Sandbox APIs directly may be included in a single enrollment. If you plan to call multiple APIs, specify each one during the enrollment process.

For Topics on Android, you can provide a single SDK or one or more app package names with your enrollment.

### Multiple enrollments for a single entity

Larger and more complex entities that have multiple, unique products may apply for more than one enrollment. For example, if your company has an SSP and a DSP line of business you may qualify for multiple enrollments. Each product must have separate sites from which to call the APIs.

If you want to enroll more than one site or SDK, fill out the [Multi Enrollment Request Process form](https://goo.gle/privacy-sandbox-enroll-multi) in addition to submitting the normal enrollment form for the first enrollment you are requesting. Once submitted, the application will be reviewed and you will receive an email when the review process is complete.

## Upload your attestation file

Once you've enrolled and passed the verification process, we'll send a file that contains the API-specific attestations to the email address you've provided. To complete enrollment, the developer must make  the file available from the public `.well-known` path on the site that was enrolled.

For example, if you enroll `https://example.com`, place the attestation file at `https://example.com/.well-known/privacy-sandbox-attestations.json`.

Developers must abide by the attestations and keep the attestation file in place for the duration of the enrollment. Attestation files are routinely verified, and failure to keep them in place will cause API calls to fail until the file is restored.

For Topics on Android attestations, app and SDK developers need to agree to the attestation within the enrollment form and do not need to place an attestation file on their server unless they are using other Privacy Sandbox APIs.

[Learn more](https://github.com/privacysandbox/attestation) about attestations.

## Android developer data

Entities that intend to use the Privacy Sandbox APIs on Android are sent an enrollment account ID, which can be included in an app's [AdServices configuration](https://developer.android.com/design-for-safety/privacy-sandbox/setup-api-access#configure-adservices). This allows app developers to have fine-grained control over the ad techs their app or SDKs interact with.

## FAQ

### Enrollment questions

#### 1. What if I am not enrolled by mid-October 2023?
  -  Enrollment will become a requirement from mid-October 2023. If you are not enrolled by then, you will not be able to call the APIs on Chrome or Android. If you have enrolled for only Chrome and/or Android, you will be able to call the APIs on the platform you have enrolled.
#### 2. How do I update enrollment information?
  -  You can update your enrollment information using the [enrollment form](https://goo.gle/privacy-sandbox-enroll-form). Updates will replace previous responses.
#### 3. As an app developer, can I verify if an SDK or site has enrolled?
  -  After the enforcement deadline in mid-October 2023, we will publish a list of currently enrolled sites and SDKs for verification purposes, along with their enrollment IDs.
#### 4. How long will it take to have my enrollment approved?
  -  Enrollment reviews are completed once the enrollment form has been correctly filled out in its entirety. Entering correct information on your original submission and responding to any inquiries from the Google tech support team in a timely manner helps speed up the process.
#### 5. Do I need to enroll to test against local development environments?
  -  No, you do not need to enroll if you are only testing with local traffic.
  -  For local testing, we are providing developer overrides from Chrome 116 with a Chrome flag and CLI switch:
  -  Flag: chrome://flags/#privacy-sandbox-enrollment-overrides
  -  CLI: --privacy-sandbox-enrollment-overrides=https://example.com,https://example.co.uk,...
#### 6. Do I need to re-enroll if I previously enrolled under the Android Enrollment Program?
  -  Yes, the new enrollment process will need to be completed. You will receive an email with instructions on how to migrate your enrollment to the new process.
#### 7. Do I need to enroll my staging, beta, QA, or test environments?
  -  Your staging, beta, QA and test environments will be automatically enrolled if they use the same site as your production environment. Separately, you can test locally (on devices you have physical control over) without enrolling.


### Attestation questions

#### 1. Do I need to update my attestation file if I decide to include more APIs in my enrollment at a later date?
  -  Yes, you will need to update your enrollment. As part of this process, you will receive an updated attestation file that must be made available from the .well-known path on your site, before you can call the new APIs.


### D-U-N-S questions

#### 1. How long will it take to receive my D-U-N-S number?
  -  The process should take five to seven business days. If it's taking longer than that, please reply back to the email you received when you submitted your enrollment form (the email that contains your unique Dun & Bradstreet link).
#### 2. What if I or my organization can't obtain a D-U-N-S number? Can I still enroll?
  -  If you can't obtain a D-U-N-S number, you may enroll as an individual from the enrollment form.
#### 3. Can I submit multiple enrollments under the same D-U-N-S number?
  -  Yes, if you are applying for multiple enrollments via the Multi Enrollment Request form you may use the same D-U-N-S number on each enrollment.
#### 4. Can I use the Dun and Bradstreet D-U-N-S number request form on the Dun & Bradstreet website?
  -  We provide free access to an expedited process not available directly through Dun & Bradstreet's website. However, you can also still apply through their usual channels.
#### 5. Can companies outside the USA get a D-U-N-S number?
  -  D-U-N-S numbers are widely supported and you can find country-specific help [on the Dun & Bradstreet website](https://ask.dnb.co.uk/help/duns-number/international-duns-faq).


### Measurement FAQs 

#### 1. In a single enrollment, how many reporting origins can I register?
  -  With site scoped enrollment, a single enrollment can cover unlimited origins, as long as they are all same-site. However, the one reporting origin per source rate limit ([github link](https://github.com/WICG/attribution-reporting-api/blob/main/EVENT.md#reporting-origin-limits), [Android developers link](https://developer.android.com/design-for-safety/privacy-sandbox/attribution#one-reporting-origin-per-source)) means you are generally limited to one origin per publisher.
#### 2. How is the budget managed between multiple origins for a single enrollment?
  -  See these github issues for changes to privacy budget: https://github.com/WICG/attribution-reporting-api/issues/661 https://github.com/patcg-individual-drafts/private-aggregation-api/issues/23 
  -  Additionally, we have implemented a new rate limit: 1 origin per {source, site/enrollment, 24 hours} ([github link](https://github.com/WICG/attribution-reporting-api/blob/main/EVENT.md#reporting-origin-limits), [Android developers link](https://developer.android.com/design-for-safety/privacy-sandbox/attribution#one-reporting-origin-per-source))
#### 3. Do I need to complete Aggregation Service enrollment and Developer Enrollment to use the Aggregation Service?
  -  Yes, there is currently a separate enrollment process for server elements and the client APIs (eg, Chrome and Android). Both enrollment forms are required to use the Aggregation Service. We are looking to streamline the processes; for now, the Aggregation Service has a [separate form](https://docs.google.com/forms/d/e/1FAIpQLSdkTR53VDIlFvaUo3Flx-RF4xCEZbArq-wjvO0W0O1G-YdHCQ/viewform). 
  -  You will enroll an origin (scheme, host name) during Aggregation Service enrollment which must be part of the same site (scheme, eTLD+1) registered via Developer Enrollment.
  -  Each Aggregation Service enrollment must include the AWS Account’s ID, in which the Aggregation Service will be deployed. Each AWS Account may only be linked to one Aggregation Service enrollment.
#### 4. If any adtech registers an impression with Origin 1 ([www.foo.com](http://www.foo.com)) and conversion with Origin 2 ([www.example.foo.com](http://www.example.foo.com)), is the attribution scoped to enrollmentID or origin?
  -  No, attribution is not allowed cross origins. Attribution scope is at the origin level.
#### 5. Can aggregatable reports from two different origins which belong to the same enrollment be aggregated together in a single batch?
  -  The team is investigating ways to support multiple origins in the same batch. We do not have a timeline for feature availability at this time but please refer to https://developer.chrome.com/docs/privacy-sandbox/status/ for the most up-to-date timelines.
#### 6. Can two different origins from the same enrollment do registration for the same source event?
  -  The 1 origin per {source, site/enrollment} would only allow you to successfully register on one of them. See documentation [here](https://pr-preview.s3.amazonaws.com/WICG/attribution-reporting-api/791/2aa8f51...d81cca3.html#max-source-reporting-origins-per-source-reporting-site).
#### 7. How are the on-device limits e.g. pending sources and max-destination applied in Chrome?
  -  The complete list of rate limits is documented [here](https://wicg.github.io/attribution-reporting-api/#vendor-specific-values).
#### 8. If site A is not enrolled but site B is enrolled, when Chrome pings `siteA.com` and then redirects to `siteB.com`, would this work given `siteA` is not enrolled?
  -  Yes this will work. ARA will ping URLs for redirects even if not enrolled, but sources/triggers will only be registered from enrolled adtechs.
#### 9. Are separate enrollments required for Web>Web vs App>Web bs Web>App?
  -  Different enrollments for Web>Web vs App>Web vs Web>App will not be allowed unless they are different lines of business and therefore must be associated with different enrolled sites.
  -  For most cases, we expect you to use the same enrollment for Web>Web vs cross App and Web because these should share the same attribution scope (meaning, attribution should be deduped across web and app)
#### 10. There is mention of Privacy Budget and Rate limits being a constraining factor to consider when AdTechs enroll. Where can we find the specific details on these limits for privacy budgets and API based rate limits?
  -  ARA limits are listed [here](https://github.com/WICG/attribution-reporting-api/blob/main/params/chromium-params.md) and Private Aggregation limits are listed [here](https://github.com/patcg-individual-drafts/private-aggregation-api#implementation-plan). There is no current privacy budget that applies across APIs. 


### Relevance FAQs 

#### 1. Can one Buyer A with a separate enrollment from another Buyer B create an IG and allow Buyer B to use it in bidding? 
  -  [Chrome]No. However, a Buyer A can delegate creation of an IG to another Site B that has no role as Buyer (details[ here](https://github.com/WICG/turtledove/blob/main/FLEDGE.md#13-permission-delegation)) with the IG retaining Buyer A as the owner.This is the only way we support an IG being created by an entity other than the (eventual) Buyer that submits a bid in the PA auction 
  -  [Android] No. However Android also supports [Delegation](https://developer.android.com/design-for-safety/privacy-sandbox/protected-audience#custom-audience-join:~:text=audience.%0AjoinCustomAudience(audience)%3B-,fetchAndJoinCustomAudience,-()) - where an on device caller can request the platform to fetch a Custom Audience from a specific buyer (Buyer B). The Buyer B must be an enrolled adtech.   
#### 2. How can the parties sending reports via ReportResult or ReportWin check the enrollment status? 
  -  [Chrome] The Chrome implementation is not currently checking for enrollment status of the ReportResult/ReportWin destinations. 
  -  [Android] There is no need for these parties to do anything. The PA flow will check for the enrollment for either of the specified destinations.
#### 3. Do the destinations for event-level engagement reporting, including 3rd party ad measurement partners need to be enrolled?
  -  [Chrome] Yes. Any entities receiving data directly from the privacy sandbox APIs via [engagement reporting](https://github.com/WICG/turtledove/blob/main/Fenced_Frames_Ads_Reporting.md) need to enroll.
  -  [Android] Same as above see [Event Reporting](https://developer.android.com/design-for-safety/privacy-sandbox/protected-audience#reporting_events)
#### 4. The limit of 1000 IGs per Owner per device, does that apply to a single enrollment?
  -  [Chrome] Not necessarily. The limit of 1000 IGs is at the origin level. If adtech decides to use several origins per site/enrollment, then each of those origins gets  its own allocation of 1000 IGs per device
  -  [Android] The Custom Audience limits are applied per User profile (4000) and per app (1000) which is applied to a single enrollment. 
#### 5. What, if any, are the rate limits I need to keep in mind when using the Topics API?
  -  The max number of API usage context domains allowed to be stored per page load: 30. This means that on a page, all domains can invoke the API to "get" topics. But only the first 30 domains are allowed to "set" topics.
  -  For Topics on Android, rate limited at 1 call per app per second (open to feedback). 


### Developer Enrollment FAQs

For Aggregation Service enrollment [see here](#bookmark=kix.3deqz4l395z7)

#### 1. What are the developer enrollment limitations I need to keep in mind when determining my organization’s enrollment structure? 
  -  One site can only be linked to one enrollment.
  -  One enrollment contains only one site. 
  -  SDK specific limitations:
      1. One enrollment may contain multiple SDKs.
      2. A given SDK can only be linked to one enrollment.
  -  Additional enrollments are allowed, but they need to be for independent products/lines of business that are clearly established and publicly verifiable (ie. there is a corresponding public website that explains the specific product). You can not have multiple enrollments for the same product. The attestations apply to each enrollment individually.
#### 2. Is there a specific registration requirement that would necessitate the separate enrollment of a DSP vs an SSP owned by the same entity? 
  -  There is no requirement from Privacy Sandbox to separate enrollment of a DSP vs an SSP owned by the same entity.
#### 3. Can I request multiple enrollments for testing (ex: testing vs prod, pre-prod vs prod)?
  -  No. While adtechs may create multiple environments to test the APIs, enrollment only allows one site to be registered per product (not one enrollment per environment per product). This is to prevent the APIs from being used in ways that they were not intended (ie. garnering more privacy budget than is allocated for a given entity). 
#### 4. How often should the adtech expect to serve the attestation file? Is every API call going to trigger a request to fetch the attestation file?
  -  Privacy Sandbox will run a server side job to fetch the attestation file from enrolled adtechs, and construct an allowlist based on the contents of the file. This allowlist will then be synced with the browser as well as the OS. This job will fetch the file no more than once an hour. 
  -  The intention is for the attestation file to be available publicly. The adtech should expect some fetch requests from external parties like researchers, regulators, users etc (outside of the fetch requests from Privacy Sandbox infra). Adtech must serve the same file to them that they serve to Google. 
  -  Every API call will not trigger a fetch request for the attestation file.
#### 5. Can I use HTTP redirects to serve the attestation file?
  -  No, the attestation file must be located at the .well-known directory at your site. It cannot redirect to another location (eg, a subdomain, or another site)
#### 6. What happens if there are one-off errors in serving the attestation file?
  -  Access would be cut off only if the server checking the attestation file is repeatedly unable to validate it. A single error/serving issue would not cause access to be removed. 