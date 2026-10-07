# Straddle Go SDK

Use Straddle's Pay by Bank and Embed APIs from Go. The SDK provides typed requests and responses, authentication, retries, and configurable HTTP transport.

## Install

Use Go 1.22 or later. Add the module to your project:

```sh
go get github.com/straddle-build/straddle-go
```

The module path is `github.com/straddle-build/straddle-go`; its package name is `straddle`. The examples use the alias `sdk`. If you use the earlier `straddleio` module, follow the [migration guide](#migrate-from-the-straddleio-module).

## Make your first request

Create a sandbox API key in the [Straddle Dashboard](https://dashboard.straddle.com), then set it in your environment. See [API authentication](https://docs.straddle.com/api-reference/authentication) for the setup steps.

```sh
export STRADDLE_API_KEY="YOUR_SANDBOX_API_KEY"
```

Save the following example as `main.go` in your Go project. It requests the first page of customers from the sandbox:

```go
package main

import (
	"context"
	"fmt"
	"log"
	"os"
	"time"

	sdk "github.com/straddle-build/straddle-go"
	"github.com/straddle-build/straddle-go/option"
)

func main() {
	apiKey := os.Getenv("STRADDLE_API_KEY")
	if apiKey == "" {
		log.Fatal("Set STRADDLE_API_KEY to your sandbox API key.")
	}

	client := sdk.NewClient(
		option.WithBearer(apiKey),
		option.WithBaseURL("https://sandbox.straddle.com"),
	)
	ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
	defer cancel()

	page, err := client.Customers.List(ctx, sdk.CustomerListParams{
		PageNumber: sdk.Int(1),
		PageSize:   sdk.Int(10),
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Customers on this page: %d\n", len(page.Data))
}
```

For a SaaS platform key, add `StraddleAccountID: sdk.String("YOUR_EMBEDDED_ACCOUNT_ID")` to `CustomerListParams` before running the example. This selects the embedded account whose customers you want to read. Direct accounts and marketplaces list customers without that header. See [platform account scoping](https://docs.straddle.com/guides/embed/api-headers).

Run the example:

```sh
go run .
```

A successful request prints the number of customers on the page. `Customers on this page: 0` is valid for an empty account. Customer records are in `page.Data`; pagination and request metadata are in `page.Meta`.

The remaining snippets use the `client` and `ctx` from this example.

## Configure authentication and environments

The example passes `STRADDLE_API_KEY` explicitly with `option.WithBearer`. If you omit this option, the client reads `BEARER`.

Set `option.WithBaseURL` explicitly to select an environment. If you omit it, the client reads `STRADDLE_BASE_URL`, then defaults to `https://sandbox.straddle.com`. Production uses `https://production.straddle.com` and a production API key. See [environments](https://docs.straddle.com/api-reference/environments).

## Read additional pages

List methods return one response page. Choose the next `PageNumber` using `page.Meta.TotalPages`, and keep your filters and account scope the same between requests:

```go
nextPage, err := client.Customers.List(ctx, sdk.CustomerListParams{
	PageNumber: sdk.Int(2),
	PageSize:   sdk.Int(10),
})
if err != nil {
	log.Fatal(err)
}
fmt.Println(len(nextPage.Data))
```

Use `sdk.Int`, `sdk.String`, `sdk.Bool`, `sdk.Float`, or the generic `sdk.F(value)` to set optional fields. These helpers distinguish a supplied zero value from an omitted field. See the [method reference](./api.md) for filters and response types.

## Handle errors

Methods return an error as their second result. Use `errors.As` to inspect an API error's status and response body:

```go
// Add "errors" to your imports.
page, err := client.Customers.List(ctx, sdk.CustomerListParams{PageSize: sdk.Int(10)})
if err != nil {
	var apiErr *sdk.Error
	if errors.As(err, &apiErr) {
		fmt.Println(apiErr.StatusCode, apiErr.JSON.RawJSON())
	}
	log.Fatal(err)
}
fmt.Println(len(page.Data))
```

For a `401`, check that the key matches the selected environment. For a `403`, check the key's permissions and account scope. Transport errors and context cancellation also return through `err`. See [API errors](https://docs.straddle.com/api-reference/errors) for response details.

## Set retries and timeouts

The client retries connection errors, `408`, `409`, `429`, and `5xx` responses twice by default. It uses exponential backoff and honors supported `Retry-After` values.

Use a context deadline to bound the complete operation, including retries. `option.WithRequestTimeout` sets a separate timeout for each attempt. Request options can be set on the client or passed after a method's parameters:

```go
page, err := client.Customers.List(
	ctx,
	sdk.CustomerListParams{PageSize: sdk.Int(10)},
	option.WithMaxRetries(0),
	option.WithRequestTimeout(10*time.Second),
)
if err != nil {
	log.Fatal(err)
}
fmt.Println(len(page.Data))
```

For write operations that accept an idempotency key, set the operation's `IdempotencyKey` field. Reuse that value when retrying the same operation. See [idempotency](https://docs.straddle.com/api-reference/idempotency).

## Configure transport and request options

The following options support client setup and individual requests.

| Option | Purpose |
| --- | --- |
| `option.WithBearer` | Set the API key |
| `option.WithBaseURL` | Set the API base URL |
| `option.WithEnvironmentStraddleApiServer` | Select the default sandbox environment |
| `option.WithMaxRetries` | Set the retry count; default `2` |
| `option.WithRequestTimeout` | Set the timeout for each attempt |
| `option.WithHTTPClient` | Supply an HTTP client or transport |
| `option.WithMiddleware` | Add request logging or tracing |
| `option.WithHeader` | Set a header |
| `option.WithQuery` | Set a query parameter |
| `option.WithRequestBody` | Supply a content type and body as bytes or an `io.Reader` |
| `option.WithResponseInto` | Capture the underlying `*http.Response` |
| `option.WithResponseBodyInto` | Override the response deserialization target |

Pass `option.WithResponseInto(&raw)` with `var raw *http.Response` to inspect the HTTP status and headers alongside the parsed result.

## Migrate from the straddleio module

The module moved from `github.com/straddleio/straddle-go`, whose last release is v0.2.0, to `github.com/straddle-build/straddle-go`. Version 1 changes the source API, so update imports and client setup together.

1. Add the new module:

   ```sh
   go get github.com/straddle-build/straddle-go@v1.0.4
   ```

2. Change every import of `github.com/straddleio/straddle-go` to `github.com/straddle-build/straddle-go`, including subpackages such as `option`.

3. Update client setup using the following mapping.

   | v0.2.0 | v1.0.4 |
   | --- | --- |
   | `option.WithAPIKey(key)` | `option.WithBearer(key)` |
   | Reads `STRADDLE_API_KEY` | Reads `BEARER`; pass `option.WithBearer(os.Getenv("STRADDLE_API_KEY"))` to keep the earlier variable |
   | `option.WithEnvironmentSandbox()` | `option.WithEnvironmentStraddleApiServer()`, also the default |
   | `option.WithEnvironmentProduction()` | `option.WithBaseURL("https://production.straddle.com/")` |

4. Remove the old requirement and rebuild:

   ```sh
   go mod tidy
   go build ./...
   ```

   Resolve remaining compile errors against the [method reference](./api.md). For example, the `shared` package no longer exists.

Rewrite imports instead of using a `replace` directive from the old module path to the new one. Version 1 imports its own module path internally, and Go rejects using one module version under two paths.

## Reference and support

Use the following resources as you build your integration:

- [SDK method reference](./api.md): operations, parameters, and response types.
- [Go package documentation](https://pkg.go.dev/github.com/straddle-build/straddle-go): exported types and methods.
- [Straddle guides](https://docs.straddle.com): payment flows, sandbox testing, and API concepts.
- [GitHub issues](https://github.com/straddle-build/straddle-go/issues): SDK bugs and feature requests.
- [Versioning and contributions](./VERSIONING.md): submit customizations against `scalar-next` so Scalar carries them through regeneration.
- [Security policy](./SECURITY.md) and [Apache 2.0 license](./LICENSE).

Straddle generates this SDK with Scalar and maintains repository customizations through the workflow in `VERSIONING.md`.
