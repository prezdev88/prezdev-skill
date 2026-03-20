---
name: prezdev
description: Java code style guardrails for clean code refactors. Use when writing or reviewing Java code to enforce imports over FQN, top-down method order, shared utility extraction, and consistent English code style.
---

# Java Clean Rules

Adapted with additional guardrails from Oracle's Java Code Conventions:
https://www.oracle.com/a/tech/docs/java/codeconventions.pdf

## When To Use
- Any Java implementation or refactor.
- Any Java code review where readability and consistency matter.

## Mandatory Rules
1. Do not use FQN in code.
Use imports instead of `java.util.List`, `java.util.Map`, etc.  
Exception: string literals that must contain full class names (for example JAAS or generated DSL content).

2. Keep method order top-down.
If method `A` calls method `B`, `B` must be declared below `A`.
Public entry points first, private helpers after.

3. Keep code in English.
Class names, method names, variable names, comments, and log messages should be in English.

4. Put `try` at the start of the method when exception handling is required.
Avoid pre-`try` logic unless it is unavoidable (for example null guard clauses or trivial setup).

5. Extract reusable value helpers.
Repeated conversions/parsing like `asLong`, `asText`, `asDouble`, `nonBlankString`, normalization helpers, and similar logic must live in shared utility classes (for example `ValueUtils`), not duplicated inside feature classes.

6. Keep one public top-level type per source file.
The public class or interface should match the file name and appear first in the file.

7. Keep source file organization predictable.
Preferred order:
- file header comment only when the project already uses it
- `package`
- imports
- public top-level type

8. Use 4-space indentation and wrap long lines deliberately.
Avoid deeply nested wrapping.
When a declaration or condition must wrap, align continuation lines so the statement body remains easy to scan.

9. Put opening braces on the same line and closing braces on their own line.
Apply this consistently to type declarations, method declarations, and control structures.
Empty blocks may stay compact only when they are genuinely trivial.

10. Always use braces for control structures.
`if`, `else`, `for`, `while`, and similar blocks must use braces even for a single statement.

11. Keep one statement per line and one declaration per line.
Do not compress multiple statements or multiple variable declarations into a single line to save space.

12. Keep whitespace consistent.
- One blank line between methods.
- Space after keywords like `if`, `for`, `while`, `catch`, `switch`.
- No space between a method name and `(`.
- Space after commas.
- Space around binary operators.

13. Follow conventional Java naming.
- Classes and interfaces: PascalCase nouns.
- Methods: lowerCamelCase verbs.
- Variables: lowerCamelCase meaningful names; avoid one-letter names except short-lived loop counters.
- Constants: UPPER_SNAKE_CASE.

14. Avoid public mutable fields.
Prefer private fields with behavior-focused methods. Public fields are acceptable only for simple data-carrier structures when that style is already intentional in the codebase.

15. Access static members through the class, not an instance.
Prefer `TypeName.member()` over `instance.member()` for static calls and constants.

16. Avoid magic numbers.
Extract named constants for non-trivial literals.
Small loop counters such as `-1`, `0`, and `1` are acceptable when they are obvious.

17. Avoid embedded assignments and multi-assignment expressions.
Assignments should be straightforward and never rely on side effects for readability or brevity.

18. Use parentheses to make precedence explicit when expressions mix operators.
Prefer clarity over relying on readers to remember precedence rules.

19. Make returns match intent directly.
Prefer direct boolean returns and clear conditional expressions over verbose `if/else` return chains when readability improves.

20. Make `switch` behavior explicit.
Include a `default` branch.
If a case intentionally falls through, document it with a clear comment such as `/* falls through */`.

21. Use structured comment markers consistently.
Use `TODO` for pending work, `FIXME` for broken behavior, and `XXX` for suspicious but currently working code.

22. Never call a function directly inside another function's argument list.
Always evaluate the inner call first, store its result in a local variable, and pass that variable on the next line.
Avoid patterns like `foo(bar())`.
Prefer:
```java
Result result = bar();
foo(result);
```
This rule is mandatory for readability, debugging, and step-by-step comprehension.

23. Do not include file paths or class file references in explanations by default.
When explaining findings, flows, or refactors, describe the code by component or responsibility instead of listing file paths.
Only include explicit file paths if the user asks for them.

24. Do not create throwaway local variables only to return them on the next line.
If a value does not need an intermediate name for clarity, branching, or debugging, return it directly.
Avoid patterns like:
```java
Result result = compute();
return result;
```
Prefer:
```java
return compute();
```

## Review Checklist
- No new FQN usages in method signatures or bodies.
- No helper method defined above callers.
- No duplicated conversion/parsing blocks in use cases, mappers, or adapters.
- Code and logs are in English.
- If a method has `try/catch`, `try` starts the method body (except unavoidable guard/setup).
- One public top-level type per file, with predictable file ordering.
- Braces are present on all control structures.
- No multi-statement lines, multi-declarations, or embedded assignments added for brevity.
- Whitespace and brace placement are consistent and easy to scan.
- Naming follows standard Java conventions and avoids cryptic abbreviations.
- Static members are accessed through the class name.
- Non-trivial literals are extracted to named constants.
- `switch` statements include `default`, and intentional fall-through is documented.
- No function call is passed inline as an argument to another function; intermediate results are stored in local variables first.
- No file paths or file references are included in explanations unless the user explicitly requests them.
- No throwaway local variable is introduced only to be returned on the next line; direct returns are preferred when no intermediate name is needed.
