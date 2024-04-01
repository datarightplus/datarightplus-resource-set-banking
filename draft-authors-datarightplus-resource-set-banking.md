%%%
title = "DataRight+: Banking Resource Set"
area = "Internet"
workgroup = "datarightplus"
submissionType = "independent"

[seriesInfo]
name = "Internet-Draft"
value = "draft-authors-datarightplus-resource-set-banking-latest"
stream = "independent"
status = "informational"

date = 2024-03-28T00:00:00Z

[[author]]
initials="S."
surname="Low"
fullname="Stuart Low"
organization="Biza.io"
[author.address]
email = "stuart@biza.io"

%%%

.# Abstract

This is the resource set profile outlining the banking endpoints utilised across multiple industries.

.# Notational Conventions

The keywords "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [@!RFC2119].

{mainmatter}

# Scope

The scope of this document is intended to be limited to the shared resource server endpoints, and their associated authorisation contexts.

# Terminology

This specification utilises the various terms outlined within [@!DATARIGHTPLUS-ROSETTA].

# Authorisation Scopes

Technical authorisation scopes are defined between Provider and Initiator and permit access to resource server endpoints. In addition, this specification also outlines the title and a simple description of the language to use to describe the data set referred to as Data Set Language.

Providers and Initiators **MUST** utilise the prescribed authorisation scopes and Data Set Language when seeking Consumer authorisation:

| `scope` value                | Data Set Language                                                                                                         |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| `bank:accounts.basic:read`   | **Account name, type and balance**                                                                                        |
|                              | Name of account;                                                                                                          |
|                              | Type of account;                                                                                                          |
|                              | Account balance;                                                                                                          |
| `bank:accounts.detail:read`  | **Account numbers and features**                                                                                          |
|                              | Account number;                                                                                                           |
|                              | Interest rates;                                                                                                           |
|                              | Fees;                                                                                                                     |
|                              | Discounts;                                                                                                                |
|                              | Account terms;                                                                                                            |
|                              | Account mail address;                                                                                                     |
| `bank:transactions:read`     | **Transaction details**                                                                                                   |
|                              | Incoming and outgoing transactions;                                                                                       |
|                              | Amounts;                                                                                                                  |
|                              | Dates;                                                                                                                    |
|                              | Descriptions of transactions;                                                                                             |
|                              | Who you have sent money to and received money from; (e.g. their name)                                                     |
| `bank:regular_payments:read` | **Direct debits and scheduled payments**                                                                                  |
|                              | Direct debits;                                                                                                            |
|                              | Scheduled payments;                                                                                                       |
| `bank:payees:read`           | **Saved payees**                                                                                                          |
|                              | Names and details of accounts you have saved; (e.g. their BSB and Account Number, BPAY CRN and Biller code, or NPP PayID) |

## Overlapping Scope Optimisation

In certain situations multiple technical scopes overlap which can lead to confusion by the User granting permission for the Consumer (which may be themselves or an Entity they represent).

Data Cluster Language presentation **MUST** be collapsed for the following pairs of `scope` values:

| `scope` pairing                  | Data Set Language               |
|--------------------------------|---------------------------------|
| `bank:accounts.basic:read` and | **Account balance and details** |
| `bank:accounts.detail:read`    | Name of account;                |
|                                | Type of account;                |
|                                | Account balance;                |
|                                | Account number;                 |
|                                | Interest rates;                 |
|                                | Fees;                           |
|                                | Discounts;                      |
|                                | Account terms;                  |
|                                | Account mail address;           |

# Providers

Providers are **REQUIRED** to deliver a number of authorisation and resource server capabilities.

## Authorisation Server

In addition to other provisions incorporated within the relevent ecosystem set, the Provider authorisation server **MUST**:

1. Support the `scope` parameters outlined within [Authorisation Scopes];

## Resource Server

The Provider **MUST** deliver the following authorisation enabled endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY]:

