# scoped-variable-jep

# Java Enhancement Proposal (JEP)

**JEP:** XXXX  
**Title:** Scoped Mutable Variables  
**Author:** Alaa Mohamed  
**Reviewers:** [To be assigned]  
**Status:** Draft  
**Target Release:** Future Java Version (e.g., Java 22+)  
**Type:** Feature Proposal  

---

## Summary
This proposal introduces a new variable declaration modifier, `scoped`, which allows variables to be mutable exclusively within a specific lexical scope (e.g., within `if`, `for`, or `while` blocks). Modifications to such variables inside the block do not propagate to their counterparts in outer scopes. This feature aims to reduce unintended side effects and enhance code safety and clarity.

---

## Motivation
In large-scale Java codebases, it is common to encounter bugs caused by unintended mutations of variables declared outside but modified within control-flow blocks. Developers often resort to creating local copies or using immutable wrappers to mitigate this risk, which introduces verbosity and may obscure intent. A built-in mechanism to confine mutability to a limited scope would address this pattern in a clean, expressive manner.

---

## Goals
- Introduce a concise syntax (`scoped`) to restrict variable mutability within a given code block.  
- Preserve the value of the variable in the enclosing scope, regardless of changes within the scoped block.  
- Ensure seamless integration with Java's existing type system and scoping rules.  
- Maintain full backward compatibility with existing Java code.  

---

## Non-Goals
- This proposal does not alter the semantics of existing keywords like `final`.  
- It is not intended to enforce immutability or adopt a purely functional paradigm.  
- It does not introduce runtime scope barriers—this is purely a compile-time feature.  

---

## Specification

### 4.1 Syntax
```java
scoped var x = 10;
// or
scoped int x = 10;


## 4.2 Semantics
A scoped variable introduces a new symbol with an initial value.

Any nested block (if, for, while, etc.) that references the variable creates a shadowed instance for local reassignment.

Upon exiting the block, the shadowed instance is discarded, and the outer variable remains unchanged.

***************************************
Examples

scoped int count = 100;
if (condition) {
    count = 50; // modifies only the shadowed instance
    System.out.println(count); // prints 50
}
System.out.println(count); // prints 100 (original value preserved)
****************************************



Compatibility and Migration
This feature introduces a new keyword (scoped) and does not interfere with existing identifiers or reserved words.

Legacy code will compile and function unchanged.

IDEs and compilers will require minor updates to parse and enforce the new semantics.

Alternatives Considered
Manual duplication of variables before a block (error-prone and verbose).

Linting tools to detect in-block reassignments (non-preventive, relies on discipline).

Use of wrapper objects or final variables with new scope declarations.

Proposed Implementation
Parser: Extend grammar to accept scoped as a valid local variable modifier.

Symbol Table: Generate unique symbols for scoped declarations and their shadows.

Bytecode Generation: Compiler emits separate synthetic variables for scoped blocks; no changes required at JVM level.

Future Work
Expand support to additional constructs such as switch statements and lambda expressions.

Explore interactions with value types introduced under Project Valhalla.

Consider enabling scoped immutability annotations for further safety guarantees.

References
JLS §6.3 — Scope of Declarations: https://docs.oracle.com/javase/specs/jls/se21/html/jls-6.html

JEP 286 — Local-Variable Type Inference: https://openjdk.org/jeps/286

mathematica
Copy
Edit
