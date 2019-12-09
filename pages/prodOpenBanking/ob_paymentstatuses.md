---
title: Payment Statuses
keywords: Payment status open banking transitions
summary: "Open Banking payments transition through various statuses through their lifecycle. This section describes each of these statuses and the Webhooks that may be generated as statuses transition."
sidebar: ob_sidebar
permalink: ob_paymentstatuses.html
folder: prodOpenBanking
toc: false
---

## Overview

Open Banking Payments transition from an initial `PENDING` to any one of a number of possible statuses (note that some statuses are Nuapay-specific and are only relevant if you are using a Nuapay beneficiary account for your Open Banking payments). 
The various statuses possible are presented below in a table and as a state diagram:



<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#profile" data-toggle="tab">Table</a></li>
    <li><a href="#about" data-toggle="tab">Diagram</a></li>
   
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="profile">


<p>The following table gives an overview of each possible status and, if applicable, the Webhook event triggered:</p>


<table style="width: 730px;">
  <tbody>
    <tr>
      <td><strong>Status</strong></td>
      <td><strong>Description</strong></td>
      <td><strong>Nuapay-Specific?</strong></td>
      <td><strong>Final Status?</strong></td>
      <td><strong>Webhook Triggered?</strong></td>
      <td><strong>Webhook Link</strong></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">PENDING</code></td>
      <td>[Relevant for <a href="ob_pispimplementation.html#checkout-mode">Checkout</a> mode only] The payment has been created: the merchant has initiated the <code class="highlighter-rouge">POST/payments</code> call but the PSU has not yet selected the required bank.</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">CONSENT_API_REJECTED</code></td>
      <td>There was a technical issue at the ASPSP; no further processing will be possible.</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td> 
      <td><a href="ob_whpaymentrejected.html">PaymentRejected</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">PENDING_APPROVAL</code></td>
      <td>The approval for the payment is pending: the PSU has not yet approved the payment on the ASPSP.</td>
       <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">OAUTH_CALLBACK_COMPLETE</code></td>
      <td>The ASPSP has informed Nuapay that the payment has been authorised/declined by the PSU</td>
       <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">AUTHORISED</code></td>
      <td>The PSU has authorised the payment at the ASPSP.</td>
       <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">DECLINED</code></td>
      <td>The PSU has declined the payment at the ASPSP.</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><a href="ob_whpaymentrejected.html">PaymentRejected</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">FUNDS_CHECK_TIMEOUT</code></td>
      <td>The response to the funds check is later than the configured timeout.</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">FUNDS_CHECK_FAILED</code></td>
      <td>The funds check has indicated that there are insufficient funds to complete the payment.</td>
       <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">FUNDS_CHECK_PASSED</code></td>
      <td>Sufficient funds are available to complete the payment.</td>
       <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">SETTLEMENT_PENDING</code></td>
      <td>The payment has been authorised but has not yet transitioned to <code class="highlighter-rouge">SETTLEMENT_IN_PROGRESS</code> status.</td>
       <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">SETTLEMENT_IN_PROGRESS</code></td>
      <td>The settlement is being processed by the ASPSP. The payment will generally move to <code class="highlighter-rouge">SETTLEMEMT_COMPLETE</code> after this status. For high value goods we recommend waiting for a final status before processing the order.</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td><a href="ob_whpaymentinprogress.html">PaymentInProgress</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">SETTLEMENT_COMPLETE</code></td>
      <td>The ASPSP has debited the payment from the PSU’s account. This may be treated as a Final status if the merchant does not have a Nuapay account; in this case, the merchant should confirm the crediting of its account before shipping goods.</td>
       <td>No</td>
      <td>Conditional</td>
      <td>Yes</td>
      <td><a href="ob_whpaymentcomplete.html">PaymentCompleted</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">SETTLEMENT_REJECTED</code></td>
      <td>The settlement has not been completed and won’t be in future. This is a Final status; the merchant should not ship goods.</td>
       <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><a href="ob_whpaymentrejected.html">PaymentRejected</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">PAYMENT_RECEIVED</code></td>
      <td>The settlement amount has been credited to the merchant’s Nuapay account.</td>
       <td>No</td>
       <td>No</td>
      <td>Yes</td>    
      <td><a href="ob_whreceived.html">PaymentReceived</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">TIMEOUT</code></td>
      <td>[Relevant for <a href="ob_pispimplementation.html#checkout-mode">Checkout</a> mode only] The payment timed out - this may happen where the user fails to progress to the ASPSP from the TPP before the merchant-configured time out period e.g. if the user has not selected an option on the Bank Select screen within the required time period.</td>
       <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><a href="ob_whpaymenttimeout.html">PaymentTimeout</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">CONSENT_TIMEOUT</code></td>
      <td>The PSU provided his/her consent but the merchant-defined timeout period has elapsed. No payment will be attempted.</td>
       <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><a href="ob_whpaymenttimeout.html">PaymentTimeout</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">PAYMENT_REVERSED</code></td>
      <td>A payment that was previously received is credited back to the PSU. A payment is typically reversed where the merchant deems it appropriate to refund a customer (following a complaint, for example).</td>
       <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td><a href="ob_whrreversed.html">PaymentReversed</a></td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">PAYMENT_REVERSAL_REJECTED</code></td>
      <td>A payment credited to the PSU has been rejected by the beneficiary bank for a specific reason (e.g. the account is closed).</td>
       <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">UNEXPECTED_ERROR</code></td>
      <td>A processing error has occurred. This may be due to connectivity issues between ASPSP and the TPP for example.</td>
       <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">UNKNOWN</code></td>
      <td>After transitioning to <code class="highlighter-rouge">AUTHORISED</code>, the TPP cannot determine the status of the payment at the ASPSP (e.g. after receiving a 500 HTTP response) .</td>
       <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>N/A</td>
    </tr>
  </tbody>
</table>



</div>

<div role="tabpanel" class="tab-pane" id="about">
<p>The diagram below illustrates all possible statuses, with the happy path highlighted in green.</p>




{% include note.html content="Click the Extend button on the top menu to enlarge or click the image to open it in a new browser window/tab, which will allow you to zoom in for a clearer view." %}


 {% include image.html file="ob_paymentStatus.png" url="images/ob_paymentStatus.png" target = "_new" alt="Payment Statuses" caption="Payment Statuses" %}

</div>
</div>

 




{% include links.html %}