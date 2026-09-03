# Straddle Go API

Complete reference of every operation, grouped by resource. See [the README](./README.md) for usage and configuration.

## Contents

- [`Accounts`](#accounts)
  - [Get an account](#get-an-account)
  - [Update an account](#update-an-account)
  - [Create an account](#create-an-account)
  - [List accounts](#list-accounts)
  - [Onboard an account](#onboard-an-account)
  - [Simulate status transitions for a sandbox account](#simulate-status-transitions-for-a-sandbox-account)
- [`CapabilityRequests`](#capabilityrequests)
  - [Create capability requests](#create-capability-requests)
  - [List capability requests](#list-capability-requests)
- [`LinkedBankAccounts`](#linkedbankaccounts)
  - [Create a linked bank account](#create-a-linked-bank-account)
  - [List linked bank accounts](#list-linked-bank-accounts)
  - [Update a linked bank account](#update-a-linked-bank-account)
  - [Get a linked bank account](#get-a-linked-bank-account)
  - [Get an unmasked linked bank account](#get-an-unmasked-linked-bank-account)
  - [Cancel a linked bank account](#cancel-a-linked-bank-account)
- [`Organizations`](#organizations)
  - [Create an organization](#create-an-organization)
  - [List organizations](#list-organizations)
  - [Get an organization](#get-an-organization)
- [`Representatives`](#representatives)
  - [Create a representative](#create-a-representative)
  - [List representatives](#list-representatives)
  - [Update a representative](#update-a-representative)
  - [Get a representative](#get-a-representative)
  - [Get an unmasked representative](#get-an-unmasked-representative)
- [`Bridge`](#bridge)
  - [Create a paykey from bank account details](#create-a-paykey-from-bank-account-details)
  - [Create a paykey from a Plaid token](#create-a-paykey-from-a-plaid-token)
  - [Create a Bridge widget session token](#create-a-bridge-widget-session-token)
  - [Create a paykey from a Quiltt token](#create-a-paykey-from-a-quiltt-token)
- [`Customers`](#customers)
  - [Get a customer](#get-a-customer)
  - [Update a customer](#update-a-customer)
  - [Delete a customer](#delete-a-customer)
  - [List customers](#list-customers)
  - [Create a customer](#create-a-customer)
  - [Get an unmasked customer](#get-an-unmasked-customer)
  - [Refresh a customer review](#refresh-a-customer-review)
  - [`Customers Review`](#customers-review)
    - [Get a customer review](#get-a-customer-review)
    - [Set a customer verification decision](#set-a-customer-verification-decision)
- [`Paykeys`](#paykeys)
  - [Get a paykey](#get-a-paykey)
  - [Get an unmasked paykey](#get-an-unmasked-paykey)
  - [List paykeys](#list-paykeys)
  - [Reveal a paykey token](#reveal-a-paykey-token)
  - [Cancel a paykey](#cancel-a-paykey)
  - [Refresh a paykey review](#refresh-a-paykey-review)
  - [Refresh a paykey balance](#refresh-a-paykey-balance)
  - [Unblock a paykey](#unblock-a-paykey)
  - [`Paykeys Review`](#paykeys-review)
    - [Set a paykey verification decision](#set-a-paykey-verification-decision)
    - [Get a paykey review](#get-a-paykey-review)
- [`Charges`](#charges)
  - [Get a charge](#get-a-charge)
  - [Update a charge](#update-a-charge)
  - [Create a charge](#create-a-charge)
  - [Hold a charge](#hold-a-charge)
  - [Release a charge](#release-a-charge)
  - [Cancel a charge](#cancel-a-charge)
  - [Get an unmasked charge](#get-an-unmasked-charge)
  - [Resubmit a charge](#resubmit-a-charge)
  - [Refund a paid charge](#refund-a-paid-charge)
  - [Upload a proof-of-authorization document for a charge](#upload-a-proof-of-authorization-document-for-a-charge)
- [`FundingEvents`](#fundingevents)
  - [List funding events](#list-funding-events)
  - [Get a funding event](#get-a-funding-event)
  - [Simulate a funding event](#simulate-a-funding-event)
  - [List funding event payments](#list-funding-event-payments)
- [`Payments`](#payments)
  - [List payments](#list-payments)
- [`Payouts`](#payouts)
  - [Get a payout](#get-a-payout)
  - [Update a payout](#update-a-payout)
  - [Create a payout](#create-a-payout)
  - [Hold a payout](#hold-a-payout)
  - [Release a payout](#release-a-payout)
  - [Cancel a payout](#cancel-a-payout)
  - [Get an unmasked payout](#get-an-unmasked-payout)
  - [Resubmit a payout](#resubmit-a-payout)
  - [Upload a proof-of-authorization document for a payout](#upload-a-proof-of-authorization-document-for-a-payout)
- [`AccountSettings`](#accountsettings)
  - [Get account settings](#get-account-settings)

## Setup

```go
import (
	"context"
	"fmt"

	sdk "github.com/straddle-build/straddle-go"
)

client := sdk.NewClient()
```

## `Accounts`

Accounts represent businesses that use Straddle through a platform.

### Get an account

Returns the account with the specified ID.

| Direction | Type |
| --- | --- |
| Request | [`AccountGetParams`](./account.go) |
| Response | [`AccountResponse`](./account.go) |

```go
account, err := client.Accounts.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(account)
```

### Update an account

Updates an account's business profile, metadata, and external ID, then returns the account.

| Direction | Type |
| --- | --- |
| Request | [`AccountUpdateParams`](./account.go) |
| Response | [`AccountResponse`](./account.go) |

```go
account, err := client.Accounts.Update(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountUpdateParams{
	BusinessProfile: sdk.F[sdk.AccountBusinessProfileParam](sdk.AccountBusinessProfileParam{
		Name:    sdk.F[string](""),
		Website: sdk.F[string]("https://example.com"),
	}),
})
if err != nil {
	panic(err)
}

fmt.Println(account)
```

### Create an account

Creates a business account in the specified organization and returns the account.

| Direction | Type |
| --- | --- |
| Request | [`AccountNewParams`](./account.go) |
| Response | [`AccountResponse`](./account.go) |

```go
account, err := client.Accounts.New(context.Background(), sdk.AccountNewParams{
	BusinessProfile: sdk.F[sdk.AccountBusinessProfileParam](sdk.AccountBusinessProfileParam{
		Name:    sdk.F[string](""),
		Website: sdk.F[string]("https://example.com"),
	}),
	OrganizationID: sdk.F[string]("7c9e6679-7425-40de-944b-e07fc1f90ae7"),
})
if err != nil {
	panic(err)
}

fmt.Println(account)
```

### List accounts

Returns a paginated list of accounts for your platform. Filter the list by status, type, external ID, or text search.

| Direction | Type |
| --- | --- |
| Request | [`AccountListParams`](./account.go) |
| Response | [`AccountList`](./account.go) |

```go
account, err := client.Accounts.List(context.Background(), sdk.AccountListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortBy:     sdk.F[string]("id"),
	SortOrder:  sdk.F[sdk.AccountListParamsSortOrder](sdk.AccountListParamsSortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(account)
```

### Onboard an account

Starts onboarding and records the account's acceptance of Straddle's Terms of Service. The account must have at least one representative and one linked bank account. This operation also moves all associated representatives and linked bank accounts to `onboarding`.

| Direction | Type |
| --- | --- |
| Request | [`AccountOnboardParams`](./account.go) |
| Response | [`AccountResponse`](./account.go) |

```go
account, err := client.Accounts.Onboard(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountOnboardParams{
	TermsOfService: sdk.F[sdk.TermsOfServiceParam](sdk.TermsOfServiceParam{
		AcceptedDate: sdk.F[time.Time](time.Now()),
		AgreementURL: sdk.F[string](""),
	}),
})
if err != nil {
	panic(err)
}

fmt.Println(account)
```

### Simulate status transitions for a sandbox account

Simulates an account status transition to `onboarding` or `active` in the sandbox and returns the account.

| Direction | Type |
| --- | --- |
| Request | [`AccountSimulateOnboardingParams`](./account.go) |
| Response | [`AccountResponse`](./account.go) |

```go
account, err := client.Accounts.SimulateOnboarding(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountSimulateOnboardingParams{})
if err != nil {
	panic(err)
}

fmt.Println(account)
```

## `CapabilityRequests`

Capability requests change the payment, customer, and consent types available to an account.

### Create capability requests

Creates one or more capability requests for an account and returns the resulting requests.

| Direction | Type |
| --- | --- |
| Request | [`CapabilityRequestNewParams`](./capabilityrequest.go) |
| Response | [`CapabilityRequestList`](./capabilityrequest.go) |

```go
capabilityRequest, err := client.CapabilityRequests.New(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CapabilityRequestNewParams{})
if err != nil {
	panic(err)
}

fmt.Println(capabilityRequest)
```

### List capability requests

Returns a paginated list of capability requests for an account. Filter the list by capability type, category, or status.

| Direction | Type |
| --- | --- |
| Request | [`CapabilityRequestListParams`](./capabilityrequest.go) |
| Response | [`CapabilityRequestList`](./capabilityrequest.go) |

```go
capabilityRequest, err := client.CapabilityRequests.List(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CapabilityRequestListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortBy:     sdk.F[string]("id"),
	SortOrder:  sdk.F[sdk.CapabilityRequestListParamsSortOrder](sdk.CapabilityRequestListParamsSortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(capabilityRequest)
```

## `LinkedBankAccounts`

Linked bank accounts connect external bank accounts to an account or platform for charges, payouts, or billing.

### Create a linked bank account

Creates a linked bank account for an account or platform, assigns its payment purposes, and returns the linked bank account.

| Direction | Type |
| --- | --- |
| Request | [`LinkedBankAccountNewParams`](./linkedbankaccount.go) |
| Response | [`LinkedBankAccountResponse`](./linkedbankaccount.go) |

```go
linkedBankAccount, err := client.LinkedBankAccounts.New(context.Background(), sdk.LinkedBankAccountNewParams{
	BankAccount: sdk.F[sdk.LinkedBankAccountNewParamsBankAccount](sdk.LinkedBankAccountNewParamsBankAccount{
		AccountHolder: sdk.F[string](""),
		RoutingNumber: sdk.F[string]("xxxxxxxxx"),
		AccountNumber: sdk.F[string](""),
	}),
})
if err != nil {
	panic(err)
}

fmt.Println(linkedBankAccount)
```

### List linked bank accounts

Returns a paginated list of linked bank accounts. Filter the list by account, scope, purpose, or status.

| Direction | Type |
| --- | --- |
| Request | [`LinkedBankAccountListParams`](./linkedbankaccount.go) |
| Response | [`LinkedBankAccountList`](./linkedbankaccount.go) |

```go
linkedBankAccount, err := client.LinkedBankAccounts.List(context.Background(), sdk.LinkedBankAccountListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortBy:     sdk.F[string]("id"),
	SortOrder:  sdk.F[sdk.LinkedBankAccountListParamsSortOrder](sdk.LinkedBankAccountListParamsSortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(linkedBankAccount)
```

### Update a linked bank account

Updates bank account details and metadata, then returns the linked bank account. The linked bank account must have status `created`, or status `onboarding` with `status_detail.reason` set to `stuck`.

| Direction | Type |
| --- | --- |
| Request | [`LinkedBankAccountUpdateParams`](./linkedbankaccount.go) |
| Response | [`LinkedBankAccountResponse`](./linkedbankaccount.go) |

```go
linkedBankAccount, err := client.LinkedBankAccounts.Update(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.LinkedBankAccountUpdateParams{
	BankAccount: sdk.F[sdk.LinkedBankAccountUpdateParamsBankAccount](sdk.LinkedBankAccountUpdateParamsBankAccount{
		AccountHolder: sdk.F[string](""),
		RoutingNumber: sdk.F[string]("xxxxxxxxx"),
		AccountNumber: sdk.F[string](""),
	}),
})
if err != nil {
	panic(err)
}

fmt.Println(linkedBankAccount)
```

### Get a linked bank account

Returns the linked bank account with the specified ID. The response masks the account number.

| Direction | Type |
| --- | --- |
| Request | [`LinkedBankAccountGetParams`](./linkedbankaccount.go) |
| Response | [`LinkedBankAccountResponse`](./linkedbankaccount.go) |

```go
linkedBankAccount, err := client.LinkedBankAccounts.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.LinkedBankAccountGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(linkedBankAccount)
```

### Get an unmasked linked bank account

Returns the linked bank account with the specified ID without masking its account number. This endpoint is available only when Straddle enables data unmasking for the account.

| Direction | Type |
| --- | --- |
| Request | [`LinkedBankAccountListUnmaskedParams`](./linkedbankaccount.go) |
| Response | [`UnmaskedLinkedBankAccountResponse`](./linkedbankaccount.go) |

```go
linkedBankAccount, err := client.LinkedBankAccounts.ListUnmasked(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.LinkedBankAccountListUnmaskedParams{})
if err != nil {
	panic(err)
}

fmt.Println(linkedBankAccount)
```

### Cancel a linked bank account

Cancels a linked bank account and returns it with status `canceled`. The linked bank account must have status `created`.

| Direction | Type |
| --- | --- |
| Request | [`LinkedBankAccountCancelParams`](./linkedbankaccount.go) |
| Response | [`LinkedBankAccountResponse`](./linkedbankaccount.go) |

```go
linkedBankAccount, err := client.LinkedBankAccounts.Cancel(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.LinkedBankAccountCancelParams{})
if err != nil {
	panic(err)
}

fmt.Println(linkedBankAccount)
```

## `Organizations`

Organizations group related Straddle accounts.

### Create an organization

Creates an organization for your platform and returns it. Organizations group related accounts and users.

| Direction | Type |
| --- | --- |
| Request | [`OrganizationNewParams`](./organization.go) |
| Response | [`OrganizationResponse`](./organization.go) |

```go
organization, err := client.Organizations.New(context.Background(), sdk.OrganizationNewParams{
	Name: sdk.F[string](""),
})
if err != nil {
	panic(err)
}

fmt.Println(organization)
```

### List organizations

Returns a paginated list of organizations for your platform. Filter the list by name or external ID.

| Direction | Type |
| --- | --- |
| Request | [`OrganizationListParams`](./organization.go) |
| Response | [`OrganizationList`](./organization.go) |

```go
organization, err := client.Organizations.List(context.Background(), sdk.OrganizationListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortBy:     sdk.F[string]("id"),
	SortOrder:  sdk.F[sdk.OrganizationListParamsSortOrder](sdk.OrganizationListParamsSortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(organization)
```

### Get an organization

Returns the organization with the specified ID.

| Direction | Type |
| --- | --- |
| Request | [`OrganizationGetParams`](./organization.go) |
| Response | [`OrganizationResponse`](./organization.go) |

```go
organization, err := client.Organizations.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.OrganizationGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(organization)
```

## `Representatives`

Representatives are people associated with a business account for ownership, control, or authorization purposes.

### Create a representative

Creates a representative for an account and returns the representative. Relationship fields identify primary representatives, control persons, and owners.

| Direction | Type |
| --- | --- |
| Request | [`RepresentativeNewParams`](./representative.go) |
| Response | [`RepresentativeResponse`](./representative.go) |

```go
representative, err := client.Representatives.New(context.Background(), sdk.RepresentativeNewParams{
	AccountID:    sdk.F[string]("7c9e6679-7425-40de-944b-e07fc1f90ae7"),
	Dob:          sdk.F[time.Time](time.Now()),
	Email:        sdk.F[string]("ron.swanson@pawnee.com"),
	FirstName:    sdk.F[string](""),
	LastName:     sdk.F[string](""),
	MobileNumber: sdk.F[string]("+12128675309"),
	Relationship: sdk.F[sdk.RepresentativeRelationshipParam](sdk.RepresentativeRelationshipParam{
		Primary: sdk.F[bool](false),
		Control: sdk.F[bool](false),
		Owner:   sdk.F[bool](false),
	}),
	SsnLast4: sdk.F[string]("1234"),
})
if err != nil {
	panic(err)
}

fmt.Println(representative)
```

### List representatives

Returns a paginated list of representatives. Filter the list by account, organization, platform, or scope.

| Direction | Type |
| --- | --- |
| Request | [`RepresentativeListParams`](./representative.go) |
| Response | [`RepresentativeList`](./representative.go) |

```go
representative, err := client.Representatives.List(context.Background(), sdk.RepresentativeListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortBy:     sdk.F[string]("id"),
	SortOrder:  sdk.F[sdk.RepresentativeListParamsSortOrder](sdk.RepresentativeListParamsSortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(representative)
```

### Update a representative

Updates a representative's personal, contact, relationship, external ID, and metadata fields, then returns the representative.

| Direction | Type |
| --- | --- |
| Request | [`RepresentativeUpdateParams`](./representative.go) |
| Response | [`RepresentativeResponse`](./representative.go) |

```go
representative, err := client.Representatives.Update(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.RepresentativeUpdateParams{
	Dob:          sdk.F[time.Time](time.Now()),
	Email:        sdk.F[string]("ron.swanson@pawnee.com"),
	FirstName:    sdk.F[string]("Ron"),
	LastName:     sdk.F[string]("Swanson"),
	MobileNumber: sdk.F[string]("+12128675309"),
	Relationship: sdk.F[sdk.RepresentativeRelationshipParam](sdk.RepresentativeRelationshipParam{
		Primary: sdk.F[bool](false),
		Control: sdk.F[bool](false),
		Owner:   sdk.F[bool](false),
	}),
	SsnLast4: sdk.F[string]("1234"),
})
if err != nil {
	panic(err)
}

fmt.Println(representative)
```

### Get a representative

Returns the representative with the specified ID.

| Direction | Type |
| --- | --- |
| Request | [`RepresentativeGetParams`](./representative.go) |
| Response | [`RepresentativeResponse`](./representative.go) |

```go
representative, err := client.Representatives.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.RepresentativeGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(representative)
```

### Get an unmasked representative

Returns the representative with the specified ID without masking sensitive fields. This endpoint requires an administrator role.

| Direction | Type |
| --- | --- |
| Request | [`RepresentativeListUnmaskedParams`](./representative.go) |
| Response | [`UnmaskedRepresentativeResponse`](./representative.go) |

```go
representative, err := client.Representatives.ListUnmasked(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.RepresentativeListUnmaskedParams{})
if err != nil {
	panic(err)
}

fmt.Println(representative)
```

## `Bridge`

Bridge connects customer bank accounts and creates paykeys from supported provider tokens or bank account details.

### Create a paykey from bank account details

Creates a paykey from a routing number, account number, and account type.

| Direction | Type |
| --- | --- |
| Request | [`BridgeNewBankAccountPaykeyParams`](./bridge.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
bridge, err := client.Bridge.NewBankAccountPaykey(context.Background(), sdk.BridgeNewBankAccountPaykeyParams{
	AccountNumber: sdk.F[string](""),
	CustomerID:    sdk.F[string]("7c9e6679-7425-40de-944b-e07fc1f90ae7"),
	RoutingNumber: sdk.F[string]("xxxxxxxxx"),
})
if err != nil {
	panic(err)
}

fmt.Println(bridge)
```

### Create a paykey from a Plaid token

Creates a paykey from a Plaid processor token.

| Direction | Type |
| --- | --- |
| Request | [`BridgeNewPlaidPaykeyParams`](./bridge.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
bridge, err := client.Bridge.NewPlaidPaykey(context.Background(), sdk.BridgeNewPlaidPaykeyParams{
	CustomerID: sdk.F[string]("7c9e6679-7425-40de-944b-e07fc1f90ae7"),
	PlaidToken: sdk.F[string](""),
})
if err != nil {
	panic(err)
}

fmt.Println(bridge)
```

### Create a Bridge widget session token

Creates a session token for the Bridge widget.

| Direction | Type |
| --- | --- |
| Request | [`BridgeNewTokenParams`](./bridge.go) |
| Response | [`BridgeTokenResponse`](./bridge.go) |

```go
bridge, err := client.Bridge.NewToken(context.Background(), sdk.BridgeNewTokenParams{
	CustomerID: sdk.F[string]("7c9e6679-7425-40de-944b-e07fc1f90ae7"),
})
if err != nil {
	panic(err)
}

fmt.Println(bridge)
```

### Create a paykey from a Quiltt token

Creates a paykey from a Quiltt processor token.

| Direction | Type |
| --- | --- |
| Request | [`BridgeNewQuilttPaykeyParams`](./bridge.go) |
| Response | [`RevealedPaykeyResponse`](./bridge.go) |

```go
bridge, err := client.Bridge.NewQuilttPaykey(context.Background(), sdk.BridgeNewQuilttPaykeyParams{
	CustomerID:  sdk.F[string]("7c9e6679-7425-40de-944b-e07fc1f90ae7"),
	QuilttToken: sdk.F[string](""),
})
if err != nil {
	panic(err)
}

fmt.Println(bridge)
```

## `Customers`

Customers are individuals or businesses that send or receive payments through your integration.

### Get a customer

Returns a customer by `id`.

| Direction | Type |
| --- | --- |
| Request | [`CustomerGetParams`](./customer.go) |
| Response | [`CustomerResponse`](./customer.go) |

```go
customer, err := client.Customers.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### Update a customer

Updates an existing customer's profile, status, and metadata.

| Direction | Type |
| --- | --- |
| Request | [`CustomerUpdateParams`](./customer.go) |
| Response | [`CustomerResponse`](./customer.go) |

```go
customer, err := client.Customers.Update(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerUpdateParams{
	Device: sdk.F[sdk.CustomerDeviceParam](sdk.CustomerDeviceParam{
		IPAddress: sdk.F[string]("192.168.1.1"),
	}),
	Email: sdk.F[string]("user@example.com"),
	Name:  sdk.F[string](""),
	Phone: sdk.F[string](""),
})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### Delete a customer

Permanently deletes a customer record. The deletion cannot be undone. Use this endpoint only to meet regulatory or privacy requirements.

| Direction | Type |
| --- | --- |
| Request | [`CustomerDeleteParams`](./customer.go) |
| Response | [`CustomerResponse`](./customer.go) |

```go
customer, err := client.Customers.Delete(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerDeleteParams{})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### List customers

Returns a paginated list of customers for the account. Optional query parameters filter, search, and sort the results.

| Direction | Type |
| --- | --- |
| Request | [`CustomerListParams`](./customer.go) |
| Response | [`CustomerSummaryList`](./customer.go) |

```go
customer, err := client.Customers.List(context.Background(), sdk.CustomerListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortOrder:  sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### Create a customer

Creates a customer and starts identity, fraud, and risk assessments.

| Direction | Type |
| --- | --- |
| Request | [`CustomerNewParams`](./customer.go) |
| Response | [`CustomerResponse`](./customer.go) |

```go
customer, err := client.Customers.New(context.Background(), sdk.CustomerNewParams{
	Device: sdk.F[sdk.CustomerDeviceParam](sdk.CustomerDeviceParam{
		IPAddress: sdk.F[string]("192.168.1.1"),
	}),
	Email: sdk.F[string]("ron.swanson@pawnee.com"),
	Name:  sdk.F[string]("Ron Swanson"),
	Phone: sdk.F[string]("+12128675309"),
	Address: sdk.F[sdk.CustomerUpdateParamsAddress](sdk.CustomerUpdateParamsAddress{
		Address1: sdk.F[string]("123 Main St"),
		City:     sdk.F[string]("Anytown"),
		State:    sdk.F[string]("CA"),
		Zip:      sdk.F[string]("94105"),
	}),
	ExternalID: sdk.F[string]("customer_123"),
	Metadata:   sdk.F[map[string]string](map[string]string{}),
})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### Get an unmasked customer

Returns unmasked details for a customer, including personally identifiable information. Straddle must enable this endpoint for your account. Use this endpoint only when unmasked data is necessary.

| Direction | Type |
| --- | --- |
| Request | [`CustomerListUnmaskedParams`](./customer.go) |
| Response | [`UnmaskedCustomerResponse`](./customer.go) |

```go
customer, err := client.Customers.ListUnmasked(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerListUnmaskedParams{})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### Refresh a customer review

Starts a new identity review for a customer. The review runs asynchronously. Webhooks and the customer review endpoint return updated results.

| Direction | Type |
| --- | --- |
| Request | [`CustomerRefreshReviewParams`](./customer.go) |
| Response | [`CustomerResponse`](./customer.go) |

```go
customer, err := client.Customers.RefreshReview(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerRefreshReviewParams{})
if err != nil {
	panic(err)
}

fmt.Println(customer)
```

### `Customers Review`

Customers are individuals or businesses that send or receive payments through your integration.

#### Get a customer review

Returns the results of a customer's identity and fraud review. The response includes decisions, risk and correlation scores, reason codes, watchlist matches, and network alerts.

| Direction | Type |
| --- | --- |
| Request | [`CustomerReviewListParams`](./customerreview.go) |
| Response | [`CustomerReviewResponse`](./customerreview.go) |

```go
review, err := client.Customers.Review.List(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerReviewListParams{})
if err != nil {
	panic(err)
}

fmt.Println(review)
```

#### Set a customer verification decision

Updates the verification decision for a customer. The customer's current `status` must be `review`.

| Direction | Type |
| --- | --- |
| Request | [`CustomerReviewSetVerificationDecisionParams`](./customerreview.go) |
| Response | [`CustomerResponse`](./customer.go) |

```go
review, err := client.Customers.Review.SetVerificationDecision(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.CustomerReviewSetVerificationDecisionParams{})
if err != nil {
	panic(err)
}

fmt.Println(review)
```

## `Paykeys`

A paykey links a verified customer to a bank account without exposing bank account details. Use a paykey to create charges and payouts.

### Get a paykey

Returns a paykey by `id`, including the masked paykey value and bank account details.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyGetParams`](./paykey.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
paykey, err := client.Paykeys.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### Get an unmasked paykey

Returns a paykey by `id`, including the full paykey value and unmasked bank account details. Straddle must enable this endpoint for your account. Use this endpoint only when unmasked data is necessary.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyListUnmaskedParams`](./paykey.go) |
| Response | [`UnmaskedPaykeyResponse`](./paykey.go) |

```go
paykey, err := client.Paykeys.ListUnmasked(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyListUnmaskedParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### List paykeys

Returns a paginated list of paykeys for the account. Optional query parameters filter, search, and sort the results.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyListParams`](./paykey.go) |
| Response | [`PaykeySummaryList`](./paykey.go) |

```go
paykey, err := client.Paykeys.List(context.Background(), sdk.PaykeyListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortOrder:  sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### Reveal a paykey token

Returns a paykey by `id`, including the full paykey value and masked bank account details.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyRevealParams`](./paykey.go) |
| Response | [`RevealedPaykeyResponse`](./bridge.go) |

```go
paykey, err := client.Paykeys.Reveal(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyRevealParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### Cancel a paykey

Cancels a paykey so it cannot be used for new payments.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyCancelParams`](./paykey.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
paykey, err := client.Paykeys.Cancel(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyCancelParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### Refresh a paykey review

Starts a new verification review for a paykey. The review runs asynchronously. Webhooks and the paykey review endpoint return updated results.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyRefreshReviewParams`](./paykey.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
paykey, err := client.Paykeys.RefreshReview(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyRefreshReviewParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### Refresh a paykey balance

Starts an asynchronous balance refresh for a paykey. The response returns the paykey before the refresh finishes.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyRefreshBalanceParams`](./paykey.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
paykey, err := client.Paykeys.RefreshBalance(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyRefreshBalanceParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### Unblock a paykey

Unblocks a paykey that was blocked by an `R29` return. The paykey must not have been unblocked before.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyUnblockParams`](./paykey.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
paykey, err := client.Paykeys.Unblock(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyUnblockParams{})
if err != nil {
	panic(err)
}

fmt.Println(paykey)
```

### `Paykeys Review`

A paykey links a verified customer to a bank account without exposing bank account details. Use a paykey to create charges and payouts.

#### Set a paykey verification decision

Updates the verification decision for a paykey. The paykey's current `status` must be `review`.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyReviewSetVerificationDecisionParams`](./paykeyreview.go) |
| Response | [`PaykeyResponse`](./bridge.go) |

```go
review, err := client.Paykeys.Review.SetVerificationDecision(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyReviewSetVerificationDecisionParams{})
if err != nil {
	panic(err)
}

fmt.Println(review)
```

#### Get a paykey review

Returns a paykey verification review, including the decision, score breakdowns, and result codes.

| Direction | Type |
| --- | --- |
| Request | [`PaykeyReviewListParams`](./paykeyreview.go) |
| Response | [`PaykeyReviewResponse`](./paykeyreview.go) |

```go
review, err := client.Paykeys.Review.List(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PaykeyReviewListParams{})
if err != nil {
	panic(err)
}

fmt.Println(review)
```

## `Charges`

Charges debit a customer's bank account through a paykey.

### Get a charge

Returns a charge by its unique identifier.

| Direction | Type |
| --- | --- |
| Request | [`ChargeGetParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Update a charge

Updates the description, amount, `payment_date`, or metadata. The charge must have a status of `created` or `on_hold`.

| Direction | Type |
| --- | --- |
| Request | [`ChargeUpdateParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.Update(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeUpdateParams{
	Amount:      sdk.F[int64](10000),
	Description: sdk.F[string]("Monthly subscription fee"),
	PaymentDate: sdk.F[time.Time](time.Now()),
})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Create a charge

Creates a charge against a customer's paykey. Straddle submits the charge for processing on `payment_date` unless the charge is on hold.

| Direction | Type |
| --- | --- |
| Request | [`ChargeNewParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.New(context.Background(), sdk.ChargeNewParams{
	Amount:      sdk.F[int64](10000),
	Config:      sdk.F[sdk.ChargeConfigurationParam](sdk.ChargeConfigurationParam{}),
	Currency:    sdk.F[string](""),
	Description: sdk.F[string]("Monthly subscription fee"),
	Device: sdk.F[sdk.PaymentDeviceParam](sdk.PaymentDeviceParam{
		IPAddress: sdk.F[string]("192.168.1.1"),
	}),
	ExternalID:  sdk.F[string](""),
	Paykey:      sdk.F[string](""),
	PaymentDate: sdk.F[time.Time](time.Now()),
})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Hold a charge

Places a charge on hold to prevent submission for processing. The charge must have a status of `created` or `scheduled`.

| Direction | Type |
| --- | --- |
| Request | [`ChargeHoldParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.Hold(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeHoldParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Release a charge

Releases a charge from `on_hold` and returns it to `created` for submission on `payment_date`.

| Direction | Type |
| --- | --- |
| Request | [`ChargeReleaseParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.Release(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeReleaseParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Cancel a charge

Cancels a charge. The charge must have a status of `created`, `scheduled`, or `on_hold`.

| Direction | Type |
| --- | --- |
| Request | [`ChargeCancelParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.Cancel(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeCancelParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Get an unmasked charge

Return a charge with its sensitive fields unmasked.

| Direction | Type |
| --- | --- |
| Request | [`ChargeListUnmaskedParams`](./charge.go) |
| Response | [`UnmaskedChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.ListUnmasked(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeListUnmaskedParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Resubmit a charge

Creates a new charge from a failed, reversed, or cancelled charge. The request can override `description`, `external_id`, and `payment_date`. Other payment details come from the original charge.

| Direction | Type |
| --- | --- |
| Request | [`ChargeResubmitParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.Resubmit(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeResubmitParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Refund a paid charge

Creates a payout to return funds from a paid charge to the customer's bank account. The payout is linked to the charge through `related_payments`. A charge can be refunded once, either fully or partially.

| Direction | Type |
| --- | --- |
| Request | [`ChargeRefundParams`](./charge.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
charge, err := client.Charges.Refund(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeRefundParams{})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

### Upload a proof-of-authorization document for a charge

Uploads a proof-of-authorization document for a charge. A later upload adds another document and does not replace an existing one.

| Direction | Type |
| --- | --- |
| Request | [`ChargeUploadAuthorizationProofParams`](./charge.go) |
| Response | [`ChargeResponse`](./charge.go) |

```go
charge, err := client.Charges.UploadAuthorizationProof(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.ChargeUploadAuthorizationProofParams{
	File: sdk.F[io.Reader](strings.NewReader("")),
})
if err != nil {
	panic(err)
}

fmt.Println(charge)
```

## `FundingEvents`

Funding events group charge and payout activity into transfers between Straddle and your linked bank account.

### List funding events

Returns a paginated list of funding events that match the specified filters.

| Direction | Type |
| --- | --- |
| Request | [`FundingEventListParams`](./fundingevent.go) |
| Response | [`FundingEventSummaryList`](./fundingevent.go) |

```go
fundingEvent, err := client.FundingEvents.List(context.Background(), sdk.FundingEventListParams{
	PageNumber: sdk.F[int64](1),
	PageSize:   sdk.F[int64](100),
	SortOrder:  sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(fundingEvent)
```

### Get a funding event

Returns a funding event by its unique identifier, including its current status, status history, and linked bank account details when available.

| Direction | Type |
| --- | --- |
| Request | [`FundingEventGetParams`](./fundingevent.go) |
| Response | [`FundingEventResponse`](./fundingevent.go) |

```go
fundingEvent, err := client.FundingEvents.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.FundingEventGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(fundingEvent)
```

### Simulate a funding event

Creates a funding event for unfunded charge or payout activity in the sandbox and returns its ID. This endpoint is unavailable in production.

| Direction | Type |
| --- | --- |
| Request | [`FundingEventSimulateParams`](./fundingevent.go) |
| Response | [`FundingEventSimulation`](./fundingevent.go) |

```go
fundingEvent, err := client.FundingEvents.Simulate(context.Background(), sdk.FundingEventSimulateParams{})
if err != nil {
	panic(err)
}

fmt.Println(fundingEvent)
```

### List funding event payments

Returns a paginated list of payments included in the funding event.

| Direction | Type |
| --- | --- |
| Request | [`FundingEventListPaymentsParams`](./fundingevent.go) |
| Response | [`FundingEventPaymentList`](./fundingevent.go) |

```go
fundingEvent, err := client.FundingEvents.ListPayments(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.FundingEventListPaymentsParams{
	DefaultSortOrder: sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
	SortOrder:        sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(fundingEvent)
```

## `Payments`

Payments provide a combined view of charges and payouts.

### List payments

Returns a paged list of charges and payouts that match the filters.

| Direction | Type |
| --- | --- |
| Request | [`PaymentListParams`](./payment.go) |
| Response | [`PaymentSummaryList`](./payment.go) |

```go
payment, err := client.Payments.List(context.Background(), sdk.PaymentListParams{
	DefaultSort:      sdk.F[sdk.PaymentListParamsDefaultSort](sdk.PaymentListParamsDefaultSort("id")),
	DefaultSortOrder: sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
	PageNumber:       sdk.F[int64](1),
	PageSize:         sdk.F[int64](100),
	SortBy:           sdk.F[sdk.PaymentListParamsSortBy](sdk.PaymentListParamsSortBy("id")),
	SortOrder:        sdk.F[sdk.SortOrder](sdk.SortOrder("asc")),
})
if err != nil {
	panic(err)
}

fmt.Println(payment)
```

## `Payouts`

Payouts send money to a customer's bank account through a paykey.

### Get a payout

Returns a payout by its unique identifier.

| Direction | Type |
| --- | --- |
| Request | [`PayoutGetParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Update a payout

Updates the description, amount, `payment_date`, or metadata. The payout must have a status of `created` or `on_hold`.

| Direction | Type |
| --- | --- |
| Request | [`PayoutUpdateParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.Update(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutUpdateParams{
	Amount:      sdk.F[int64](10000),
	Description: sdk.F[string](""),
	PaymentDate: sdk.F[time.Time](time.Now()),
})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Create a payout

Creates a payout to a customer's bank account. Straddle submits the payout for processing on `payment_date` unless the payout is on hold.

| Direction | Type |
| --- | --- |
| Request | [`PayoutNewParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.New(context.Background(), sdk.PayoutNewParams{
	Amount:      sdk.F[int64](10000),
	Currency:    sdk.F[string](""),
	Description: sdk.F[string]("Vendor invoice payment"),
	Device: sdk.F[sdk.PaymentDeviceParam](sdk.PaymentDeviceParam{
		IPAddress: sdk.F[string]("192.168.1.1"),
	}),
	ExternalID:  sdk.F[string](""),
	Paykey:      sdk.F[string](""),
	PaymentDate: sdk.F[time.Time](time.Now()),
})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Hold a payout

Places a payout on hold to prevent submission for processing. The payout must have a status of `created` or `scheduled`.

| Direction | Type |
| --- | --- |
| Request | [`PayoutHoldParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.Hold(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutHoldParams{})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Release a payout

Releases a payout from `on_hold` and returns it to `created` for submission on `payment_date`.

| Direction | Type |
| --- | --- |
| Request | [`PayoutReleaseParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.Release(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutReleaseParams{})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Cancel a payout

Cancels a payout. The payout must have a status of `created`, `scheduled`, or `on_hold`.

| Direction | Type |
| --- | --- |
| Request | [`PayoutCancelParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.Cancel(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutCancelParams{})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Get an unmasked payout

Return a payout with its sensitive fields unmasked.

| Direction | Type |
| --- | --- |
| Request | [`PayoutListUnmaskedParams`](./payout.go) |
| Response | [`UnmaskedPayoutResponse`](./payout.go) |

```go
payout, err := client.Payouts.ListUnmasked(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutListUnmaskedParams{})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Resubmit a payout

Creates a new payout from a failed, reversed, or cancelled payout. The request can override `description`, `external_id`, and `payment_date`. Other payment details come from the original payout.

| Direction | Type |
| --- | --- |
| Request | [`PayoutResubmitParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.Resubmit(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutResubmitParams{})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

### Upload a proof-of-authorization document for a payout

Uploads a proof-of-authorization document for a payout. A later upload adds another document and does not replace an existing one.

| Direction | Type |
| --- | --- |
| Request | [`PayoutUploadAuthorizationProofParams`](./payout.go) |
| Response | [`PayoutResponse`](./charge.go) |

```go
payout, err := client.Payouts.UploadAuthorizationProof(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.PayoutUploadAuthorizationProofParams{
	File: sdk.F[io.Reader](strings.NewReader("")),
})
if err != nil {
	panic(err)
}

fmt.Println(payout)
```

## `AccountSettings`

Account settings define payment limits, capabilities, statement details, and policy controls for an account.

### Get account settings

Returns all effective settings for the account, including values inherited from its organization, platform, and system defaults.

| Direction | Type |
| --- | --- |
| Request | [`AccountSettingGetParams`](./accountsetting.go) |
| Response | [`AccountSettingsResponse`](./accountsetting.go) |

```go
accountSetting, err := client.AccountSettings.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountSettingGetParams{})
if err != nil {
	panic(err)
}

fmt.Println(accountSetting)
```
