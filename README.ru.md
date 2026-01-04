# Proto SSO contract

## Начиная
- Go 1.17+ (рекомендуется)
- Установлен protoc https://protobuf.dev/downloads
- Установлены плагины:
```Bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```
- Убедитесь, что $(go env GOPATH)/bin или GOBIN добавлены в PATH.
- Команда генерации
```Bash
protoc -I proto proto/sso/sso.proto \
  --go_out=./gen/go --go_opt=paths=source_relative \
  --go-grpc_out=./gen/go --go-grpc_opt=paths=source_relative

```
- Можно создать скрипт для удобства
```Bash
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