# Suggested Commands for k8sgpt

## Build and Test
- **Build**: `make build` (builds the binary to `bin/k8sgpt`)
- **Test**: `make test` (runs unit tests)
- **Lint**: `make lint` (runs `golangci-lint`)
- **Vet**: `make vet` (runs `go vet`)

## Running the Tool
- **Run from source**: `go run main.go [command]`
- **Analyze**: `bin/k8sgpt analyze`
- **Analyze with Explanation**: `bin/k8sgpt analyze --explain`
- **Auth**: `bin/k8sgpt auth add`

## Installation (Dev)
- `make install`

## Docker
- `make docker-build`
