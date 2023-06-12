# Enroll for the Privacy Sandbox

To access the Privacy Sandbox relevance and measurement APIs on [Chrome](https://developer.chrome.com/docs/privacy-sandbox/) and [Android](https://developer.android.com/design-for-safety/privacy-sandbox), developers need to enroll with the Privacy Sandbox. As of June 2023, this includes Attribution Reporting, Protected Audience, Topics, Private Aggregation, and Shared Storage.

_**Note:** this process replaces the previous Android-only enrollment process._


## What is enrollment?

[Developer enrollment](https://developer.chrome.com/blog/announce-enrollment-privacy-sandbox/) provides a mechanism to verify the entities that call these APIs and gather developer-specific data needed to properly configure and use the Privacy Sandbox APIs. This enrollment process adds an additional layer of protections on top of the structural restrictions enforced within each API, by adding transparency to who is collecting data, and mitigating attempts to misuse the APIs to gather more data than intended. To provide auditable transparency, enrollment information about the company will be made public.

**Enrollment becomes a requirement in August 2023, and from that point on you won't be able to call the APIs until you have successfully completed the enrollment and attestation process.**

_**Note:** Enrollment applies only to companies that will call one or more of the Privacy Sandbox relevance and measurement APIs. Publishers and/or other companies that are customers of ad techs do not need to enroll._


### How to Enroll

To enroll, developers must [complete the enrollment form](https://goo.gle/privacy-sandbox-enroll-form). The form will request the following pieces of information:

*   Business contact information
*   D-U-N-S number for your organization
*   APIs that you plan to use and API configuration information

Developers also need to agree to attestations about their usage of the enrolled Privacy Sandbox APIs.

_**Note:** Android developers who previously enrolled must migrate to this new enrollment process. We will reach out in June or July to individually provide instructions relevant to your specific use-case._

For Android local testing only from now until August: If you want to start testing the Attribution Reporting API on local-only traffic before August, use this [form](https://docs.google.com/forms/d/1bPtf4f6j9LkUjWg8ODosRzr6a2q98YLe7BsN2BGsKzE/edit). After August, this process will not be needed.

After your enrollment form has been submitted, we'll review and process your application. Once the review is complete, you will be sent a confirmation email with a unique developer enrollment account ID and the attestation file that you must place on your [site](https://web.dev/same-site-same-origin/) (scheme eTLD+1, such as `example.com`).


### Before you begin

Before starting the process, make sure you have a [D-U-N-S number](https://www.dnb.com/duns.html) for your organization. This is a unique nine digit number provided by Dun & Bradstreet that is used to identify your business, and is checked as part of the Privacy Sandbox enrollment verification process. If your company has been issued multiple D-U-N-S numbers, please provide the highest level one that represents your overall corporate entity. If your business is not a legal entity, you will not be able to obtain a D-U-N-S number.

To check if your company already has an assigned D-U-N-S number, or to obtain a new D-U-N-S number, send in a request via the [enrollment form](https://goo.gle/privacy-sandbox-enroll-form). Google has a complimentary, expedited D&B request process available for this purpose. Once you've submitted, we will send you an email with a unique, one-time only link to visit the Google-specific landing page to submit your business details. If you close out of the landing page before submitting your information, you won't be able to access it anymore and will need to request another one from Google.

Once completed, it takes approximately 5 to 7 business days to receive your D-U-N-S number. As part of their process, Dun & Bradstreet may contact you to gather additional information.

If you are an individual and not affiliated with an organization, or your organization is not able to obtain a D-U-N-S number, you may enroll as an individual on the enrollment form.

After your enrollment is processed, you'll receive a confirmation email that includes an enrollment account ID and an attestation file, which will need to be placed on your site. [Android developers](#android-developer-data) can provide the enrollment ID to app developers so apps can set granular access control. See [details here](https://developer.android.com/design-for-safety/privacy-sandbox/setup-api-access?version=preview#configure-api-specific).

_**Note:** All information (excluding personally identifiable information) collected during developer enrollment may be included in Privacy Sandbox reports. These reports will be publicly available._


## Enroll your site, SDK, or Android app

During enrollment, you need to provide a site, SDK, or app package name that you'll use to call the APIs.

How you enroll depends on how the Privacy Sandbox APIs are called:

*   If you're a web developer and your site calls the Privacy Sandbox APIs directly, you should provide your site in enrollment.
*   If you're an app developer and your app calls the Attribution Reporting and/or Protected Audience APIs directly, you should provide your site during enrollment.
*   If you're an app developer and your app calls the Topics API directly, you need to enroll your app package. You do not need to provide an SDK name unless you also own and use an SDK.
*   If you're an app developer and you delegate your ads functionality entirely to an SDK, you **do not** need to go through the enrollment process.
*   If you're an SDK developer, provide your SDK name in enrollment. If your SDK uses the Attribution Reporting and/or Protected Audience APIs, provide your site in enrollment as well. Apps that use your SDK don't need to enroll separately, unless they call Privacy Sandbox APIs directly.
*   If you are using the Attribution Reporting API on Android immediately (for local testing or at-scale testing), you will need to fill out [this form](https://docs.google.com/forms/d/1bPtf4f6j9LkUjWg8ODosRzr6a2q98YLe7BsN2BGsKzE).

Every site or SDK that calls the Privacy Sandbox APIs requires a unique enrollment and needs to attest individually.  Apps that call the Privacy Sandbox APIs directly may be included in a single enrollment. If you plan to call multiple APIs, specify each one during the enrollment process.

For Topics on Android, you can provide a single SDK or one or more app package names with your enrollment.


### Multiple enrollments for a single entity

Larger and more complex entities that have multiple, unique products may apply for more than one enrollment. For example, if your company has an SSP and a DSP line of business you may qualify for multiple enrollments. Each product must have separate sites from which to call the APIs.

If you want to enroll more than one site or SDK, fill out the [Multi Enrollment Request Process form](https://goo.gle/privacy-sandbox-enroll-multi) in addition to submitting the normal enrollment form for the first enrollment you are requesting. Once submitted, the application will be reviewed and you will receive an email when the review process is complete.


## Upload your attestation file

Once you've enrolled and passed the verification process, we'll send a file that contains the API-specific attestations to the email address you've provided. To complete enrollment, the developer must place the file in a site's public `.well-known` location.

For example, if you enroll with `example.com`, place the attestation file at `https://example.com/.well-known/privacy-sandbox-attestations.json`.

Developers must abide by the attestations and keep the attestation file in place for the duration of the enrollment. Attestation files are routinely verified, and failure to keep them in place will cause a terminal state (API calls will fail) until the file is restored.

For Topics on Android attestations, app and SDK developers need to agree to the attestation within the enrollment form and do not need to place an attestation file on their server unless they are using other Privacy Sandbox APIs.

[Learn more](https://github.com/privacysandbox/attestation) about Attestations.


## Android developer data

Entities that intend to use the Privacy Sandbox APIs on Android  are sent an enrollment account ID, which can be included in an app's [AdServices configuration](https://developer.android.com/design-for-safety/privacy-sandbox/setup-api-access#configure-adservices). This allows app developers to have fine-grained control over the ad techs their app or SDKs interact with.


## FAQ


### Enrollment questions

Q: What if I am not enrolled by August 2023? \
A: Enrollment will become a requirement from August 2023. If you are not enrolled by then, you will not be able to call the APIs on Chrome or Android. If you have enrolled for only Chrome and/or Android, you will be able to call the APIs on the platform you have enrolled.

Q: How do I update enrollment information? \
A: You can update your enrollment information using the [enrollment form](https://goo.gle/privacy-sandbox-enroll-form). Updates will replace previous responses.

Q: As an app developer, can I verify if an SDK or site has enrolled? \
A: After the enforcement deadline in August 2023, we will publish a list of currently enrolled sites and SDKs for verification purposes, along with their enrollment IDs.

Q: How long will it take to have my enrollment approved? \
A: Enrollment reviews are completed once the Enrollment form has been correctly filled out in its entirety. Entering correct information on your original submission and responding to any inquiries from the Google tech support team in a timely manner helps speed up the process.

Q: Do I need to enroll to test against local development environments ? \
A: No, you do not need to enroll if you are only testing with local traffic, with one exception: If you are testing the Measurement APIs locally on Android before August, you will have to enroll via a separate [Google form](https://docs.google.com/forms/d/e/1FAIpQLSe-mnC_Fk0IfdSnp1Fptb2tUnRrkzTxqgNb9tQV2JQ21AYjzw/formResponse?resourcekey=0-VQVQg91CmG4OsUBbetT3oQ). After August, you will no longer need to enroll for local-only testing of the APIs on either platform.

Q: Do I need to do this if I previously enrolled under the Android Enrollment Program? \
A: Yes, this new process will need to be completed. You will receive an email with instructions on how to migrate your enrollment to the new process.

Q: Do I need to enroll my staging, beta, QA, or test environments? \
A: Your staging, beta, QA and test environments will be automatically enrolled if they use the same site as your production environment. Separately, you can test locally (on devices you have physical control over) without enrolling.


### Attestation questions

Q: Do I need to update my Attestation file if I decide to include more APIs in my enrollment at a later date? \
A: Yes, you will need to update your enrollment and as part of this process, you will receive an updated Attestation file that must be placed at the `.well-known` attestation path on your site before you can call the new APIs.


### D-U-N-S questions

Q: How long will it take to receive my D-U-N-S number? \
A: The process should take 5 to 7 business days. If it's taking longer than that, please reply back to the email you received when you submitted your enrollment form (the email that contains your unique D&B link).

Q: What if I or my organization can't obtain a D-U-N-S number? Can I still enroll? \
A: If you can't obtain a D-U-N-S number, you may enroll as an individual on the Google form.

Q: Can I submit multiple enrollments under the same D-U-N-S number? \
A: Yes, if you are applying for multiple enrollments via the Multi Enrollment Request form you may use the same D-U-N-S number on each enrollment.

Q: Can I use the Dun and Bradstreet D-U-N-S number request form on D&B's website? \
A: We provide free access to an expedited process not available directly through D&B's website. However, you can also still apply through their usual channels.

Q: Can companies outside the USA get a D-U-N-S number? \
A: D-U-N-S numbers are widely supported and you can find country-specific help [on their website](https://ask.dnb.co.uk/help/duns-number/international-duns-faq).
