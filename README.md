# The Privacy Sandbox enrollment attestation model

## Introduction

In [A Potential Privacy Model for the Web: Sharding Web Identity](https://github.com/michaelkleber/privacy-model), we outlined a particular threat to user privacy: the ability to widely track and share cross-site identities[^1]. The Privacy Sandbox initiative was introduced to help create technologies that protect user privacy online while providing ways to build thriving digital businesses. Core technologies introduced by the Privacy Sandbox, such as the relevance and measurement APIs, are designed to have structural limitations enforced within each API, to meet privacy goals and prevent abuse. 

In addition to these technical privacy guardrails, we [announced our plans](https://developer.chrome.com/blog/announce-enrollment-privacy-sandbox/) to launch a new developer enrollment process which will verify companies before they can use the APIs, as an additional layer of protection for user privacy. As part of this process, we propose an attestation model, which requires developers to agree to restrictions around the usage of these services to prevent re-identification of users across sites. 

[^1]: Even though the Potential Privacy Model for the Web document was written with the web in mind, it still describes the core privacy threat that the Privacy Sandbox as a whole is designed to protect, and we apply the principle to both web and Android.

## Goals

The goals for designing the attestation model include:

* Deter entities from abusing the Privacy Sandbox APIs to re-identify users across sites.
* Inform external parties (end users, key opinion formers, regulators, etc) of the guarantees that companies have made as to how they'll use the Privacy Sandbox APIs.

### Non-goals

The attestation model does not seek to enforce or validate the truthfulness of the attestations made by developers, beyond providing transparency to the ecosystem with respect to who has enrolled and what attestations they have made. The attestation model does not cut off access to the APIs of those who may violate the attestations.  There are well-established consequences in much of the world for a company misrepresenting what it does with people's data.

Today, the attestation model does not seek to provide information to users within the browser or device in real-time about a developer's attestations. This may be considered for a future iteration. 

## Design

The attestation model consists of two main components:

* Core privacy attestations as well as potentially API-specific attestations
* Validation and transparency framework

Within this explainer, we use the term "site" which is inclusive of websites, web apps, and Android apps. Attestations for the Privacy Sandbox APIs on Android are required in addition to the Play store policy. 

### Core privacy attestations

To start, we propose one core privacy attestation that is applicable to all APIs. A foundational attestation that meets the core privacy protection needs of the APIs provides opportunity for expansion if more API-specific attestations are needed. Broadly speaking, fewer attestations  means less friction and time spent by developers completing enrollment. The following proposed attestation is the primary guarantee that developers must agree to:

```
ServiceNotUsedForIdentifyingUserAcrossSites = True
```

In addition to the computer readable version of the attestations, a natural language version is included in the attestation file in one or more languages. Providing a natural language version of the attestation strengthens the legal representation claim for the entity providing the attestation.

The English version of the core privacy attestation is:

> The attesting entity states that it will not use the Privacy Sandbox
APIs or services for the purpose of learning that you are the same
user across different sites or apps, and that it will not otherwise
circumvent the privacy protections of the Privacy Sandbox.

In the attestation file, a language marker must be provided for any translations of the natural language version so that a user agent or app can select the appropriate language version to display to the user if desired. Machine-based translations can provide translations for other languages that are not included in the attestation file.

Let's take a look at the parts that make up the proposed core privacy attestation:

<table>
  <tr>
   <td><code>Service</code>
   </td>
   <td>A Service includes both the client side APIs in the Privacy Sandbox and any related servers that all together represent the API.
   </td>
  </tr>
  <tr>
   <td><code>NotUsed</code>
   </td>
   <td>The data provided by the Privacy Sandbox API is not used for the purpose stated in the attestation.
   </td>
  </tr>
  <tr>
   <td><code>IdentifyingUserAcrossSites</code>
   </td>
   <td>The act of utilizing the API's data for the purpose of learning the identity of an individual user or device across different sites or apps.
   </td>
  </tr>
</table>

The core attestation adheres to several key principles:

**Flexible coverage:** Ad tech systems are sophisticated and complex, so it's critical that the attestation is flexible enough to cover abuses wherever they might happen: within the client side API or within any component or connection that makes up the service. 

**Practicality:** It's more practical for sites to list commitments for abuse prevention, rather than listing every way a site will use any particular service. As developers become more familiar with these technologies, usage may change.

**Focus on outcomes:** Instead of identifying all of the mechanisms by which abuse can occur, the attestation focuses on the result of the abuse, such as re-identifying users across sites. This simplifies the attestation and its description, and allows for comprehension across various audiences such as regulators, key opinion formers, and consumers.

Additional attestations may be added to individual APIs as scenarios arise.

### Validation and transparency framework

Developers who submit an enrollment form are then sent a file that contains the attestations for the APIs they requested to use. To complete enrollment, the developer places the file in a site's public `.well-known` location. For example, if they enroll `example.com`, they need to place the attestation file at `https://foo.com/.well-known/privacy-sandbox-attestations`. 

The attestation file serves these purposes:

* Make attestations public and easily accessible directly through the enrolled party, a source not owned by Google.
* Validate domain ownership with the help of an included ownership token.

File fields:

<table>
  <tr>
   <td><code>attestation_parser_version</code>
   </td>
   <td>Parser version to allow for future format changes.
   </td>
  </tr>
  <tr>
   <td><code>attestation_version</code>
   </td>
   <td>Version of attestation file. Value will increment by 1 each time the ad tech changes the APIs they use or set different attestations. This allows the maintenance of a historical record of attestations.
   </td>
  </tr>
  <tr>
   <td><code>privacy_policy</code>
   </td>
   <td>Link to an organization's privacy policy.
   </td>
  </tr>
  <tr>
   <td><code>ownership_token</code>
   </td>
   <td>Allows the platform to verify that the attestation file is added to the site by the entity it is issued to and ensures that the entity that completes the enrollment process owns the site that they enroll.
   </td>
  </tr>
  <tr>
   <td><code>issued_seconds_since_epoch</code>
   </td>
   <td>Attestation file issuance timestamp (expressed as seconds since the Unix epoch).
   </td>
  </tr>
  <tr>
   <td><code>expiry_seconds_since_epoch</code>
   </td>
   <td>Attestation file expiry timestamp (expressed as seconds since the Unix epoch).
   </td>
  </tr>
  <tr>
   <td><code>enrollment_id</code>
   </td>
   <td>Enrollment ID issued to the ad tech. 
   </td>
  </tr>
  <tr>
   <td><code>platform_attestations</code>
   </td>
   <td>List of per-API attestations for each platform. This contains one field for each API that is subject to attestation requirements. The list of APIs may differ by platform based on API availability on the given platform. There is a core attestation <code>ServiceNotUsedForReidentification</code> which applies to all known APIs. There may be new attestations added in the future that apply to one or more APIs. For each API, access is granted only if all the required attestations for it are declared in this file. 
   </td>
  </tr>
  <tr>
   <td><code>attribution_reporting_api</code>
   </td>
   <td>Attestations for the Attribution Reporting APIs. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean></code> items that indicate the attestation strings the ad tech attests to. 
   </td>
  </tr>
  <tr>
   <td><code>topics_api</code>
   </td>
   <td>Attestations for the Topics API. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean></code> items that indicate the attestation strings the ad tech attests to. 
   </td>
  </tr>
  <tr>
   <td><code>protected_audience_api</code>
   </td>
   <td>Attestations for the Protected Audience APIs. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean></code> items that indicate the attestation strings the ad tech attests to.
   </td>
  </tr>
  <tr>
   <td><code>shared_storage_api</code>
   </td>
   <td>Attestations for the Shared Storage API. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean></code> items, that indicate the attestation strings the ad tech attests to.
   </td>
  </tr>
  <tr>
   <td><code>private_aggregation_api</code>
   </td>
   <td>Attestations for the Private Aggregation API. This entry is a list, which contains a list of <code>&lt;Attestation, Boolean></code> items, that indicate the attestation strings the ad tech attests to. 
   </td>
  </tr>
</table>

The attestation file is verified for consistency regularly to ensure the language included within the file remains unchanged and on the ad tech's server in the correct location. It's important to ensure that developers do not maintain multiple versions of the attestation file. In the future, it may be helpful for browsers or devices to routinely observe attestation files so that differences from expectation attestations could be highlighted to the ecosystem. 

We strive to bring enhanced transparency to the use of the Privacy Sandbox relevance and measurement APIs. Therefore, besides placing the file on a server, we will generate reports that share enrollment and attestation related information for each enrolled developer. These reports will be designed to help a wide variety of interested parties gain more understanding and context around who is using these APIs. 

## Attestation enrollment scenarios

The developer attestation part of Privacy Sandbox Enrollment is designed to prevent abuse of Privacy Sandbox APIs in scenarios where developers purposely work around the limits in the APIs designed to learn whether the user is the same user across sites or apps. The developer attestation covers the use of the Privacy Sandbox APIs and data received from the APIs; it does not cover the use of other data or technologies.

This section contains examples of how the attestation works in practice, based on scenarios that have been raised by members of the ecosystem. This document only describes if the scenarios are consistent with the attestation and makes no other claims about the scenarios described with regards to the Privacy Sandbox.

### Scenario 1: first-party data joins

In this scenario, a user signs into two different sites with the same information; for example, the user’s email address. To illustrate a possible use case, the first site is a news publisher where the user is shown an ad for a pair of shoes. Later, the user visits the site of the shoe company and buys the pair of shoes from the ad. Because both sites have the same email address from the user, the two sites are able to compare and confirm that it was the same user who both viewed the ad and made the purchase. This comparison does not depend on the use of any Privacy Sandbox technologies.

In parallel, the publisher and shoe company work with an ad tech company to measure ad conversions on their sites using the Attribution Reporting API (ARA). Using the ARA, the ad tech records that a specific ad impression on the publisher’s site led to a general "purchase" type event on the shoe company's site, without revealing specifically which user on the shoe site made the purchase or what they purchased.

The conversion report from ARA with the ad impression can then be compared with the publisher's other records of ad impressions to see that the publisher already has a record of the conversion through the comparison with the user's email address. The publisher can then remove the duplicate conversion report; in other words deduplicate the conversion, so that it does not double count. Relatedly, it can use this information to potentially build models about the rate of duplicate conversions.

This scenario is consistent with the attestation as the developer already has a linked identity through a source outside of the Privacy Sandbox.

### Scenario 2: third-party cookie use while third-party cookies are still supported

In this scenario, the developer already has access to a third-party cookie which provides a cross-site identifier. Similar to the first-party data join scenario, an identity linkage through a third-party cookie can be used to create a baseline to help learn and optimize the use of the Privacy Sandbox APIs. Such evaluation is an important part of the transition to a post-third-party cookie deprecation environment.

As the developer already has a cross-site identity via a third-party cookie, this scenario would be considered consistent with the attestation during the transitionary period while third-party cookies are generally available. 

### Scenario 3: API debugging data joins

In this scenario, the developer is using the debugging channels of the Privacy Sandbox APIs to perform the development and testing of their integration with the Privacy Sandbox APIs. The Privacy Sandbox debugging channels are only available under certain circumstances where third-party cookies or AdID are available to provide a cross-site identity across sites or apps. 

As the debugging information is thus only available with such cross-site identifiers, the use of the API debugging channel information with those identifiers is consistent with the attestation.


### Scenario 4: model training

When training models for bidding and other related use cases, there may be inferences made from the various input data sources that include Privacy Sandbox API data that could potentially recognize the same user across sites. 

If the information from the model is not used to produce a persistent profile of a user that contains a cross-site identity outside of the model, then the inference performed within the model would be considered consistent with the attestation.

### Scenario 5: downstream receivers of topics

An enrolled caller of the Topics API is typically doing so in order to include the topic in an ad slot opportunity that is sent to other parties such as potential ad buyers or other exchanges. The receiving party for this opportunity would receive the topic with the rest of the ad request information.

The downstream receivers of topics beyond the initial Topics API caller do not need to be enrolled, though many are likely to be enrolled for other API usage. A list of Privacy Sandbox enrollees will be provided programmatically as part of the program's transparency efforts, which would allow an interested caller of the Topics API to check if the recipient they are sending a topic to is enrolled, if the caller should want to.

## Release Notes

Refer to the complete guidance available on GitHub for [previous releases](https://github.com/privacysandbox/attestation/commits/main/README.md) regarding updates to the Attestation explainer. We have updated our [Developer enrollment guide](https://github.com/privacysandbox/attestation/blob/main/how-to-enroll.md) accordingly.

<table>
  <thead>
    <tr>
      <th>Enrollment Attestation Explainer Version</th>
      <th>Date Updated</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1.0</td>
      <td>06.01.23</td>
    </tr>
    <tr>
      <td>2.0</td>
      <td>10.05.23</td>
    </tr>
  </tbody>
</table>
