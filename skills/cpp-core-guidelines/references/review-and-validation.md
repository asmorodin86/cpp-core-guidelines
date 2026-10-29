# Review and Validation

## Contents

- [Choose checks from the changed behavior](#choose-checks-from-the-changed-behavior)
- [Review method](#review-method)
- [Build and analysis discipline](#build-and-analysis-discipline)
- [Performance discipline](#performance-discipline)
- [Final evidence summary](#final-evidence-summary)

## Choose checks from the changed behavior

| Area | Inspect | Useful evidence when applicable |
| --- | --- | --- |
| Ownership and lifetime | Cleanup paths, aliasing, invalidation, borrowed views, escaping callbacks | Focused lifetime tests; ASan/UBSan where supported; relevant static analysis |
| Copy/move and exceptions | Class invariants, self-assignment, partial construction, guarantees, destructor behavior | Value-semantics tests and representative failure paths |
| Interfaces and conversions | Nullability, bounds, signedness, narrowing, preconditions, error contracts | Boundary and error-path tests with actual callers |
| Concurrency | Happens-before relationships, object lifetime, lock ordering, callback reentrancy, shutdown | Concurrency tests; TSan where supported; analysis of synchronization |
| Templates and generic code | Constraints, substitution, overload resolution, instantiation coverage, ODR | Representative positive/negative compilation cases |
| API/ABI and portability | Exported symbols, layout, calling convention, public headers, platform assumptions | Consumer builds and the project's ABI checks |
| Build configuration | Actual target standard, include paths, definitions, library/runtime selection | Effective compile/link commands and affected target builds |
| Performance | Workload, profile, allocation, contention, algorithmic cost, cache behavior | Repeated comparable measurements and correctness checks |

Use only the rows relevant to the task. A review can report a demonstrated
defect from code evidence without executing the entire project; say what was
and was not verified.

## Review method

1. Establish the requested scope and baseline.
2. Inspect relevant callers, ownership boundaries, and failure paths.
3. Read the applicable Core Guidelines rules.
4. Form a concrete claim and identify its evidence.
5. Distinguish current failures, conditional risks, and optional improvements.
6. Suggest a compatible fix and a proportionate verification step.

For a requested review, do not modify project files unless fixing them is
authorized. Do not report unrelated cleanup as a blocking defect.

### Finding format

Adapt to the user's requested format; keep the evidence fields:

- **Location:** file and the smallest useful location or symbol.
- **Category / severity:** defect, design risk, or improvement; severity based
  on demonstrated consequences.
- **Scenario:** the input, lifetime, exception, or interleaving that matters.
- **Evidence:** the code path or actual diagnostic; label assumptions.
- **Guideline:** verified rule ID and direct source link, when applicable.
- **Remedy:** a concrete fix compatible with the target standard and constraints.
- **Verification:** what ran, its result, or the focused check still needed.

A missing guideline citation does not invalidate an independently established
language or runtime defect. Use the correct normative or implementation source
instead of forcing an unrelated Guidelines rule onto it. Disclose unavailable
mandatory-source verification as required by SKILL.md.

### Design format

State the relevant requirements and baseline, then describe:

1. Ownership, lifetime, invariants, and error contracts.
2. The proposed interface and implementation approach.
3. Material alternatives and tradeoffs.
4. Relevant verified Guidelines rules and justified departures.
5. Compatibility and the validation approach.

Skip fields that add no information for a small task.

## Build and analysis discipline

- Prefer existing presets, targets, test runners, and analyzer configurations.
- Use target-scoped configuration changes for a justified local requirement.
- Compile with the declared standard and check both compile and link results.
- Enable appropriate warnings without blindly imposing `-Werror` or new
  platform-specific flags on an established project.
- Use configured `clang-tidy` or equivalent analysis where it answers a
  concrete question. Tool coverage is partial; verify findings in context.
- Run ASan/UBSan or TSan only in supported, suitable configurations; respect
  documented incompatibilities and use separate configurations when needed.
- Add regression tests for material behavioral fixes, not tests that merely
  repeat the implementation or checks for trivial formatting changes.
- Do not hide diagnostics by adding suppressions, casts, or blanket
  initialization without resolving the cause.
- Do not change public ABI, dependency versions, exception policy, or
  language baseline as an incidental workaround.

## Performance discipline

Measure a representative workload before and after the change. Keep build
mode, inputs, hardware, and execution conditions comparable. Use repetitions
and describe variability where material. Confirm unchanged required behavior.
Label estimates and unmeasured hypotheses; avoid claims such as "zero cost",
"thread-safe", or "faster" without appropriate evidence.

## Final evidence summary

For each material check, distinguish:

- **Passed:** command actually ran and met the stated criterion.
- **Failed:** command ran and exposed a test, build, or analysis failure.
- **Blocked:** a concrete environmental limitation prevented execution.
- **Not run:** the check was not executed; give the reason if material.
- **Not applicable:** the check does not address the change's risks.

Report the relevant standard, compiler/library assumptions, actual commands,
outcomes, unverified behavior, and justified guideline departures. A passing
build or analyzer does not establish complete standard conformance, absence
of undefined behavior, or universal Guidelines compliance.
