# The Privacy Sandbox enrollment attestation model

## Introduction

In [A Potential Privacy Model for the Web: Sharding Web Identity](https://github.com/michaelkleber/privacy-model), we outlined a particular threat to user privacy: The ability to widely track and share cross-site identities[^1]. The Privacy Sandbox initiative was introduced to help create technologies that protect user privacy online while providing ways to build thriving digital businesses. Core technologies introduced by the Privacy Sandbox, such as the relevance and measurement APIs, are designed to have structural limitations enforced within each API, to meet these privacy goals and prevent abuse. 

In addition to these technical privacy guardrails, we [announced our plans](https://developer.chrome.com/blog/announce-enrollment-privacy-sandbox/) to launch a new developer enrollment process which will verify companies before they can use the APIs, as an additional layer of protection for user privacy. As part of this process, we propose an attestation model, which requires developers to agree to restrictions around the usage of these services to prevent re-identification of users across sites. 

[^1]: Even though the Potential Privacy Model for the Web document was written with the web in mind, it still describes the core privacy threat that the Privacy Sandbox as a whole is designed to protect, and we apply the principle to both web and Android.

## Goals

The goals for designing the attestation model include:

*   Deter entities from abusing the Privacy Sandbox APIs to re-identify users across sites.
*   Inform external parties (end users, key opinion formers, regulators, etc) of the guarantees that companies have made as to how they'll use the Privacy Sandbox APIs.

### Non-goals

The attestation model does not seek to enforce or validate the truthfulness of the attestations made by developers, beyond providing transparency to the ecosystem with respect to who has enrolled and what attestations they have made. The attestation model does not cut off access to the APIs of those who may violate the attestations.  There are well-established consequences in much of the world for a company misrepresenting what it does with people's data.

Today, the attestation model does not seek to provide information to users within the browser or device in real-time about a developer's attestations. This may be considered for a future iteration. 

## Design

The attestation model consists of two main components:

*   Core privacy attestations as well as potentially API-specific attestations
*   Validation and transparency framework

Within this explainer, we use the term "site" which is inclusive of websites, web apps, and Android apps. Attestations for the Privacy Sandbox APIs on Android are required in addition to the Play store policy. 

### Core privacy attestations

To start, we propose one core privacy attestation that is applicable to all APIs. A foundational attestation that meets the core privacy protection needs of the APIs provides opportunity for expansion if more API-specific attestations are needed. Broadly speaking, fewer attestations means less friction and time spent by developers completing enrollment. The following proposed attestation is the primary guarantee that developers must agree to:

```
ServiceNotUsedForIdentifyingUserAcrossSites = True
```

In addition to the computer readable version of the attestations, a natural language version would be included in the attestation file in one or more languages. Providing a natural language version of the attestation strengthens the legal representation claim for the entity providing the attestation.

The equivalent English version of the core privacy attestation would be:

> This entity that is using the Privacy Sandbox states that it will not use information from Privacy Sandbox APIs or services in a way that allows that entity to identify you or your device across sites or apps. 

In the attestation file, a language marker will need to be provided for any translations of the natural language version so that a user agent or app can select the appropriate language version to display to the user if desired. Machine based translations could provide translations for other languages that are not included in the attestation file.

Let's take a look at the parts that make up the proposed core privacy attestation:
<table>
  <tr>
   <td><strong>Service</strong></td>
   <td>A 'service' reflects both client side APIs in the Privacy Sandbox and any related servers that altogether, colloquially represent an API. For example, the Protected Audience API includes both client side APIs in a browser or Android but also a variety of servers such as the key-value server, which inclusively are considered the <em>service</em>.</td>
  </tr>
  <tr>
   <td><strong>NotUsed</strong></td>
   <td>The public documented privacy goal of the Privacy Sandbox is based on protecting the user from a particular privacy abuse. By having the site provide a negative attestation via <em>NotUsed</em>,  that it is not violating that principle, the attestation can offer more flexibility for how a site can use the data from the services in new and innovative ways while still staying true to the privacy protecting principle we have laid out.</td>
  </tr>
  <tr>
   <td><strong>IdentifyingUser</strong></td>
   <td>The use of <em>Identifying</em> simplifies the description of the outcome that the Privacy Sandbox is trying to protect users from. <em>Identifying</em> here covers the outcome and thus would cover both specific and probabilistic identification, e.g. fingerprinting.  
     <p><em>User</em> help focus the harm that we are protecting to the individual users and makes room for supported ways of using the services e.g. aggregated or k-anonymous. Since we're trying to protect the individual, a device would also be considered an individual for the purposes of the attestation, given the common representation of a single device as a representation of the device's user.</td>
  </tr>
  <tr>
   <td><strong>AcrossSites</strong></td>
   <td><em>AcrossSites</em> scopes the outcome we are trying to protect to when the action results in identifying the user or device across different sites or apps as defined by the Privacy Sandbox boundaries.</td>
  </tr>
</table>

The core attestation adheres to several key principles:

* **Flexible coverage:** Ad tech systems are sophisticated and complex, so it's critical that the attestation is flexible enough to cover abuses wherever they might happen: Within the client side API or within any component or connection that makes up the service. 
* **Practicality:** It's more practical for sites to list commitments for abuse prevention, rather than listing every way a site will use any particular service. As developers become more familiar with these technologies, usage may change.
* **Focus on outcomes:** Instead of identifying all of the mechanisms by which abuse can occur, the attestation focuses on the result of the abuse (such as, re-identifying users across sites). This simplifies the attestation itself and the description of the attestation, and also allows for comprehension across various audiences, such as regulators, key opinion formers (KOFs), and consumers. 

Additional attestations may be added to individual APIs as scenarios arise.

### Validation and transparency framework

Developers who submit the enrollment form will be sent a file that contains the attestations for the APIs they are requesting to use. To complete enrollment, the developer must place the file in a site's public `.well-known` location. For example, if they enroll with `example.com`, they need to place the attestation file at `https://example.com/.well-known/privacy-sandbox-attestations`. 

The attestation file serves two purposes: 
*   Make attestations public and easily accessible directly through the enrolled party, a source not owned by Google
*   Validate domain ownership with the help of an included ownership token

File fields:
<table>
  <tr>
   <td><code>attestation_parser_version</code></td>
   <td>(Internal) Parser version to allow for future format changes.</td>
  </tr>
  <tr>
   <td><code>attestation_version</code></td>
   <td>Version of attestation file. Value will increment by 1 each time the ad tech changes the APIs they use or set different attestations. This allows the maintenance of a historical record of attestations.</td>
  </tr>
  <tr>
   <td><code>privacy_policy</code></td>
   <td>Link to organization privacy policy</td>
  </tr>
  <tr>
   <td><code>ownership_token</code></td>
   <td>Allows the platform to verify that the attestation file is added to the site by the entity it is issued to and ensures that the entity that completes the enrollment process owns the site that they enroll.</td>
  </tr>
  <tr>
   <td><code>issued_seconds_since_epoch</code></td>
   <td>Attestation file issuance timestamp (expressed as seconds since the unix epoch)</td>
  </tr>
  <tr>
   <td><code>expiry_seconds_since_epoch</code></td>
   <td>Attestation file expiry timestamp (expressed as seconds since the unix epoch)</td>
  </tr>
  <tr>
   <td><code>enrollment_id</code></td>
   <td>Enrollment ID issued to the ad tech.</td>
  </tr>
  <tr>
   <td><code>platform_attestations</code></td>
   <td>List of per-API attestations for each platform. This contains one field for each API that is subject to attestation requirements. The list of APIs may differ by platform based on API availability on the given platform. There is a core attestation <code>ServiceNotUsedForReidentification</code> which applies to all known APIs. There may be new attestations added in the future that are applicable to one or more APIs. For each API, access is granted only if all the required attestations for it are declared in this file. </td>
  </tr>
  <tr>
   <td><code>attribution_reporting_api</code></td>
   <td>Attestations for the Attribution Reporting APIs. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean&gt;</code> the attestation strings the ad tech attests to. </td>
  </tr>
  <tr>
   <td><code>topics_api</code></td>
   <td>Attestations for the Topics API. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean&gt;</code> the attestation strings the ad tech attests to. </td>
  </tr>
  <tr>
   <td><code>protected_audience_api</code></td>
   <td>Attestations for the Protected Audience APIs. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean&gt;</code> the attestation strings the ad tech attests to.</td>
  </tr>
  <tr>
   <td><code>shared_storage_api</code></td>
   <td>Attestations for the Shared Storage API. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean&gt;</code> the attestation strings the ad tech attests to.  </td>
  </tr>
  <tr>
   <td><code>private_aggregation_api</code></td>
   <td>Attestations for the Private Aggregation API. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean&gt;</code> the attestation strings the ad tech attests to. </td>
  </tr>
</table>

The attestation file is verified for consistency regularly to ensure the language included within the file remains unchanged and on the ad tech's server in the correct location. It's important to ensure that developers do not maintain multiple versions of the attestation file. In the future, it may be helpful for browsers or devices to routinely observe attestation files so that differences from expectation attestations could be highlighted to the ecosystem. 

We strive to bring enhanced transparency to the use of the Privacy Sandbox relevance and measurement APIs. Therefore, besides placing the file on a server, we will generate reports that share enrollment and attestation related information for each enrolled developer. These reports will be designed to help a wide variety of audiences, including end users, governments and KOFs, gain more understanding and context around who is using these APIs. 
