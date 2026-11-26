# Shebka Homebrew tap

Install **zel**, the Zellij documentation and product website CLI by **Shebka**:

```sh
brew install mylife-inc/tap/zel
```

This tap currently supplies **0.2.0-rc.24**, a development preview. It supports
Apple Silicon and Intel Macs, and ARM64 and x86-64 Linux with glibc. The formula
uses the checksummed native binaries from our [public release mirror](https://github.com/mylife-inc/releases/releases/tag/zellij-v0.2.0-rc.24).
Zellij is the website engine; it is unrelated to the terminal multiplexer with
the same name. `mylife-inc` is Shebka's existing GitHub account.

## Create a portable documentation site

```sh
zel create my-docs
zel build my-docs
```

Edit Markdown in `my-docs/content/` and configure the site in `my-docs/zel.yaml`.
Open `my-docs/site/index.html` directly, or use `zel serve my-docs` for an HTTP
preview. Portable web-only builds run entirely inside the native CLI. Rich builds
download a matching renderer once, then reuse it from the cache. Neither needs
separately installed Docker, Node.js, npm or Python. Use `zel build my-docs --offline`
after the renderer is cached. Rich `--target static` builds share the cache for HTTP hosting. Image builds and explicitly selected container development/PDF retain their container prerequisites.
Use `zel pdf my-book` for PDF only, or `zel bp my-book` for a website with a
PDF download. PDF uses the verified cached compiler by default, without installed
Docker, Node.js, Python or TeX. The optional compiler is about 12 MB and reuses
the renderer's private Node; subsequent runs support `--offline`. Its matching
sources remain separate release downloads. PDF generation stays optional and
preserves the documented print-content limits. `zel bs` builds an HTTP static
site, `zel vw` previews and rebuilds edits, and `zel lint` runs deep/strict checks.

For the standard hosted wrapper, `zel dev my-site --open` downloads optional
verified development dependencies and reuses private Node and source packages.
No separately installed Docker, Node.js, npm or Python is needed.
`zel dev my-site --offline --open` reuses those packages after the first download.
Stopping removes the temporary session; downloaded packages remain cached.
`zel vw` is the lighter watched static-preview workflow. Custom Next apps keep
their own toolchain requirements.

Default `zel eject` and `zel new app` also need only `zel` to create editable
Next.js wrappers. A separate version-matched source archive is downloaded into
`ZELLIJ_HOME/cache/sources`; `--offline` reuses prepared sources. Each app gets
its own engine copy. Running the generated app still needs Node/npm. Explicit
image ejection and local-engine overrides keep their existing requirements.

Rich `zel check`, `zel watch` and `zel view --watch` also use the cached renderer
without separately installed tools. The first rich operation downloads it;
`--offline` reuses the verified cache for both `file` and `static` targets.
Static deployment builds and optional import validation also use that cache. Validation reports source errors without executing authored
MDX; live preview retains the last successful site after an invalid edit.

`zel templates` lists the bundled, versioned starters. For a reading-oriented
book, use `zel create my-book -t book-classic`. For a hosted product landing
page, use `zel create my-site -t product-studio`. Creation works offline and
protects existing files. Book-classic 0.3.0 includes a cover, preface, part,
chapters and appendix. `zel build my-book --pdf` adds a matching PDF and download
link using the cached LuaLaTeX toolchain; `zel bp my-book` is its short form.
An explicit `--pdf-backend container` uses Docker, Podman or nerdctl. PDF is off by default;
`zel pdf my-book --check` validates compatibility without a runtime. The three
print editions support English, French, German and Spanish LTR and Arabic RTL. Use `book-technical` for mathematics and
proofs or `book-study` for passages and commentary. With `pdf.enabled: true`,
`zel ni` also includes the matching PDF in a Next.js image. The LaTeX toolchain
stays outside the serving image. `pdf.style` controls colors, fonts, chapter
openings, spacing and margins; pinned Amiri/Libertinus fonts are included.
`zel create my-api -t api-platform` uses the light/dark Kie theme. The catalog
contains nine starters.

The formula installs Bash, Zsh and Fish completions. Shell setup follows
[Homebrew's completion instructions](https://docs.brew.sh/Shell-Completion).
Run `zel --help` for commands, and `zel explain` for the embedded content model.

The hosted section composer now includes video, layered layouts, semantic
content, product selectors, scroll stories and reuse of eight built-in sections.
Run `zel presets` to list nine editable section presets, or create one with
`zel ns -p product-overview --out content/sections/overview.yaml`.
Rich builds use the automatically downloaded matching renderer. Container commands
use the matching versioned engine image.

Homebrew keeps the executable in its managed Cellar/prefix. Zellij downloads use `ZELLIJ_HOME`, defaulting to `~/Dev/config/zellij`: renderer packages live under `cache/engines`, remote templates under `cache/templates`, editable sources under `cache/sources`, development dependencies under `cache/development`, and PDF compilers under `cache/pdf`. Set an absolute `ZELLIJ_HOME` to change that resource root. Standalone installers use its `bin` directory; Homebrew installation paths stay managed by Homebrew. Existing caches are not silently moved.

## Update or remove

```sh
brew update
brew upgrade mylife-inc/tap/zel
brew uninstall zel
```

Homebrew owns this installation. `zel upgrade` explains how to update through
Homebrew and does not replace Homebrew's binary. To verify an installation:

```sh
brew test mylife-inc/tap/zel
```

During pre-1.0 development, this tap tracks the newest verified preview and old
Zellij distribution artifacts may be retired. Do not depend on old preview
URLs or expect downgrades to remain available. Version 1.0 will follow the
agreed product-completion and acceptance gates.

## Maintainers

The formula is generated in [mylife-inc/Zellij](https://github.com/mylife-inc/Zellij)
with `scripts/homebrew-formula.mjs` from a release's verified `SHA256SUMS`.
Each release also attaches the generated `zel.rb`. After native release
acceptance succeeds, copy that exact formula to `Formula/zel.rb`, update this
README's version, and run this repository's four-platform acceptance workflow.
Do not substitute unchecked checksums or mutable download URLs.

The workflow installs the public artifact, checks completions and ownership,
builds a portable site, exercises a formula-revision upgrade, and uninstalls it
on disposable GitHub runners. It does not create provider deployments.

The binary's licensing is governed by the Zellij product, not by Homebrew.
No open-source licensing grant is implied by this public distribution tap.

Before retiring a prior preview, dispatch acceptance with `upgrade_from_commit`
set to its full tap commit SHA. This additionally installs the old public CLI,
upgrades to the current formula and checks both versions and portable builds.
Leave the input empty after retirement; ordinary lifecycle checks do not require
old downloads.
