%%%
title = "DataRight+: Banking Resource Set"
area = "Internet"
workgroup = "datarightplus"
submissionType = "independent"

[seriesInfo]
name = "Internet-Draft"
value = "draft-authors-datarightplus-resource-set-banking-latest"
stream = "independent"
status = "experimental"

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

This is the resource set profile outlining the banking sector related endpoints.

.# Notational Conventions

The keywords "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [@!RFC2119].

{mainmatter}

# Scope

The scope of this document is intended to be limited to the resource server endpoints related to banking, and their associated authorisation contexts.

# Terminology

This specification utilises the various terms outlined within [@!DATARIGHTPLUS-ROSETTA].

# Providers

Providers which providing banking services are expected to deliver a number of resource server end points.

## Authorisation Server

In addition to other provisions incorporated within the relevant ecosystem set, the Provider authorisation server **SHALL**:

1. Support the [@!RFC6749] `scope` parameter with possible values outlined within [Authorisation Scopes](#name-authorisation-scopes);

### Authorisation Scopes

The Provider authorisation server **SHALL** utilise the following Data Set Language when seeking Consumer authorisation from a User for specific `scope` values:

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

Alternative Data Cluster Language **SHALL** be used when pairs of `scope` value are used as follows:

| `scope` pairing                | Data Set Language               |
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

## Resource Server

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `bank:accounts.basic:read` scope value:

| Resource Server Endpoint                                         | `x-v`            |
|------------------------------------------------------------------|------------------|
| `GET /banking/accounts`                                          | `1` and `2`      |
| `GET /banking/accounts/balances`                                 | `1`              |
| `POST /banking/accounts/balances`                                | `1`              |
| `GET /banking/accounts/{accountId}/balance`                      | `1`              |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `bank:accounts.detail:read` scope value:

| Resource Server Endpoint                                         | `x-v`            |
|------------------------------------------------------------------|------------------|
| `GET /banking/accounts/{accountId}`                              | `1`, `2` and `3` |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `bank:regular_payments:read` scope value:

| Resource Server Endpoint                                         | `x-v`            |
|------------------------------------------------------------------|------------------|
| `GET /banking/accounts/direct-debits`                            | `1`              |
| `POST /banking/accounts/direct-debits`                           | `1`              |
| `GET /banking/accounts/{accountId}/direct-debits`                | `1`              |
| `GET /banking/accounts/{accountId}/payments/scheduled`           | `1`              |
| `POST /banking/payments/scheduled`                               | `1`              |
| `GET /banking/payments/scheduled`                                | `1`              |


The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `bank:payees:read` scope value:

| Resource Server Endpoint                                         | `x-v`            |
|------------------------------------------------------------------|------------------|
| `GET /banking/payees`                                            | `2`              |
| `GET /banking/payees/{payeeId}`                                  | `1` and `2`      |


The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `bank:transactions:read` scope value:

| Resource Server Endpoint                                         | `x-v`            |
|------------------------------------------------------------------|------------------|
| `GET /banking/accounts/{accountId}/transactions`                 | `1`              |
| `GET /banking/accounts/{accountId}/transactions/{transactionId}` | `1`              |




In addition, the Provider **SHALL** deliver the following unauthenticated and generally available endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY-ID1]:

| Resource Server Endpoint            | `x-v`       |
|-------------------------------------|-------------|
| `GET /banking/products`             | `2`         |
| `GET /banking/products/{productId}` | `3` and `4` |

# Initiators

Initiators **SHALL** describe the requested `scope` values using the same Data Set Language as Providers, as outlined in [Authorisation Scopes](#name-authorisation-scopes).

# Acknowledgement

The following people contributed to this document:

- Stuart Low (Biza.io) - Editor

We acknowledge the contribution to the [@!CDS] of the following individuals:

- James Bligh (Data Standards Body) - Lead Architect for the Consumer Data Right
- Mark Verstege (Data Standards Body) - Lead Architect, Banking & Information Security for the Consumer Data Right
- Ivan Hosgood (formerly Data Standards Body & ACCC) - Solutions Architect

{backmatter}

<reference anchor="DATARIGHTPLUS-REDOCLY-ID1" target="https://datarightplus.github.io/datarightplus-redocly/?v=ID1"> <front><title>DataRight+: Redocly (ID1)</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author>
<author initials="W." surname="Cai" fullname="Wei Cai"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-ROSETTA" target="https://datarightplus.github.io/datarightplus-rosetta/draft-authors-datarightplus-rosetta.html"> <front><title>DataRight+ Rosetta Stone</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="CDS" target="https://consumerdatastandardsaustralia.github.io/standards"><front><title>Consumer Data Standards (CDS)</title><author><organization>Data Standards Body (Treasury)</organization></author></front> </reference>

<reference anchor="RFC6749" target="https://datatracker.ietf.org/doc/html/rfc6749"> <front> <title>The OAuth 2.0 Authorization Framework</title><author fullname="D. Hardt"> <organization>Microsoft</organization> </author><date month="Oct" year="2012"/></front> </reference>