| Resource Server Endpoint                                         | Authorisation Scope          | OpenAPI Operation ID                    | `x-v`            |
|------------------------------------------------------------------|------------------------------|-----------------------------------------|------------------|
| `GET /banking/accounts`                                          | `bank:accounts.basic:read`   | `listBankingAccounts`                   | `1` and `2`      |
| `GET /banking/accounts/{accountId}`                              | `bank:accounts.detail:read`  | `getBankingAccountDetail`               | `1`, `2` and `3` |
| `GET /banking/accounts/balances`                                 | `bank:accounts.basic:read`   | `listBankingBalancesBulk`               | `1`              |
| `POST /banking/accounts/balances`                                | `bank:accounts.basic:read`   | `listBalancesSpecificBankingAccounts`   | `1`              |
| `GET /banking/accounts/{accountId}/balance`                      | `bank:accounts.basic:read`   | `getBankingBalance`                     | `1`              |
| `GET /banking/accounts/direct-debits`                            | `bank:regular_payments:read` | `listDirectDebitsBulk`                  | `1`              |
| `POST /banking/accounts/direct-debits`                           | `bank:regular_payments:read` | `listDirectDebitsSpecificAccounts`      | `1`              |
| `GET /banking/accounts/{accountId}/direct-debits`                | `bank:regular_payments:read` | `listDirectDebits`                      | `1`              |
| `GET /banking/payees`                                            | `bank:payees:read`           | `listPayees`                            | `2`              |
| `GET /banking/payees/{payeeId}`                                  | `bank:payees:read`           | `getPayeeDetail`                        | `1` and `2`      |
| `GET /banking/accounts/{accountId}/payments/scheduled`           | `bank:regular_payments:read` | `listScheduledPayments`                 | `1`              |
| `POST /banking/payments/scheduled`                               | `bank:regular_payments:read` | `listScheduledPaymentsSpecificAccounts` | `1`              |
| `GET /banking/payments/scheduled`                                | `bank:regular_payments:read` | `listScheduledPaymentsBulk`             | `1`              |
| `GET /banking/accounts/{accountId}/transactions`                 | `bank:transactions:read`     | `getTransactions`                       | `1`              |
| `GET /banking/accounts/{accountId}/transactions/{transactionId}` | `bank:transactions:read`     | `getTransactionDetail`                  | `1`              |

In addition the Provider **MUST** deliver the following unauthenticated and generally available endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY]:

| HTTP Endpoint            | OpenAPI Operation ID | `x-v` |
|--------------------------|----------------------|-------|
| `GET /banking/products` | `listBankingProducts`         | `2`   |
| `GET /banking/products/{productId}`  | `getBankingProductDetail`          | `3` and `4`   |

# Initiators

This specification contains no additional provisions for Initiators.

# Acknowledgement

The following people contributed to this document:

- Stuart Low (Biza.io) - Editor

We acknowledge the contribution to the [@!CDS] of the following individuals:
- James Bligh (Data Standards Body) - Lead Architect for the Consumer Data Right
- Mark Verstege (Data Standards Body) - Lead Architect, Banking & Information Security for the Consumer Data Right
- Ivan Hosgood (formerly Data Standards Body & ACCC) - Solutions Architect

{backmatter}

<reference anchor="DATARIGHTPLUS-REDOCLY" target="https://datarightplus.github.io/datarightplus-redocly/"> <front><title>DataRight+: Redocly</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author>
<author initials="W." surname="Cai" fullname="Wei Cai"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-ROSETTA" target="https://datarightplus.github.io/datarightplus-specs/main/datarightplus-rosetta.html"> <front><title>DataRight+ Rosetta Stone</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="CDS" target="https://consumerdatastandardsaustralia.github.io/standards"><front><title>Consumer Data Standards (CDS)</title><author><organization>Data Standards Body (Treasury)</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-INFOSEC-SHARING-V1" target="https://datarightplus.github.io/datarightplus-specs/main/datarightplus-infosec-sharing-v1.html"> <front><title>CDR: Sharing Arrangement V1</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="OIDC-Core" target="http://openid.net/specs/openid-connect-core-1_0.html"> <front> <title>OpenID Connect Core 1.0 incorporating errata set 1</title> <author initials="N." surname="Sakimura" fullname="Nat Sakimura"></author></front></reference>

<reference anchor="TDIF" target="https://www.digitalidentity.gov.au"><front><title>Trusted Digital Identity Framework (
TDIF)</title><author><organization>Commonwealth of
Australia (Digital Transformation Agency)</organization></author></front> </reference>






