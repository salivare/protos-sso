# Протокол SSO (Proto SSO contract)
### Обзор
Этот репозиторий содержит gRPC‑контракт для сервиса SSO (Auth), сгенерированный Go‑код и вспомогательный скрипт для генерации кода. Ниже приведены инструкции по началу работы, генерации Go‑исходников из файла .proto и рекомендованное уведомление об ответственности за код.

### Начало работы
- Go: 1.17+ (рекомендуется)

- protoc: установить с https://protobuf.dev/downloads

- плагины protoc:

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```
- Убедитесь, что $(go env GOPATH)/bin или GOBIN добавлены в ваш PATH.

### Генерация Go кода из proto
- Команда

```bash
protoc -I proto proto/sso/sso.proto \
--go_out=./gen/go --go_opt=paths=source_relative \
--go-grpc_out=./gen/go --go-grpc_opt=paths=source_relative
```
### Удобный скрипт
- Можно создать скрипт для автоматизации установки плагинов и генерации кода.

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
- Сделайте скрипт исполняемым и запустите:

```bash
chmod +x scripts/gen-proto.sh
./scripts/gen-proto.sh
```

### Заметки и рекомендации
Сгенерированный код будет размещён в ./gen/go при использовании приведённых выше команд.

Держите опцию option go_package в .proto согласованной с путём вашего модуля, чтобы избежать проблем с импортами.

Добавьте шаг генерации в CI, чтобы сгенерированные исходники всегда были актуальны перед сборкой.

Не коммитьте большие сгенерированные файлы, если это не политика вашего проекта; предпочтительнее генерировать их в CI или как часть процесса сборки.