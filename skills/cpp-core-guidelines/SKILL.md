---
name: cpp-core-guidelines
description: >-
  Design, implement, debug, review, refactor, modernize, and optimize C++ using
  the C++ Core Guidelines as a mandatory reference. Use for C++98, C++03,
  C++11, C++14, C++17, C++20, C++23, and C++26 code; ownership, lifetime,
  templates, concurrency, API/ABI compatibility, and C++ build or toolchain
  questions. Preserve the project's standard and verify feature availability.
  Do not activate solely for ISO C, C#, or unrelated architecture tasks.
---

# C++ Core Guidelines

Deliver correct, maintainable C++ within the user's requested scope. Follow
explicit user instructions and applicable project guidance over this skill.
Keep explanations in the user's requested language and follow the project's
conventions for code, comments, formatting, and tests.

## Mandatory source

Use **https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines.html** as the
mandatory primary reference for C++ design and review recommendations.
Keep the [project home](https://isocpp.github.io/CppCoreGuidelines/) as the
required companion source for the document's scope and maintenance context.

Read the relevant rules in the
[full document](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines.html)
before making guideline-backed recommendations. A link to the landing page
alone does not count as consulting a rule. Check the
[upstream source](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)
when the rendered copy's freshness affects a decision.

- Verify rule identifiers, wording, examples, exceptions, and applicability.
- Cite the verified rule ID and a direct rule link for each substantive
  guideline-backed finding or design decision. Group findings when one rule
  supports the same reasoning.
- Treat Guidelines as engineering recommendations. Use the selected ISO
  standard or its identified draft for language semantics, and vendor
  documentation plus actual builds for implementation support.
- Preserve justified project constraints. Explain a material departure from
  a relevant guideline; do not classify every departure as a defect.
- If online access is unavailable, use a trustworthy supplied or existing
  local copy and identify its revision when known. Otherwise mark guideline
  verification as pending, continue code-based analysis where useful, and
  do not claim source verification or Guidelines compliance.
- Never invent rule IDs, quotations, paper status, support tables, or results.

Use [guidelines-and-sources.md](references/guidelines-and-sources.md) to select
relevant rules and authoritative sources. This index is a navigation aid,
not a substitute for reading the cited rules.

## 1. Establish the project contract

Inspect only the context needed for the task:

1. Read applicable project instructions and relevant code.
2. Determine the task: explain, design, implement, debug, review, migrate,
   optimize, or investigate a C++ build.
3. Identify the target standard for each affected build target:
   **C++98, C++03, C++11, C++14, C++17, C++20, C++23, or C++26**.
4. Inspect build configuration, presets, toolchain files, and effective compile
   commands. Record the compiler, standard library, target platform, and
   relevant flags when they affect the answer.
5. Identify material constraints: public API/ABI, exceptions, RTTI, allocation,
   threading, embedded/freestanding targets, supported platforms, dependencies,
   and performance requirements.

Preserve the existing baseline unless migration is requested. Do not infer
the language standard from the compiler version alone or from one target in
a mixed-standard build. State unresolved assumptions; ask only when a missing
fact changes the solution and cannot be established from available evidence.

For a new project without a specified baseline, state a provisional choice
based on deployment constraints and verified toolchain support. Do not choose
C++26 merely because it is the newest version mentioned.

## 2. Select compatible techniques

Read [standards-and-toolchains.md](references/standards-and-toolchains.md)
when the task crosses language versions, introduces a feature, or involves
C++26, modules, a new library facility, or portability.

- Separate the principle from the example's syntax. Apply RAII to C++98/03
  using compatible resource wrappers and the project's established facilities.
- State the minimum standard for newly introduced facilities when material.
- Distinguish standard-library facilities from GSL, Boost, external libraries,
  compiler extensions, experimental forks, and proposed features.
- Check language and library support independently.
- For C++26, identify the relevant adopted wording or draft and check current
  compiler and library documentation. When a recommendation depends on actual
  implementation support, inspect applicable feature-test macros and run a
  minimal compile/link probe if the relevant environment is available.
  Otherwise state that buildability is unverified; do not require environment
  setup for a conceptual explanation or document review.
- Never interpret a standard-mode flag, an existing header, or an unrelated
  overload as proof of complete feature support.
- Offer a compatible implementation or clearly labeled migration option when
  a requested facility is unavailable.

## 3. Apply the appropriate workflow

### Design and implementation

Define invariants, ownership, lifetime, error behavior, and interface contracts
before choosing abstractions. Prefer clear value semantics and scoped resources;
introduce dynamic allocation, inheritance, shared ownership, or concurrency when
requirements justify them. Evaluate templates, concepts, ranges, and coroutines
against the actual baseline and maintenance cost.

Make the smallest coherent change that satisfies the request. Preserve public
behavior and API/ABI unless changing them is part of the task. Follow established
project error handling rather than imposing exceptions, error codes, or
`std::expected` universally. Explain consequential design choices using verified
Guidelines rules and project requirements.

### Review

Stay read-only unless fixes are requested. Trace behavior, ownership, lifetime,
and failure paths before reporting findings. Distinguish:

- **Defect:** a supported failure scenario or violated correctness requirement.
- **Design risk:** a plausible hazard whose triggering conditions are not
  established as a current defect.
- **Improvement:** a maintainability or clarity opportunity without a demonstrated
  correctness failure.

Derive severity from impact and evidence, not the wording of a guideline.
Include a precise location, scenario, relevant rule, compatible remedy, and
verification status. Do not manufacture findings to populate a checklist.

### Debugging and refactoring

Reproduce or isolate the failure where possible. Separate the cause from
symptoms. Preserve intended behavior, ownership contracts, and supported
platforms. Add a focused regression check when it materially protects the fix.
For refactoring, compare observable behavior and compatibility, not only style.

### Modernization and optimization

Identify the requested migration boundary and affected consumers. Separate
required changes from optional modernization. For performance work, obtain a
baseline and a profile, change the demonstrated bottleneck, and remeasure under
comparable conditions. Do not claim an improvement without measurements.
Prefer established synchronization over introducing unproved lock-free designs.

## 4. Verify proportionately

Use [review-and-validation.md](references/review-and-validation.md) for targeted
checks and reporting. Reuse project build/test commands and tool configurations.

- For code or build changes, build affected targets in their declared language
  modes when the required environment is available.
- Run relevant existing tests and focused regression checks for material
  behavior changes.
- Use static analysis, sanitizers, ABI checks, benchmarks, or concurrency checks
  only when they resolve a concrete risk or satisfy project requirements.
- Respect platform support, cross-compilation constraints, and existing warning
  policy. Do not add blanket flags or replace the build system by default.
- Report commands actually run, their results, and what remains unverified.
- Distinguish a proposed check, a failed environmental setup, a failed test,
  and a passing check. Compilation or a clean analyzer run does not prove
  complete correctness or conformance.

## 5. Deliver the result

Lead with the outcome, then provide only the details needed to assess it:

1. Applicable target standard and material toolchain assumptions.
2. Code, design, or findings that directly answer the request.
3. Verified Guidelines rules and source links supporting substantive advice.
4. Compatibility implications and justified guideline departures.
5. Actual validation and concrete residual uncertainty.

Use the adaptable formats in
[review-and-validation.md](references/review-and-validation.md).
Keep routine answers short; expand when the task or evidence requires it.
