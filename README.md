# json-api-builder-create-folders

[![npm version](https://img.shields.io/npm/v/json-api-builder-create-folders?color=cb3837&style=flat-square)](https://www.npmjs.com/package/json-api-builder-create-folders)
[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)
[![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen.svg?style=flat-square)](package.json)
[![GitHub Repository](https://img.shields.io/badge/GitHub-keshavsoft%2Fjson-api-builder-create-folders-181717.svg?style=flat-square&logo=github)](https://github.com/keshavsoft/json-api-builder-create-folders)
[![Documentation](https://img.shields.io/badge/docs-live_portal-amber.svg?style=flat-square)](https://keshavsoft.github.io/json-api-builder-create-folders/)

> **Stage 1 of 8**: Stage 1: Structural Directory Scaffolding from JSON Specification Trees

Part of the modular **[JSON API Builder](https://github.com/keshavsoft/json-api-builder-orchestrator)** ecosystem by [KeshavSoft](https://github.com/keshavsoft).

---

## The Story

Traverses tree.json specifications using json-traversal-core to create the physical directory structure under the client's output directory. It guarantees that all necessary parent paths and nested route directories exist before any files or code are generated.

### The Problem It Solves
Monolithic generators mix directory creation with file templating. When intermediate folder generation fails, partial files are left behind, leading to corrupted, half-written project states.

### The Solution
Decouples directory structure creation into a deterministic, idempotent, and standalone stage. It safely ensures every target directory exists before any files or code are touched.

---

## Architecture & Pipeline Position

```
[1. Folders] ──▶ [2. Files] ──▶ [3. Content] ──▶ [4. Hook Routes]
                                                         │
[8. Orchestrator] ◀── [7. Validate] ◀── [6. Endpoints] ◀── [5. App]
```

**This repository implements Stage 1 in isolation.** It can be used independently in custom build workflows or composed automatically via `json-api-builder`.

---

## Installation

```bash
npm install json-api-builder-create-folders
```

---

## Quick Start

```js
import { createFolders } from "json-api-builder-create-folders";

const result = createFolders({
    clientRoot: "./my-project",
    outputRoot: "./my-project/output",
    treePath: "./my-project/input/source-of-truth/tree.json"
});

console.log("Created directories under:", result.outputRoot);
```

---

## Input Specification

Nested tree.json where objects represent directories and string leaves point to route definitions.

```json
{
  "api": {
    "routes.js": "routes.estree.json",
    "v1": {
      "routes.js": "routes.estree.json",
      "clinics": {
        "routes.js": "routes.estree.json"
      }
    }
  }
}
```

---

## Output Transformation

Pure directory nodes created on the file system, perfectly matching the tree structure.

```text
output/
└── api/
    └── v1/
        └── clinics/
```

---

## API Reference

### `createFolders`

Accepts an options object with the following configuration:

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `clientRoot` | `string` | `process.cwd()` | Root path of the client project containing input and output. |
| `outputRoot` | `string` | `path.join(clientRoot, 'output')` | Destination directory where folders will be created. |
| `treePath` | `string` | `input/source-of-truth/tree.json` | Absolute or relative path to the tree.json definition file. |

---

## Sibling Pipeline Packages

| Stage | Package | Description |
| :---: | :--- | :--- |
| **01** | [json-api-builder-create-folders](https://github.com/keshavsoft/json-api-builder-create-folders) | Directory hierarchy scaffolding |
| **02** | [json-api-builder-create-files](https://github.com/keshavsoft/json-api-builder-create-files) | Non-destructive placeholder file creation |
| **03** | [json-api-builder-insert-content](https://github.com/keshavsoft/json-api-builder-insert-content) | ESTree template to JavaScript insertion |
| **04** | [json-api-builder-alter-content](https://github.com/keshavsoft/json-api-builder-alter-content) | Hierarchical route hooking & AST imports |
| **05** | [json-api-builder-create-app](https://github.com/keshavsoft/json-api-builder-create-app) | Root app.js assembly & server runtime |
| **06** | [json-api-builder-create-endpoints](https://github.com/keshavsoft/json-api-builder-create-endpoints) | CRUD endpoints & local JSON file sync |
| **07** | [json-api-builder-validation](https://github.com/keshavsoft/json-api-builder-validation) | Specification linting & artifact checks |
| **08** | [json-api-builder](https://github.com/keshavsoft/json-api-builder-orchestrator) | Master orchestrator coordinating all stages |

---

## Development & Testing

```bash
# Install dependencies
npm install

# Run automated tests
npm test

# Run syntax check
npm run check

# Prepublish verification
npm run prepublishOnly
```

---

## License

[MIT](LICENSE) © 2026 [KeshavSoft](https://github.com/keshavsoft)
