# k8sgpt Coding Conventions

**Language**: Go

**Style Guide**:
- Follow standard Go formatting (`gofmt`).
- Use `golangci-lint` for linting.
- Comments should be full sentences.
- Commits should follow conventional commits (if observed in `CONTRIBUTING.md`, but generally good practice).

**Project Structure**:
- `cmd/`: Command-line entry points (using Cobra).
- `pkg/`: Library code and core logic.
    - `pkg/analyzer/`: Implementation of various analyzers.
    - `pkg/ai/`: AI provider integrations.
- `charts/`: Helm charts for deployment.
