# Go プロジェクト運用ガイド

## レイアウト（推奨）
```
.
├── cmd/<app>/
├── internal/
├── pkg/
├── api/
├── web/             # Next.js（別apps/でもOK）
├── scripts/
└── Makefile
```

## ツール・Makefile 例
- `golangci-lint`, `govulncheck`, `gitleaks`
- `go test ./... -race -shuffle=on -cover`
- `go test -bench=. -benchmem`

```makefile
lint: ; golangci-lint run
test: ; go test ./... -race -shuffle=on -cover
bench: ; go test -bench=. -benchmem ./... | tee bench.txt
fuzz: ; go test -run=^$$ -fuzz=. -fuzztime=10s ./...
vuln: ; govulncheck ./...
```
