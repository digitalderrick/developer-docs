---
title: Partner Integration
keywords: Partner Integration Open Banking
summary: "Partner integrations require you to retrieve an OAuth access token for a specific merchant and use that unique token in all subsequent API calls."
sidebar: ob_sidebar
permalink: ob_partnerintegration.html
folder: prodNuapay
toc: true
---

## Partner Integration Modes

Nuapay offers two Partner models allowing you to connect to Nuapay as a:

1. Third Party Provider (TPP).
1. Technical Service Provider

|With Nuapay in **TPP Mode**|:

* You are licensed to offer Open Banking services under the Nuapay TPP license.
* All APIs are listed on the nuapay.com domain.
* ASPSP OAuth Callback is passed to nuapay.com
* A 'Powered by Nuapay' logo is displayed on the checkout window.


|With Nuapay as a **Technical Service Provider**|: 

 You will need: 
 
* An Open Banking TPP license.
* To provide us with your eIDAS certificate (QWAC and QSEAL).
* To provide us with a Software Statement Assertion (SSA) file to allow Nuapay to establish relationships with each of the ASPSPs.

Note that:

* APIs are white labelled as `partnerdomain.com` (you must point your DNS records to the Nuapay infrastructure).
* The ASPSP OAuth Callback is white labelled as `callback.partnerdomain.com` (you point your DNS records to the Nuapay infrastructure).
* A 'Powered by Partner' logo/notification can be displayed on the checkout window.

{% include note.html content="If you want to use Nuapay as a Technical Service Provider and meet the criteria above, please contact your account manager; your domain, callback URLs and any white-labelling will need to be configured as per your requirements." %}

## OAuth Token Generation 

Regardless of the integration mode chosen, in order to initiate API requests on behalf of your merchants, you will first need to retrieve OAuth tokens.
The process is illustrated below:
<img src="images/partner_integration.png">

The Nuapay Customer Support team will issue An API Key to you upon request.
The OAuth token retrieved from the `/tokens` endpoint allows you to then generate API requests <b>on behalf of</b> a specific merchant/organisation.

When generating an API request, provide the retrieved token as the authentication username in all your API requests. 
A password is not required, however the request must be made from an allowed IP address.

|API authentication header format:|`Authorization: Bearer <OAuth Token>`|


## API Details - GET /organisations

<div markdown="span" class="alert alert-info" role="alert"><i class="fab fa-github"></i> <b>Swagger Reference:</b>
[Gatekeeper](https://github.com/sentenial/gatekeeper-swagger){:target="_blank"}</div>

Use the `/organisations` endpoint to retrive the organisations linked to your partner entity:


<ul id="profileTabs" class="nav nav-tabs">
</ul>
  
{% include redoc.html %}
   
loadRedoc('#profileTabs', 'https://sentenial.github.io/gatekeeper-swagger/docs/redoc.html');
var timerRef = setInterval(function() { getDocs('operation/ListOrganisationsUsingGET','#profileTabs',timerRef); }, 500);
</script>
</div>
</div>


## Scopes and Time To Live (TTL)

Before generating an OAuth token for a specific organisation you will need to specify the scopes required and the token's TTL.

Two scopes are available for Open Banking APIs:

* AISP 
* PISP 

In additions, the following scopes are available:

* Admin
* Bank Admin

At least one scope must be included in your request.

The TTL by default is 10 seconds but long-lived tokens may also be created by configuring the `expiresIn` value in the request body.

{% include note.html content="Where you have created a long-lived token, it is possible to revoke it through a revoke API request. See [Token Management](ob_tokenmgmt.html) " %}


## API Details - POST /tokens

The `/organisations/{encodedOrganisationId}/tokens` endpoint takes an encoded organisation ID (returned from the `/organisations` endpoint) and returns an OAuth token. 

As outlined above, specify the `scopes` (required) and Time To Live - `expiresIn` (optional) in the request.


<ul id="profileTabs2" class="nav nav-tabs">
</ul>
  
{% include redoc.html %}
   
var timerRef2 = setInterval(function() { getDocs('operation/requestTokensForOrganisationUsingPOST','#profileTabs2',timerRef2); }, 500);
</script>
</div>
</div>


Once you have retrieved the token for the required merchant, use it in the HTTP Authorization Header of the [Bank](ob_getbank.html), [PISP](ob_createpayment.html) and/or [AISP](ob_createaccountaccess.html) API requests you generate.





{% include links.html %}