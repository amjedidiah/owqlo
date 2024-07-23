# OWQLO Stripe Connect Platform Resolution

A planning document to provide insights for Owqlo(a Stripe client):

- as to why they constantly get debits on their Stripe Connect Express platform bank account
- into certain debit transactions on their platform accounts that they do not understand

## Table of Content

- [The Problems](#the-problems)
- [Suggested Fix](#suggested-fix)
- [More Suggestions](#more-suggestions)
- [Resources](#resources)
- [Further Reading](#further-reading)

## The Problems

### Negative Balance

The reason why Stripe constantly debits the Stripe Connect Express platform bank account is because the platform is responsible for associated Connected account(s) with **_negative balance_**. More details can be found in the following links:

- [Fix the negative balance on your account](https://support.stripe.com/questions/fix-the-negative-balance-on-your-account)
- [Risk management with Connect](https://docs.stripe.com/connect/risk-management#connect-merchant-risk-options)

### Unclear Debit Transactions

For unclear debits transactions, consider these documents:

- [Balance Transaction Types](https://docs.stripe.com/reports/balance-transaction-types)
- [Understanding IC+ Fees](https://support.stripe.com/questions/understanding-ic-fees)

- [Reporting Categories and types](https://docs.stripe.com/reports/reporting-categories)
- [Other fees](https://stripe.com/resources/more/merchant-fees-101-what-they-are-how-they-work-and-how-to-minimize-your-costs#other-fees)

## Suggested Fix

- Enable [`debit_negative_balances`](https://docs.stripe.com/api/accounts/object#account_object-settings-payouts-debit_negative_balances) on connected accounts

- [Rejecting](https://docs.stripe.com/api#reject_account) the connected account’s whose negative balance is cleared through a [`connect_collection_transfer`](https://support.stripe.com/questions/reserves-for-connect-platforms-and-connected-accounts) to prevent losses

- Set Stripe to manage risk and cover any negative balances on your connected accounts
  > This is only possible if you use direct charge as shown in this [video](https://drive.google.com/file/d/1nGoUvIhZ0EykHz824ehP0xeD00MpaOBe/view?usp=sharing)

## More Suggestions

- In your integration, make sure you are vetting onboarded connected accounts for:
  - [fraud](https://docs.stripe.com/connect/risk-management/best-practices#fraud)
    - Consider rejecting fraudulent connected accounts to prevent losses
  - a valid verified bank account, not a debit card
    [![Valid connect bank account](https://i.ibb.co/Smk75Mq/Screenshot-2024-07-22-at-08-07-58.png)](https://docs.stripe.com/connect/account-balances#validating-connected-banks)
    [![WHy valid connect bank account is required](https://i.ibb.co/FYxjNvZ/Screenshot-2024-07-22-at-08-25-24.png)](https://docs.stripe.com/connect/account-balances#validating-connected-banks:~:text=automatically%20do%20so.-,Caution,-Stripe%20can%E2%80%99t%20correct)

- Consider opting in to separate charges and transfers or manual payouts
    > Note that using separate charges and transfers or destination charges introduces **_transaction risks_** for the platform as opposed to direct charges where the transaction risk is associated with the connected account.
    > Transaction risk is risk that a customer might charge back a transaction, such as disputes or fraud identified from card testing.

- Consider enabling Radar with Connect for chargeback protection: protects your platform and connected accounts from disputes by covering both disputed amounts and any dispute fees.

- Set up webhooks to listen for connected account activity and require connected account to provide any new needed documents / information required by Stripe OR Constantly review the [Accounts To Review tab](https://dashboard.stripe.com/connect/accounts_to_review) in your dashboard for connect accounts with onboarding restrictions or risks
    > Connected accounts can provide these new information using remediation links

- Consider preventing [account takeovers](https://docs.stripe.com/connect/risk-management/best-practices#prevent-account-take-overs)

- Use your Connect dashboard to [view support cases](https://docs.stripe.com/connect/dashboard/managing-individual-accounts#view-and-unblock-support-cases) that your connected accounts have opened with Stripe and help unblock them by providing additional context.

- Mitigate credit risks by:
  - [Monitoring accounts](https://docs.stripe.com/connect/risk-management/best-practices#account-monitoring)
  - Consider delaying or holding payouts until goods or services are delivered for ALL accounts, to prevent credit risk
  - Reverse transfers on connected accounts that don't have enough balance to cover refunds or immediately issue the refund knowing that future payments to the connected account can cover the refund.
  - Proactively cancel and refund charges that are likely to be disputed
  - Permit your team to handle refunds by adding them to your platform account with [assigned roles](https://docs.stripe.com/get-started/account/teams/roles).
  - Pause collection of recurring payments for subscriptions that are at high risk for chargebacks
  - Protect your platform from negative balances by adding funds to your platform balance.
  - Consider activating Stripe Sigma, to generate a report of each account’s [negative balance over time](https://dashboard.stripe.com/sigma/queries/templates/Account%20balances%20for%20each%20connected%20account).
  - Pause payouts for suspicious connected accounts
  - Pause payment collection for suspicious connected accounts
  - Reject suspicious connected accounts

## Resources

- [Understanding Connect account balances](https://docs.stripe.com/connect/account-balances)
- [Connect integration guide](https://docs.stripe.com/connect/design-an-integration)
- [Using Connect with Express connected accounts](https://docs.stripe.com/connect/express-accounts)
- [Best practices for risk management](https://docs.stripe.com/connect/risk-management/best-practices)
- [Embedded Stripe Managed Risk](https://docs.stripe.com/connect/embedded-risk)
- [Fix the negative balance on your account](https://support.stripe.com/questions/fix-the-negative-balance-on-your-account)
- [Merchant fees 101: What they are, how they work, and how to minimize your costs](https://stripe.com/resources/more/merchant-fees-101-what-they-are-how-they-work-and-how-to-minimize-your-costs#other-fees)

## Further Reading

- [Introduction to risk management for software platforms](https://stripe.com/guides/introduction-to-risk-management)
- [Risk management with Connect](https://docs.stripe.com/connect/risk-management)
- [Stripe Managed Risk](https://docs.stripe.com/connect/risk-management/managed-risk)
- [Create a charge](https://docs.stripe.com/connect/charges)
- [Using manual payouts](https://docs.stripe.com/connect/manual-payouts)
- [Remediation link process walkthrough](https://docs.stripe.com/connect/dashboard/remediation-links)
- [Use Radar with Connect](https://docs.stripe.com/connect/radar)
- [Platform controls in the Stripe Dashboard](https://docs.stripe.com/connect/platform-controls-for-stripe-dashboard-accounts)
