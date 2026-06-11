# Troubleshooting Guide

This guide documents common issues, error messages, and solutions encountered while using `run`. Please update this document whenever you encounter a new issue or find a solution.

---

## Table of Contents

- [Language Toolchain Issues](#language-toolchain-issues)
- [REPL Issues](#repl-issues)
- [Compilation and Execution Errors](#compilation-and-execution-errors)
- [Platform-Specific Issues](#platform-specific-issues)
- [Installation Issues](#installation-issues)
- [Logging and Debugging](#logging-and-debugging)
- [Known Issues](#known-issues)

---

## Language Toolchain Issues

### Toolchain Not Found

**Symptom:** Error message like `Language engine not available` or `command not found`

**Cause:** The required language runtime/compiler is not installed or not in PATH.

**Solution:**
1. Verify the toolchain is installed:
   ```bash
   which python3    # For Python
   which node       # For JavaScript
   which rustc      # For Rust
   which go         # For Go
   ```
2. Ensure the toolchain is in your PATH
3. Use `:languages` in REPL mode to see which engines are available

### Wrong Language Version

**Symptom:** Code fails due to version-specific syntax or features

**Solution:**
1. Check your installed version (e.g., `python3 --version`)
2. Update to a compatible version
3. Use explicit language flags: `run --lang python3 file.py`

---

## REPL Issues

### Variables Not Persisting

**Symptom:** Variables defined in one command are not available in subsequent commands

**Cause:** This may happen when switching languages or after a `:reset` command.

**Solution:**
- Remember that variables do NOT persist across language switches
- Use `:reset` intentionally to clear session state
- Check if auto-detection switched the language unexpectedly

### Multiline Input Problems

**Symptom:** Multiline code blocks don't execute correctly or produce syntax errors

**Solution:**
1. Ensure you paste the complete code block at once
2. For compiled languages, ensure proper brace matching
3. Check for trailing whitespace issues

### REPL Hanging or Unresponsive

**Symptom:** REPL doesn't respond to input

**Solution:**
1. Press Ctrl+C to interrupt
2. Press Ctrl+D to exit
3. Check if the underlying toolchain process is stuck
4. Restart the REPL session

---

## Compilation and Execution Errors

### Missing Main Function (Compiled Languages)

**Symptom:** Compiler errors about missing `main` function

**Context:** `run` auto-wraps code without main functions, but this doesn't always work.

**Solution:**
1. Provide a complete program with main function for complex code
2. For simple expressions, the auto-wrapping should work
3. Check language-specific notes in the README

### Include/Import Errors

**Symptom:** `use of undeclared identifier` or `module not found`

**Solution:**
1. For C/C++: Include necessary headers (`#include <iostream>`)
2. For Node.js: Use correct module paths
3. For TypeScript/Deno: Use `node:` prefix for Node builtins (e.g., `import fs from "node:fs"`)

### Shell Interpolation Issues

**Symptom:** Variables like `$name` in code are empty or cause errors

**Cause:** Shell is expanding variables before `run` sees them

**Solution:**
1. Use single quotes around code: `run python -c 'print("$literal")'`
2. Use quoted here-docs:
   ```bash
   cat <<'EOF' | run python
   name = "test"
   print(f"Hello, {name}!")
   EOF
   ```

---

## Platform-Specific Issues

### macOS

**Issue:** Permission denied when executing
**Solution:** Check Gatekeeper settings or run `xattr -cr /path/to/run`

**Issue:** Homebrew formula not found
**Solution:** Use the full URL: `brew install --formula https://github.com/Esubaalew/run/releases/latest/download/homebrew-run.rb`

### Linux

**Issue:** Missing shared libraries
**Solution:** Install required dependencies (glibc, libssl, etc.)

### Windows

**Issue:** Command not found after Scoop install
**Solution:** Restart terminal or refresh PATH: `refreshenv`

**Issue:** Line ending issues
**Solution:** Ensure files use LF line endings, not CRLF

---

## Installation Issues

### Cargo Install Fails

**Symptom:** `cargo install run-kit` fails to compile

**Solution:**
1. Update Rust: `rustup update`
2. Ensure Rust 1.70+ is installed
3. Check for missing system dependencies

### Binary Not in PATH

**Symptom:** `run: command not found` after installation

**Solution:**
1. For Cargo: Add `~/.cargo/bin` to PATH
2. For manual install: Add the installation directory to PATH
3. Restart terminal or source profile

---

## Logging and Debugging

### Enabling Verbose Output

Currently, `run` does not have a built-in verbose mode. For debugging:

1. Check stderr output - errors are printed there
2. Test the underlying toolchain directly to isolate issues
3. Use `:languages` in REPL to verify engine status

### Where to Find Logs

- **Temporary files:** Check `/tmp` or system temp directory for intermediate files
- **Compiler output:** Error messages include compiler stdout/stderr
- **REPL history:** Session state is kept in memory (not persisted to disk by default)

### Debugging Tips

1. **Isolate the issue:** Test the same code with the native toolchain
2. **Check language detection:** Use `--lang` flag to force specific language
3. **Minimal reproduction:** Reduce code to smallest failing example
4. **Check PATH:** Ensure toolchains are accessible

---

## Known Issues

### Auto-Detection Ambiguity

**Issue:** `print('hello')` could be Python, Ruby, Lua, etc.

**Solution:** Always use `--lang` flag for ambiguous syntax

### Groovy Shell Variable Expansion

**Issue:** `$variables` in Groovy code get expanded by shell

**Solution:** Use quoted here-docs (`<<'EOF'`)

### TypeScript Bare Imports

**Issue:** Deno doesn't support bare Node module names

**Solution:** Use `node:` prefix for Node builtins

---

## Contributing to This Guide

When you encounter a new issue:

1. Document the **symptom** (what you observed)
2. Identify the **cause** (if known)
3. Provide the **solution** (steps to resolve)
4. Add to the appropriate section above
5. Include any relevant error messages or code snippets

---

## Getting Help

- **GitHub Issues:** https://github.com/Esubaalew/run/issues
- **Documentation:** https://run.esubalew.et/docs/overview

---

*Last updated: 2026-01-29*
