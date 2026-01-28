# Decomposer

**Explicit, Deterministic, Defying Composer**

> While Composer guesses, Decomposer knows.  
> PHAR, classmap, namespace-aware functions, and self-contained stubs — no dynamic scanning, no implicit execution.  
> You control the loader, we guarantee determinism.

---

## Overview

**Decomposer** is a standalone PHAR tool designed to fix Composer's “dynamic guessing” habits.  
It compiles every downloaded PHP package into a **PHAR archive** and generates a **.stub file** containing a self-contained autoloader.  
Developers only need to determine if PHAR metadata requires pre-processing before loading the package; all class and function loading is then handled automatically.

### Key Responsibilities

1. **PHAR Packaging & Metadata**
   - Each package is compiled into a PHAR archive.  
   - The package includes:
     - Class definitions
     - Namespace-aware function definition files
     - Full metadata (version, package name, dependencies)
   - Guarantees source code, dependencies, and loading order are immutable at runtime.

2. **Stub Autoloader**
   - Each PHAR package contains a **`.stub` file** that registers the package’s autoloader.  
   - Developers only decide whether **pre-processing of PHAR metadata** is required before using the package.  
   - Once stub is included, classes and functions are loaded deterministically.

3. **Manifest & Validation**
   - Generates a **classmap** with exact file locations.
   - Validates function and class files rigorously:
     - **Files with namespace functions that execute on load are rejected**  
     - **Files with global functions or classes without namespaces are rejected**  
     - **Files using namespaces outside the package's own namespace are rejected**  
   - Any package containing such illegal files will **immediately break the installation process**.
   - Runtime never scans or guesses — only uses explicit manifest and stub autoloaders.

4. **No Dynamic Loading**
   - Developers implement no dynamic discovery of classes or functions.  
   - Only manifest and stub autoloaders are trusted at runtime.

### Philosophy

> Composer autoloads magically, we autoload intentionally.

Decomposer exists to provide **clarity, determinism, and control** — ideal for long-running PHP services (Swoole, Swow) or FPM applications where startup consistency is critical.

---

## Why Decomposer Exists

- Composer guesses autoloading at runtime.  
- Decomposer enforces **explicit, deterministic, and verifiable loading**.  
- Guarantees **PHAR + stub + manifest = immutable, predictable behavior**.  
- Any package that violates namespace, stub, or load rules is **rejected immediately during installation**.

> In short: Composer composes, Decomposer decomposes — and does it intentionally.

---

## Contributing

Contributions, bug reports, and feature suggestions are welcome.  
Decomposer is designed with **strict manifest and stub control** — any dynamic-loading shortcuts will be rejected.

---

## License

MIT
