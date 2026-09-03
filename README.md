# SSVC Rust Implementation

A Rust implementation of the **SSVC (Stakeholder-Specific Vulnerability Categorization)** specification.
SSVC is a framework for prioritizing software vulnerability remediation efforts. It helps stakeholders make informed decisions about which vulnerabilities to address first by considering factors like vulnerability severity, the stakeholder's position in the ecosystem, and their specific constraints.
Learn more at the [official SSVC documentation](https://certcc.github.io/SSVC/).

## Features

This library provides validation and processing of SSVC decision points and selection lists, with support for SSVC namespaces and extensions.
It features full serde support. The library supports both native Rust usage and WebAssembly (WASM) bindings for JavaScript/web applications.

## Installation

Add to your project:

```bash
cargo add ssvc
```

## MSRV

1.85.0

## Examples

### Rust

```rust
use ssvc::selection_list::SelectionList;
use ssvc::validate_selection_list;

let json_data = "..."; // Your SSVC selection list

let selection_list: SelectionList =
    serde_json::from_str(json_data).expect("SSVC SelectionList was invalid JSON");

// Validate the selection list
let result = validate_selection_list(&selection_list, false);

if result.success {
    println!("Selection list is valid!");
} else {
    for error in result.errors {
        println!("Validation error: {}", error.message);
    }
}
```

### WebAssembly

#### Build

```bash
# Install wasm-pack if you haven't already
cargo install wasm-pack
# Build for web
wasm-pack build --target web --out-dir pkg -- --features wasm
```

#### Usage

```javascript
import * as wasm from './pkg/ssvc.js';

const jsonData = {...}; // Your SSVC selection list

try {
  const result = wasm.validateSelectionList(JSON.stringify(jsonData), false);
  if (result.success) {
    console.log("Valid SSVC data");
  } else {
    console.log("Validation errors:", result.errors);
  }
} catch (error) {
  console.error("Error:", error);
}
```

## Commit Messages

Please use commit messages and pull request titles following
[Conventional Commits](https://www.conventionalcommits.org/)
when contributing to this project.

You can use a tool such as [commitlint](https://commitlint.js.org/)
to check your commit locally, e.g., by running:

    commitlint --default-config --last

to verify the latest commit. More options can be found with `--help`.

The CI workflow `pre-checks` will also ensure that PR titles and commit
messages in pushes to `main` conform to Conventional Commits.

In addition, warnings in the pipeline are emitted if the commits contained
within a PR do not adhere to Conventional Commits; these warnings do not fail
the pipeline.

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) file for details.
