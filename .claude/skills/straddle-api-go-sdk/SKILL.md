---
name: straddle-api-go-sdk
description: "Go SDK for Straddle API. Use when writing Go code that calls Straddle API with the github.com/straddle-build/straddle-go package: installing it, constructing and authenticating the client, and calling API operations."
---

# Straddle API Go SDK

Generated Go client for Straddle API, published as `github.com/straddle-build/straddle-go`. Use the generated client instead of hand-writing HTTP requests.

## Install

```sh
go get github.com/straddle-build/straddle-go
```

## Client setup and authentication

```go
import (
	"context"
	"fmt"

	sdk "github.com/straddle-build/straddle-go"
)

client := sdk.NewClient()
```

Provide credentials using the options below. Environment variables are read automatically when the target runtime supports them:

- `option.WithBearer` (env: `BEARER`) — Send the API key as a bearer token in the `Authorization` header.

## Calling operations

```go
package main

import (
	"context"
	"fmt"
	"os"

	sdk "github.com/straddle-build/straddle-go"
	"github.com/straddle-build/straddle-go/option"
)

func main() {
	client := sdk.NewClient(
		option.WithBearer(os.Getenv("BEARER")),
	)

	account, err := client.Accounts.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountGetParams{})
	if err != nil {
		panic(err)
	}

	fmt.Println(account)
}
```

Method names, parameter shapes, and response types are generated from the API description — do not guess them. Look up the exact call signature in [api.md](../../../api.md) before writing a call.

## Error handling

Non-success responses return generated API errors. Error objects expose status, headers, response body, and request metadata where the target runtime supports it.

```go
account, err := client.Accounts.Get(context.Background(), "7c9e6679-7425-40de-944b-e07fc1f90ae7", sdk.AccountGetParams{})
if err != nil {
	var apiErr *sdk.Error
	if errors.As(err, &apiErr) {
		fmt.Println(apiErr.StatusCode, apiErr.RawJSON())
	}
	panic(err)
}

// imports: "context", "errors", "fmt", sdk "github.com/straddle-build/straddle-go"
```

## Requirements

- Go 1.22 or newer

## Reference files

- [README.md](../../../README.md) — full feature tour: client options, request options, retries and timeouts, logging.
- [api.md](../../../api.md) — complete catalogue of every operation with request and response types.
