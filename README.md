# Awesome Rust Migrations [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Migrating existing codebases to Rust — and eliminating memory-safety vulnerabilities at the source.

Rewriting C, C++, and other legacy code in Rust is now a serious, funded, engineering discipline: Google is rewriting dependencies with AI assistance, CISA and the NSA tell vendors to publish memory-safe roadmaps, and DARPA is paying to translate all of C. This list tracks the evidence, the rewrites, the tools, and the research.

## Contents

- [The Evidence](#the-evidence)
- [AI-Assisted Rewrites](#ai-assisted-rewrites)
- [Automated Translation and C2Rust Case Studies](#automated-translation-and-c2rust-case-studies)
- [Production Adoptions at Scale](#production-adoptions-at-scale)
- [Tools and Interop](#tools-and-interop)
- [Research](#research)
- [Guides, Articles, and Talks](#guides-articles-and-talks)
- [Adjacent Lists](#adjacent-lists)

## The Evidence

The data and policy behind the migration movement. Memory safety bugs are the majority of serious vulnerabilities in C and C++ codebases, and Rust measurably eliminates them.

- [Scaling Memory Safety: AI-Assisted Rewrites of C/C++ Dependencies to Rust](https://bughunters.google.com/blog/scaling-memory-safety) - Google's pilot using Gemini to rewrite C/C++ dependencies such as giflib into Rust, with the process, verification, and lessons from scaling AI-assisted translation.
- [Rust in Android: move fast and fix things](https://blog.google/security/rust-in-android-move-fast-fix-things/) - 2025 update: Android memory-safety vulnerabilities fell below 20% of the total for the first time, with Rust code showing roughly 1000x lower memory-safety vulnerability density than C and C++ code.
- [Eliminating memory safety vulnerabilities at the source](https://security.googleblog.com/2024/09/eliminating-memory-safety-vulnerabilities-Android.html) - Google Security Blog analysis of why new safe code beats rewriting old code, and how Android's strategy turns off the tap of new memory-unsafe code.
- [Chromium: Memory safety](https://www.chromium.org/Home/chromium-security/memory-safety/) - The Chromium project's canonical page: roughly 70% of serious security bugs are memory safety issues, and the plan to address them, including Rust.
- [The Case for Memory Safe Roadmaps](https://www.cisa.gov/resources-tools/resources/case-memory-safe-roadmaps) - Joint guidance from CISA, NSA, FBI, and international partners urging every technology manufacturer to publish a memory-safe roadmap and describe how memory-unsafe dependencies will be eliminated.
- [Software Memory Safety Is Cybersecurity](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-352a) - CISA advisory AA23-352A on memory safety as a national cybersecurity concern, with language-by-language migration recommendations.
- [Memory Safe Languages: Reducing Vulnerabilities in Modern Software Development](https://www.cisa.gov/resources-tools/resources/memory-safe-languages-reducing-vulnerabilities-modern-software-development) - Joint NSA and CISA Cybersecurity Information Sheet (2024) on adopting memory-safe languages, expanding on NSA's original Software Memory Safety guidance and the White House ONCD "Back to the Building Blocks" report.
- [Prossimo: Memory Safety](https://www.memorysafety.org/) - Internet Security Research Group initiative funding memory-safe rewrites and foundations, including `rav1d`, `rustls`, `zlib-rs`, and Rust support in the Linux kernel.
- [DARPA TRACTOR](https://www.darpa.mil/research/programs/translating-all-c-to-rust) - DARPA program to automate translation of legacy C code to Rust using software analysis and machine learning.
- [TRACTOR Benchmarks](https://www.ll.mit.edu/r-d/projects/translating-all-c-rust-tractor-benchmarks) - MIT Lincoln Laboratory benchmarks and evaluation work for DARPA's C-to-Rust translation program.

## AI-Assisted Rewrites

Projects pushing the limits of AI-assisted rewrites into Rust, where behavior compatibility and test suites are the bar.

- [Bun Zig-to-Rust rewrite](https://github.com/oven-sh/bun/pull/30412) - Bun merged "Rewrite Bun in Rust" on May 14, 2026 from the `claude/phase-a-port` branch, adding a Rust workspace across thousands of files; Bun canary builds are released from every `main` commit via `bun upgrade --canary`.
- [Grit](https://github.com/gitbutlerapp/grit) - GitButler's AI-authored Git reimplementation in Rust, targeting close behavior compatibility with upstream Git by porting and running the Git test suite against `grit-git`.
- [Ladybird's Rust adoption](https://ladybird.org/posts/adopting-rust/) - Browser project adopting Rust with AI assistance, starting with LibJS because it is relatively isolated and has extensive test coverage.
- [pacquet](https://github.com/pnpm/pacquet) - Official pnpm rewrite in Rust, porting the pnpm CLI from TypeScript to Rust while matching pnpm behavior, flags, defaults, error codes, file formats, and directory layout.
- [pgrust](https://pgrust.com/) - PostgreSQL rewrite in Rust using AI-assisted engineering, with PostgreSQL behavior and compatibility tests as the bar.
- [tsz](https://github.com/tsz-org/tsz) - AI-assisted Rust implementation of a TypeScript checker targeting drop-in `tsc` compatibility, with conformance progress reported against the official TypeScript test suite.

## Automated Translation and C2Rust Case Studies

These are mostly not AI-assisted, but they are the prior art for automated translation, cleanup, safety work, and compatibility.

- [libbzip2-rs](https://github.com/trifectatechfoundation/libbzip2-rs) - Drop-in compatible Rust implementation of bzip2 created with C2Rust and later used by the `bzip2` crate.
- [libyaml-safer](https://github.com/simonask/libyaml-safer) - Fully safe Rust fork of `unsafe-libyaml`, originally translated from libyaml with C2Rust.
- [rav1d](https://github.com/memorysafety/rav1d) - Fully safe Rust port of the `dav1d` AV1 decoder, created with C2Rust and then refactored toward safer, more idiomatic Rust.
- [rexpat](https://github.com/immunant/rexpat) - Rust port of Expat used as a C2Rust case study.
- [sapp-kms](https://docs.rs/sapp-kms/latest/sapp_kms) - C2Rust-derived port of sokol's KMS backend, cleaned up but still unsafe.
- [spiro.rlib](https://github.com/MFEK/spiro.rlib) - Fully safe C2Rust-derived port of the `spiro` spline interpolation library.
- [tsuki](https://github.com/ultimaweapon/tsuki) - Fully safe C2Rust-derived port of the Lua interpreter.
- [unsafe-libyaml](https://github.com/dtolnay/unsafe-libyaml) - Mostly direct C2Rust-derived port of libyaml, kept fully unsafe with minor cleanup.
- [zlib-rs](https://github.com/trifectatechfoundation/zlib-rs) - Rust implementation of zlib exposed as both a C dynamic library and a Rust crate.

## Production Adoptions at Scale

Not necessarily AI-assisted — these show large ecosystems replacing existing developer tooling and infrastructure with Rust implementations.

- [gRPC Rust](https://grpc.io/blog/grpc-welcomes-tonic/) - Tonic officially moved into the gRPC project under the CNCF, with Google building a new production-grade `grpc-rust` implementation alongside it.
- [Astro 7](https://astro.build/blog/astro-7/) - Astro rewrote its `.astro` compiler in Rust, made its Rust-powered Markdown and MDX pipeline the default, and reports 15-61% faster builds across benchmark sites.
- [Biome](https://biomejs.dev/) - Rust web toolchain for formatting, linting, and code analysis, positioned as a faster alternative to common JavaScript tooling.
- [Deno 2.0](https://deno.com/blog/v2.0) - Rust-based JavaScript and TypeScript runtime with Node.js and npm compatibility, plus built-in formatter, linter, test runner, and task runner.
- [InfluxDB Rust monolith](https://www.influxdata.com/blog/rust-monolith-migration-influxdb/) - InfluxData rewrote core account and resource management APIs from Go microservices into a single Rust monolith, using the strangler pattern for a zero-downtime migration.
- [Grab's Counter Service](https://engineering.grab.com/counter-service-how-we-rewrote-it-in-rust) - Grab deliberately rewrote a Go service in idiomatic Rust rather than translating it line by line, migrating traffic incrementally.
- [Lightning CSS](https://github.com/parcel-bundler/lightningcss) - Rust CSS parser, transformer, bundler, and minifier used by Parcel and other tools.
- [Next.js Compiler](https://nextjs.org/docs/architecture/nextjs-compiler) - Rust/SWC-based compiler that replaces Babel for individual files and Terser for minification in Next.js.
- [Oxc](https://oxc.rs/) - Rust JavaScript tooling stack covering parser, linter, formatter, transformer, resolver, and minifier.
- [Rolldown](https://rolldown.rs/) - Rust Rollup-compatible bundler used by Vite to replace its previous esbuild/Rollup split.
- [Rspack](https://rspack.rs/blog/announcing-1-0) - Rust Webpack-compatible bundler designed for progressive migration from Webpack.
- [Ruff](https://github.com/astral-sh/ruff) - Rust Python linter and formatter that replaces or consolidates tools such as Flake8, isort, and Black.
- [rustls](https://github.com/rustls/rustls) - Memory-safe TLS library in Rust, Prossimo-funded and available as an officially supported TLS backend in curl, giving the incumbent C TLS ecosystem a production Rust alternative.
- [Rustwright](https://github.com/Skyvern-AI/rustwright) - Alpha Rust reimplementation of Playwright's browser-control engine under Playwright-shaped Python and Node APIs; reports 515 shared parity cases and 1,046 Docker-gate tests, plus local diagnostic speed and client-memory gains, while explicitly saying full behavioral parity is not yet proven.
- [Tailwind CSS v4](https://tailwindcss.com/blog/tailwindcss-v4) - New high-performance Tailwind engine using Rust-powered pieces and Lightning CSS, with substantially faster full and incremental builds.
- [Turbopack](https://nextjs.org/blog/next-13) - Vercel's Rust-based successor to Webpack, introduced through Next.js.
- [Turso](https://github.com/tursodatabase/turso) - Rust rewrite of SQLite evolving into a pluggable database core; its new [PostgreSQL frontend](https://turso.tech/blog/a-new-modern-version-of-postgres-in-rust) compiles PostgreSQL syntax and types to Turso bytecode, targets common-application compatibility rather than 100% PostgreSQL parity, and documents simulation, oracle, fuzz, and formal-method testing.
- [uv](https://github.com/astral-sh/uv) - Rust Python package and project manager designed as a fast replacement for tools such as `pip`, `pip-tools`, `pipx`, `poetry`, and `virtualenv`.

## Tools and Interop

Translators, migration platforms, and the FFI/interop machinery that makes incremental migration practical.

- [C2Rust](https://github.com/immunant/c2rust) - C-to-Rust translator and refactoring toolkit for migrating C code to Rust.
- [Corrode](https://github.com/jameysharp/corrode) - Older C-to-Rust translator, useful historical context for deterministic translation.
- [ShiftCodex](https://shiftcodex.com/) - AI code migration platform built around translate, test, fix, and re-run loops.
- [autocxx](https://github.com/google/autocxx) - Google's tool for calling C++ from Rust with a safe, generated binding layer, designed for incrementally introducing Rust into large C++ codebases such as Chromium.
- [crubit](https://github.com/google/crubit) - Google's bidirectional C++ and Rust interoperability tooling.
- [cxx](https://github.com/dtolnay/cxx) - Safe interop between Rust and C++ with a shared bridge definition and static analysis of the boundary.
- [bindgen](https://github.com/rust-lang/rust-bindgen) - Generates Rust FFI bindings from C and C++ headers, the first tool most migrations touch.
- [uniffi](https://github.com/mozilla/uniffi-rs) - Mozilla's toolkit for generating foreign-language bindings from Rust, used to ship Rust cores behind Kotlin, Swift, and Python APIs.
- [corrosion](https://github.com/corrosion-rs/corrosion) - CMake integration for Cargo, for adding Rust targets to existing C and C++ builds.

## Research

Academic work on C-to-Rust translation, from LLM-assisted pipelines to program-analysis-guided migration.

- [C2RustXW](https://arxiv.org/abs/2603.28686) - C-to-Rust translation that combines program analysis, LLM translation, dependency-aware ordering, and execution-based validation.
- [EvoC2Rust](https://arxiv.org/abs/2508.04295) - Skeleton-guided framework for project-level C-to-Rust translation.
- [Rustine](https://arxiv.org/abs/2511.20617) - Repository-level C-to-Rust translation system targeting compilable, idiomatic, safer Rust with test-verified functional equivalence.
- [&inator](https://arxiv.org/abs/2604.17261) - Correct and precise C-to-Rust interface translation.
- [SafeTrans](https://arxiv.org/abs/2505.10708) - LLM-assisted C-to-Rust transpilation with iterative repair.
- [SACTOR](https://arxiv.org/abs/2503.12511) - LLM-driven multi-step C-to-Rust translation using static analysis.
- [RustMap](https://arxiv.org/abs/2503.17741) - Project-scale C-to-Rust migration using program analysis, LLMs, dependency guidance, and test feedback.
- [LLM4C2Rust](https://arxiv.org/abs/2604.15485) - Retrieval-augmented C/C++ to Rust transpilation framework focused on memory safety.
- [Scylla](https://arxiv.org/abs/2412.15042) - Formalized translation of an applicative subset of C to safe Rust, targeting memory safety by construction rather than after-the-fact cleanup.
- [CNnotator](https://arxiv.org/abs/2606.21822) - LLM-guided synthesis of CN memory-safety annotations for C code, cutting the manual annotation effort needed to verify legacy C during migration.

## Guides, Articles, and Talks

Practical write-ups on how teams actually run these migrations.

- [Memory-Unsafe Code Is a Liability](https://corrode.dev/blog/memory-safety/) - Corrode's 2026 essay on why memory safety is the lowest-hanging fruit in software security and where Rust fits in migration plans.
- [C++ to Rust Migration](https://blog.jetbrains.com/rust/2026/07/27/cpp-to-rust-migration/) - Luca Palmieri's guide to incremental C++ to Rust migration for large, active, customer-deployed systems.
- [Porting C to Rust for a Fast and Safe AV1 Media Decoder](https://www.memorysafety.org/blog/porting-c-to-rust-for-av1/) - Prossimo write-up on the goals and approach behind `rav1d`.
- [Making the rav1d Video Decoder 1% Faster](https://ohadravid.github.io/posts/2025-05-rav1d-faster/) - Performance case study on a C2Rust-derived port.
- [Translating bzip2 with C2Rust](https://trifectatech.org/blog/translating-bzip2-with-c2rust/) - Trifecta Tech Foundation write-up on translating bzip2 with C2Rust.
- [Rust Is Eating JavaScript](https://leerob.com/rust) - Lee Robinson's evolving survey of Rust replacing JavaScript tooling, updated in 2026 with Turbopack, Rolldown, Oxc, Biome, Rspack, Tailwind CSS v4, Deno, uv, and Ruff.
- [Using GPT-4 to Assist in C to Rust Translation](https://www.galois.com/articles/using-gpt-4-to-assist-in-c-to-rust-translation) - Galois experiment using GPT-4 for behavior-preserving refactoring in C2Rust output.
- [Function Argument Nullability Using an LLM](https://www.galois.com/articles/function-argument-nullability-using-an-llm) - Galois article on augmenting C2Rust migration analysis with an LLM.
- [Canonical Funded a PhD to Translate C to Rust. Ask Why.](https://www.beri.net/article/ai-c-to-rust-translation-residual-unsafe-code-canonical-phd) - Critical analysis of what AI C-to-Rust translation leaves behind, the residual-unsafe-code problem, and how Google's data argues for writing new safe code rather than translating the old corpus.
- [Ghosts in the Silicon: Fixing Memory Safety and Surviving Hardware Decay](https://josephhall.org/blog/wallach-mem-safety-sw-independence/) - Discussion with computer scientist Dan Wallach covering DARPA TRACTOR, the correctness-safety-idiomaticity trilemma of automated translation, and why manual rewrites cannot scale to the legacy C corpus.

## Adjacent Lists

- [Awesome Rust](https://github.com/rust-unofficial/awesome-rust) - Rust code and resources.
- [Awesome AI Rust Rewrites](https://github.com/malisper/awesome-ai-rust-rewrites) - The AI-assisted-rewrite-focused ancestor of this list.
- [Awesome Alternatives in Rust](https://github.com/TaKO8Ki/awesome-alternatives-in-rust) - Reimplementations and alternatives written in Rust.
- [Awesome Transpilers](https://github.com/milahu/awesome-transpilers) - Transpilers and source-to-source compilers.

## Footnotes

This list is intentionally narrow. It is not a general Rust list, a general AI coding list, or a list of greenfield Rust projects. Scale and ambition matter: entries should replace a meaningful surface of an established system or demonstrate unusual migration scope. Small wrappers, plugins, and accelerators are out of scope even when they are useful and well built.

Good entries should show at least one serious validation signal: test results, compatibility target, benchmark data, migration log, source and target code links, or discussion of unsafe code and semantic gaps.

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

This list started as an expansion of malisper's Awesome AI Rust Rewrites (CC0), which is linked in Adjacent Lists above.
