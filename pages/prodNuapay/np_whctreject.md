---
title: Credit Transfer Reject Event
keywords: Credit Transfer Reject Event Webhook 
summary: "Credit Transfer Reject Webhook event"
sidebar: np_sidebar
permalink: np_ctreject.html
folder: prodNuapay
toc: false
---
 
{% include webhook.html content="A PAIN.002 Import with a Credit Transfer rejection." %}


## Webhook Message Details

This Webhook has the following event types:

|**Webhook Event Type**| **Description**|
|CreditTransferReject|Triggered where a PAIN.002 import updates a Credit Transfer transaction to status = REJECTED|
|CreditTransferCancel|Triggered where a PAIN.002 import updates a Credit Transfer transaction to status = CANCELLED. A Credit Transfer is updated to CANCELLED where the SEPA Reason Code is one of the following: CUST, CUTA, DUPL, UPAY|



## Webhook Event Message Details

<p>
	The following table describes the details of the Webhook notification:</p>

<table cellspacing="0">
	
	<tbody>
		<tr>
			<th>Parent</th>
			<th>Parameter</th>
			<th>Type</th>
			<th>Mandatory/Optional</th>
			<th>Description</th>
		</tr>
		<tr>
			<td >root</td>
			<td>eventTimestamp</td>
			<td>number</td>
			<td>Mandatory</td>
			<td> The Unix epoch timestamp </td>
		</tr>
		<tr>
			<td>root</td>
			<td>eventType</td>
			<td>string</td>
			<td>Mandatory</td>
            <td>Either <b>CreditTransferReject </b> <br/> or <br/> <b>CreditTransferCancel</b></td>
		</tr>
		<tr>
			<td>root</td>
			<td>resourceTechnicalId</td>
			<td>number</td>
			<td>Mandatory</td>
            <td>Unique identifier</td>
		</tr>
		<tr>
			<td>root</td>
			<td>resourceReference</td>
			<td>string</td>
			<td>optional</td>
			<td>This can be the business reference of the resource, useful when filtering events via the Webhooks area of the Developer Dashboard.</td>
		</tr>
		<tr>
			<td>root</td>
			<td>resourceReferenceType</td>
			<td>string</td>
			<td> optional</td>
			<td>This can be a business reference of the resource, useful when filtering events via the Webhooks area of the Developer Dashboard.</td>
		</tr>
		<tr>
			<td>root</td>
			<td>resourceDetails</td>
			<td> object</td>
			<td>Mandatory</td>
			<td>Resource specific data grouping object </td>
		</tr>
		<tr>
			<td>resourceDetails</td>
			<td>uri</td>
			<td>string</td>
			<td>Mandatory</td>
			<td> This is the URI of the resource. Use the URI to retrieve more details - see <a href ="np_viewtransaction.html">Retrieve Credit Transfer Transaction</a>.</td>
		</tr>
		<tr>
			<td>resourceDetails</td>
			<td>type</td>
			<td>string</td>
			<td>Mandatory</td>
			<td>This is the type of the resource to which the URI is related. In this case it is a Credit Transfer resource.</td>
		</tr>
		<tr>
			<td>resourceDetails</td>
			<td>reasonCode</td>
			<td>string</td>
			<td>optional</td>
			<td>The SEPA Reason code</td>
		</tr>
		<tr>
			<td>root</td>
			<td>organizationId</td>
			<td>number</td>
			<td>Mandatory</td>
			<td> The unique merchant identifier </td>
		</tr>
	</tbody>
</table>

## JSON Sample

The following is an example of an electonic mandate signing event JSON:

```js
POST http://example.com/webhooks
Content-Type: application/json;charset=UTF-8
X-Signature: 521ab01d030dee864fb44cc65a3be52ae591f46cde8d14d3e72fbc3790e4a304
Content-Length: 261
X-Request-Id: dc645679-71a5-498d-bb29-ec027948c7c1
	{
		"eventTimestamp": 1500651159000,
		"eventType": "CreditTransferCancel",
		"resourceReference": "DemoE2EID",
		"resourceReferenceType": "EndToEndId",
		"resourceUri": "/accounts/qj29pkgnbx/transactions/yabcdwgrg23",
		"resourceType": "Transaction",
		"reasonCode": "CUST"
	}
````


{% include links.html %}