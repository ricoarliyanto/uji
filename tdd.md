# Test Driven Development (TDD) Guidelines

## Workflow

### 1. Red - Tulis Test
```rust
// 1. Buat test baru
#[cfg(test)]
mod tests {
    #[test]
    fn test_feature() {
        // kondisi yang belum terimplementasi
        panic!("not implemented yet");
    }
}
```

### 2. Green - Tulis Code Minimal
```rust
// 2. Implementasi minimal untuk pass test
// Pastikan test PASS
```

### 3. Refactor - Perbaiki Code
```rust
// 3. Perbaiki code tanpa mengubah behavior
// Pastikan test masih PASS
```

## Test Structure

```
src/
├── lib.rs              // Library entry point
├── auth/
│   ├── mod.rs
│   ├── handler.rs
│   └── tests.rs        // Unit tests
├── authorization/
│   ├── mod.rs
│   ├── engine.rs
│   └── tests.rs
├── accounting/
│   ├── mod.rs
│   ├── handler.rs
│   └── tests.rs
├── radius/
│   ├── mod.rs
│   ├── packet.rs
│   └── tests.rs
└── tests/
    └── integration/
        └── full_flow.rs // Integration tests
```

## Requirements

### 1. Test Coverage Target
- **Unit Tests**: > 90%
- **Integration Tests**: > 80%
- **Mutation Testing**: > 85%

### 2. Test Categories
- **Unit Tests**: Single function/module
- **Integration Tests**: Multiple modules bekerja bersama
- **Acceptance Tests**: Full flow (auth → authz → accounting)
- **Fuzz Tests**: Random input untuk edge cases

### 3. Test Requirements
```rust
// Setiap test harus:
// - Deterministic (same input = same output)
// - Fast (< 100ms)
// - Independent (tidak bergantung urutan)
// - Self-contained (tidak perlu state global)

// Anti-pattern:
// ❌ Skip test karena "belum selesai"
// ❌ Test yang hard-coded value tanpa parameter
// ❌ Test yang bergantung pada external service
// ✅ Mock external dependencies
// ✅ Use fixtures untuk setup
// ✅ Test edge cases
```

### 4. CI/CD Integration
```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: dtolnay/rust-action@stable
        with:
          command: cargo test --all-features --all-targets
      - name: Coverage
        run: cargo tarpaulin --out html --out text
      - name: Lint
        run: cargo clippy --all-targets
```

### 5. Pre-Commit Checklist
- [ ] Semua test hijau (green)
- [ ] Coverage check passed
- [ ] Clippy warnings resolved
- [ ] Dokumentasi ditambahkan
- [ ] Error handling lengkap
