# Buildifier

buildifier is a tool for formatting bazel BUILD files with a standard convention.

## Setup

Build the tool:
* Checkout the repo and then either via `go install` or `bazel build //buildifier`
* If you already have 'go' installed, then build a binary via:

`go get github.com/bazelbuild/buildtools/buildifier`

## Usage

Use buildifier to create standardized formatting for BUILD files in the
same way that clang-format is used for source files.

`$ buildifier -showlog -mode=check $(find . -type f \( -iname BUILD -or -iname BUILD.bazel \))`

## Setup and usage via Bazel

You can also invoke buildifier via the Bazel rule.
`WORKSPACE` file:
```bzl
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# buildifier is written in Go and hence needs rules_go to be built.
# See https://github.com/bazelbuild/rules_go for the up to date setup instructions.
http_archive(
    name = "io_bazel_rules_go",
    sha256 = "c1f52b8789218bb1542ed362c4f7de7052abcf254d865d96fb7ba6d44bc15ee3",
    url = "https://github.com/bazelbuild/rules_go/releases/download/0.12.0/rules_go-0.12.0.tar.gz",
)

http_archive(
    name = "com_github_bazelbuild_buildtools",
    strip_prefix = "buildtools-<commit hash>",
    url = "https://github.com/bazelbuild/buildtools/archive/<commit hash>.zip",
)

load("@io_bazel_rules_go//go:def.bzl", "go_register_toolchains", "go_rules_dependencies")
load("@com_github_bazelbuild_buildtools//buildifier:deps.bzl", "buildifier_dependencies")

go_rules_dependencies()

go_register_toolchains()

buildifier_dependencies()
```

`BUILD.bazel` typically in the workspace root:
```bzl
load("@com_github_bazelbuild_buildtools//buildifier:def.bzl", "buildifier")

buildifier(
    name = "buildifier",
)
```
Invoke with
```bash
bazel run //:buildifier
```
