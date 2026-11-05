# AUM-IC Sanitizer - Conformance & Verification Specification

**Status:** Technical Standard / Verification Specification  
**Authority:** Ingenieria Creativa (IngCrea)  
**Standard Identifier:** AUM-IC-SANITIZER-VERIF-1:2026  
**Governing Standard:** AUM-IC 7:2026 (AUM-IC-SPEC-7:2026)  
**License:** MIT License

---

## 1. Scope
This document specifies the mechanical operation, inspection passes, and automated mutation rules enforced by the **AUM-IC Sanitizer (`@ingcrea/aumic-sanitizer`)** to guarantee objective repository conformance with the AUM-IC 7:2026 standard.

## 2. Automated Conformance Passes

### 2.1. Pass 1: POSIX Line Ending Flattening (Clause 3.1)
- The validator **MUST** scan all UTF-8 text streams and replace all Carriage Return (CR, `0x0D`) bytes preceding Line Feeds (LF, `0x0A`) with a solitary LF byte (`0x0A`).
- The validator **MUST** reject or automatically rectify isolated CR (`0x0D`) characters.

### 2.2. Pass 2: Hexadecimal & Mojibake Forensic Audit (Clause 3.2)
- The engine **MUST** execute byte-level regex evaluation targeting Windows-1252 artifact tables (e.g., `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¡`, `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â©`, `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â³`, `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â±`, `ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬ÃƒÂ¢Ã¢â€šÂ¬Ã‚Â`).
- The engine **MUST** strip zero-width control points (U+200B, U+200C, U+200D, U+FEFF in non-initial byte streams).

### 2.3. Pass 3: Production Code Hygiene (Clause 3.4)
- When invoked in strict mode or pre-commit context, the validator **MUST** strip or flag active `console.log()` calls, `debugger;` statements, and orphan trace markers in non-test target directories.

## 3. Exit Codes & Gatekeeper Contracts
- **Exit Code 0:** Conformance verified (repository clean or self-healed successfully).
- **Exit Code 1:** Fatal non-conformance detected under non-mutating `--check` mode, preventing Git commit or CI/CD stage passage.\n