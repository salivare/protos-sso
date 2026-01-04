# Proto SSO contract
### Overview
This repository contains the gRPC contract for the SSO (Auth) service, generated Go code and a convenience script to generate the code. Below are instructions to get started, generate the Go sources from the .proto file and a recommended disclaimer about responsibility for the code.

### Getting started
- Go: 1.17+ (recommended)

- protoc: install from https://protobuf.dev/downloads

- protoc plugins:

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
Make sure $(go env GOPATH)/bin or GOBIN is added to your PATH.
```
- Generate Go code from proto
Command
```bash
protoc -I proto proto/sso/sso.proto \
--go_out=./gen/go --go_opt=paths=source_relative \
--go-grpc_out=./gen/go --go-grpc_opt=paths=source_relative
```
### Convenience script.
- You can create a script to automate plugin installation and code generation.

```bash
#!/usr/bin/env bash
set -euo pipefail

export PATH="$PATH:$(go env GOPATH)/bin"

command -v protoc >/dev/null 2>&1 || { echo "protoc not found"; exit 1; }
command -v protoc-gen-go >/dev/null 2>&1 || go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
command -v protoc-gen-go-grpc >/dev/null 2>&1 || go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

protoc -I proto proto/sso/sso.proto \
--go_out=./gen/go --go_opt=paths=source_relative \
--go-grpc_out=./gen/go --go-grpc_opt=paths=source_relative
```
- Make it executable and run:

```bash
chmod +x scripts/gen-proto.sh
./scripts/gen-proto.sh
```
### Notes and recommendations
- Generated code will be placed under ./gen/go when using the commands above.

- Keep option go_package in the .proto consistent with your module path to avoid import issues.

- Add the generation step to CI so generated sources are always up to date before builds.

- Do not commit large generated files unless that is your project policy; prefer generating in CI or as part of the build process.