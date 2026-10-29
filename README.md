# C++ Core Guidelines skill

An English-language agent skill for designing, implementing, debugging, reviewing,
refactoring, modernizing, and optimizing C++.

## Standards and mandatory reference

The workflow covers C++98, C++03, C++11, C++14, C++17, C++20, C++23, and C++26.
It preserves the project's declared standard and checks compiler and standard
library support before recommending version-specific features.

The mandatory primary reference for design and review is the
[full C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines.html).
The [project home](https://isocpp.github.io/CppCoreGuidelines/) is the companion
source for scope and maintenance context. The skill requires applicable rules
to be read and verified before guideline-backed recommendations are made.

This is an independent agent skill. It is not an official skill published or
endorsed by Bjarne Stroustrup, Herb Sutter, the Standard C++ Foundation, or OpenAI.

## Install in Codex

Install separately on each computer where you use local Codex. Installation
in ChatGPT and installation in local Codex are separate product operations.

### Through Codex

Paste this into a Codex CLI or IDE-extension conversation:

```text
$skill-installer install https://github.com/asmorodin86/cpp-core-guidelines/tree/main/skills/cpp-core-guidelines
```

### Through a terminal

With a supported Node.js runtime and npm available:

```bash
npx skills add asmorodin86/cpp-core-guidelines --skill cpp-core-guidelines --agent codex --global
```

The global option installs for your user across projects on that computer.
Omit `--global` to install for the current project. Use `--copy` if your
environment does not support symbolic links.

If a skill with the same name already exists, inspect its source before
replacing it. Different skills with identical names are not automatically merged.

## Use

In Codex, open `/skills` and select `cpp-core-guidelines`, or invoke it explicitly:

```text
$cpp-core-guidelines Review this C++ project. Identify the target standard
and toolchain first, preserve the current baseline, and cite relevant
C++ Core Guidelines rules.
```

The skill text is English. Responses follow the user's requested language.

## Update

If installed with the skills CLI:

```bash
npx skills update cpp-core-guidelines --global
```

Run the update separately on each computer. Updates are not automatically
propagated between unrelated local installations.

## Included files

- `skills/cpp-core-guidelines/SKILL.md`: task selection and core workflow.
- `references/guidelines-and-sources.md`: rule navigation and authoritative sources.
- `references/standards-and-toolchains.md`: version compatibility and feature verification.
- `references/review-and-validation.md`: evidence-based reviews and proportionate checks.
- `agents/openai.yaml`: display metadata and invocation prompt.
- `assets/icon.svg`: skill icon.

The last five paths are relative to `skills/cpp-core-guidelines/`.

## Scope of the guarantees

Listing a language version does not guarantee that every feature is implemented
by every compiler or standard library. C++26 advice must be checked against the
relevant wording, implementation documentation, and the actual project environment.
The skill distinguishes verified checks from checks that were not run.

## Documentation

- [OpenAI: Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Skills CLI](https://github.com/vercel-labs/skills)
- [C++ Core Guidelines source](https://github.com/isocpp/CppCoreGuidelines)
