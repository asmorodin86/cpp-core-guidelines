# Standards and Toolchains

## Contents

- [Establish the actual baseline](#establish-the-actual-baseline)
- [Compatibility map](#compatibility-map)
- [Legacy C++98/03](#legacy-c9803)
- [Feature-availability procedure](#feature-availability-procedure)
- [C++26-specific checks](#c26-specific-checks)

## Establish the actual baseline

Check the applicable target's configuration, presets, toolchain, effective
compiler invocation, and deployment requirements. Account for target-specific
settings and compiler extensions. A newer compiler can still compile in an
older language mode, and a build can contain targets using different modes.

For CMake, inspect target properties and compile features as well as global
settings; verify effective commands when the configuration is ambiguous.
Do not change the global standard to solve one target's error without
understanding the affected consumers.

Record, when material:

- Required C++ standard and extension policy.
- Compiler vendor/version and target triple or platform.
- Standard-library implementation/version and runtime or sysroot.
- Exceptions, RTTI, threading, allocation, ABI, and freestanding restrictions.
- Actual build configuration and relevant flags.

## Compatibility map

Use this as a compact introduction-version reminder, not a claim that every
implementation supports every facility.

| Baseline | Representative facilities | Main compatibility caution |
| --- | --- | --- |
| C++98 / C++03 | Classes, templates, exceptions where enabled, STL, destructor-based RAII | Keep modern syntax and library facilities out of required legacy paths; model copying and ownership explicitly. |
| C++11 | Move semantics, lambdas, `nullptr`, `unique_ptr`, `shared_ptr`, atomics, threads, basic `constexpr` | `make_unique` is not part of C++11; preserve existing compatible helpers or construct the owner directly when appropriate. |
| C++14 | `make_unique`, generic lambdas, expanded `constexpr` | Do not assume C++17 facilities such as `optional` or `string_view`. |
| C++17 | `optional`, `variant`, `string_view`, `filesystem`, structured bindings, `if constexpr` | Views remain non-owning; library/link support can depend on the toolchain. |
| C++20 | Concepts, ranges, `span`, `jthread`, `format`, language coroutines and modules | Check each facility; modules require build-system and dependency integration. |
| C++23 | `expected`, `print`, explicit object parameters, further ranges and `constexpr` changes | Check the exact API and feature-test value; basic and extended operations can have different implementation status. |
| C++26 | Features in the intended C++26 wording, potentially including the requested reflection, contracts, or sender/receiver facilities | Gate each feature against current primary sources, the actual compiler/library, required flags, and a minimal probe. |

Consult [GCC](https://gcc.gnu.org/projects/cxx-status.html),
[Clang](https://clang.llvm.org/cxx_status.html), and the selected library's
[libstdc++](https://gcc.gnu.org/onlinedocs/libstdc++/manual/status.html),
[libc++](https://libcxx.llvm.org/), or
[MSVC](https://learn.microsoft.com/en-us/cpp/overview/visual-cpp-language-conformance)
documentation for the actual implementation.

## Legacy C++98/03

Use compatible RAII wrappers, scoped objects, explicit invariants, and the
project's existing containers and libraries. Distinguish a recommended
principle from a modern code example.

For an owning class, analyze destruction, copying, assignment, and partial
construction. If copying is not supported, use a compatible noncopyable
interface. If copying is supported, preserve the intended value semantics.
Do not introduce `= delete`, move operations, `nullptr`, or lambdas into a
legacy-only target.

Do not introduce a new dependency merely to emulate a newer feature when
a small project-compatible solution already exists. Treat a standard upgrade
as a separate migration decision with affected consumers and verification.

## Feature-availability procedure

Use this procedure when the answer depends on implementation availability.
For conceptual explanations, verify the relevant wording and documentation
without requiring a build environment. If the required toolchain is absent,
report support documented by the vendor and label local buildability as
unverified; provide a probe to run in the target environment when useful.

1. Name the exact capability and API, including required operations.
2. Identify its minimum standard and, for evolving features, the relevant
   paper revision and adoption into the intended wording.
3. Check the official compiler and library support records independently.
4. Inspect the appropriate `__cpp_*` or `__cpp_lib_*` macro after including
   the relevant standard header or `<version>` where available. Check the
   value required by the specific revision, not just whether it is defined.
5. When the relevant environment is available and support affects the answer,
   compile a representative minimal program with the real standard mode,
   compiler, library, sysroot, flags, and dependencies.
6. Link and, when applicable and runnable, execute the probe. For a cross
   target, distinguish host compilation, target linking, and target execution.
7. Record what the probe establishes and any remaining gaps. Offer a
   compatible alternative if the required configuration is unavailable.

Do not treat `__cplusplus`, compiler version, a header's existence, or
acceptance of `-std=c++26`/`-std=c++2c` as sufficient evidence. On MSVC,
check the documented standard-mode reporting and relevant options instead
of assuming that `__cplusplus` alone reflects the active mode.

## C++26-specific checks

- Distinguish proposals, Technical Specifications, adopted wording, defect
  reports, draft milestones, and published ISO standards.
- Do not assume that the newest WG21 working draft still describes C++26.
  Select a version-appropriate draft or adoption record.
- Check reflection syntax, library facilities, supported paper revisions,
  access rules, and any required experimental switches separately.
- Check contracts syntax and evaluation/enforcement behavior against the
  exact implementation. Do not substitute older proposals or GSL macros
  without labeling the difference.
- Distinguish C++17 execution policies from C++26 sender/receiver
  `std::execution`. A pre-existing `<execution>` header is not proof of
  the latter's availability.
- Label external implementations and experimental compiler forks explicitly;
  do not present them as the installed standard library.
- Do not generalize changes to uninitialized reads into a claim that all
  such reads are safe, defined, or acceptable.
- Keep compile-time evaluation and `constexpr` behavior specific to the
  language version. A function declared `constexpr` can still execute at
  runtime; that declaration is not a universal guarantee of freedom from UB.

Record the source date and actual toolchain whenever support determines the
recommendation. Use the live links in
[guidelines-and-sources.md](guidelines-and-sources.md).
