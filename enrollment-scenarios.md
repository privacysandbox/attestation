# Attestation enrollment scenarios

The developer attestation part of Privacy Sandbox Enrollment is designed to prevent abuse of Privacy Sandbox APIs in scenarios where developers purposely work around the limits in the APIs designed to learn whether the user is the same user across sites or apps. The developer attestation covers the use of the Privacy Sandbox APIs and data received from the APIs; it does not cover the use of other data or technologies.

This section contains examples of how the attestation works in practice, based on scenarios that have been raised by members of the ecosystem. This document only describes if the scenarios are consistent with the attestation and makes no other claims about the scenarios described with regards to the Privacy Sandbox.

## Scenario 1: first-party data joins

In this scenario, a user signs into two different sites with the same information; for example the user’s email address. To illustrate a possible use case, the first site is a news publisher where the user is shown an ad for a pair of shoes. Later, the user visits the site of the shoe company and buys the pair of shoes from the ad. Because both sites have the same email address from the user, the two sites are able to compare and confirm that it was the same user who both viewed the ad and made the purchase. This comparison does not depend on the use of any Privacy Sandbox technologies.

In parallel, the publisher and shoe company work with an ad tech company to measure ad conversions on their sites using the Attribution Reporting API (ARA). Using the ARA, the ad tech records that a specific ad impression on the publisher’s site led to a general “purchase” type event on the shoe company’s site, without revealing specifically which user on the shoe site made the purchase or what they purchased.

The conversion report from ARA with the ad impression can then be compared with the publisher’s other records of ad impressions to see that the publisher already has a record of the conversion through the comparison with the user’s email address. The publisher can then remove the duplicate conversion report, in other words de-duplicate the conversion, so that it does not double count. Relatedly, it can use this information to potentially build models about the rate of duplicate conversion rates.

This scenario is consistent with the attestation as the developer already has a linked identity through a source outside of the Privacy Sandbox.

## Scenario 2: third-party cookie use while third-party cookies are still supported

In this scenario, the developer already has access to a third-party cookie which provides a cross site identifier. Similar to the first-party data join scenario, an identity linkage through a third-party cookie can be used to create a baseline to help learn and optimize the use of the Privacy Sandbox APIs. Such evaluation is an important part of the transition to a post third-party cookie deprecation environment.

As the developer already has a cross-site identity via a third-party cookie, this scenario would be considered consistent with the attestation during the transitionary period while third-party cookies are generally available. 

## Scenario 3: API debugging data joins

In this scenario, the developer is using the debugging channels of the Privacy Sandbox APIs to perform the development and testing of their integration with the Privacy Sandbox APIs. The Privacy Sandbox debugging channels are only available under certain circumstances where third-party cookies or AdID are available to provide a cross-site identity across sites or apps. 

As the debugging information is thus only available with such cross-site identifiers, the use of the API debugging channel information with those identifiers is consistent with the attestation.


## Scenario 4: model training

When training models for bidding and other related use cases, there may be inferences made from the various input data sources that include Privacy Sandbox API data that could potentially recognize the same user across sites. 

If the information from the model is not used to produce a persistent profile of a user that contains a cross-site identity outside of the model, then the inference performed within the model would be considered consistent with the attestation.

## Scenario 5: downstream receivers of topics

An enrolled caller of the Topics API is typically doing so in order to include the topic in an ad slot opportunity that is sent to other parties such as potential ad buyers or other exchanges, for example. The receiving party for this opportunity would receive the topic with the rest of the ad request information.

The downstream receivers of topics beyond the initial Topics API caller do not need to be enrolled, though many are likely to be enrolled for other API usage. A list of Privacy Sandbox enrollees will be provided programmatically as part of the programs transparency efforts, which would allow an interested caller of the Topics API to check if the recipient they are sending a topic to is enrolled, if the caller should want to.
