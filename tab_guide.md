---
title: Getting Started
layout: col-sidebar
tab: true
order: 1
tags: guide
---

## Getting Started

You may choose to use the prebuilt container images, GitHub Actions, or build
directly from the source code.

### Using prebuilt container images

Refer to [analyze-demo](https://github.com/naivesystems/analyze-demo) for an
example.

For projects using Makefiles, run the commands below in your project root:

```
podman pull ghcr.io/naivesystems/analyze:latest

mkdir -p output

podman run --rm \
  -v $PWD:/src:O \
  -v $PWD/.naivesystems:/config:Z \
  -v $PWD/output:/output:Z \
  ghcr.io/naivesystems/analyze:latest \
  /opt/naivesystems/misra_analyzer -show_results
```

A few notes:

* You may use `docker` instead of `podman` here.
  * Read the wiki to learn more about how to run on
    [Windows](https://github.com/naivesystems/analyze/wiki/How-to-run-on-Windows)
    and
    [macOS](https://github.com/naivesystems/analyze/wiki/How-to-run-on-macOS).
  * Running on Linux with `podman` is the only officially supported way in the
    Community Edition.

* You must configure the rules in `.naivesystems/check_rules`.
  - Refer to
    [analyze-demo](https://github.com/naivesystems/analyze-demo/blob/master/.naivesystems/check_rules)
    for an example.
  - Most (if not all) supported rules are listed in `rulesets/*.check_rules.txt` in this repository.

* You may remove `:Z` if you are not using SELinux.

* Replace `latest` with the actual version that you want to use.

NaiveSystems Analyze can trace and capture your build process automatically.
Currently we only publish Fedora-based images in the Community Edition, so your
code must compile successfully under Fedora Linux in order to use the prebuilt
container images. For other operating systems such as Debian, Ubuntu, CentOS, or
RHEL, please reach out to us to get the Enterprise Edition.

The analysis results are also available in the `output` directory. You may use
our [VS Code Extension](https://marketplace.visualstudio.com/items?itemName=naivesystems.analyze)
to view the results in Visual Studio Code.

In addition to Makefiles, we support many other project types. See also:

* [How to analyze CMake projects](https://github.com/naivesystems/analyze/wiki/How-to-analyze-CMake-projects)
* [How to analyze Keil MDK projects](https://github.com/naivesystems/analyze/wiki/How-to-analyze-Keil-MDK-projects)

### Using GitHub Actions

NaiveSystems Analyze supports running directly in GitHub Actions. For example,
[googlecpp-action](https://github.com/naivesystems/googlecpp-action) is our
officially published action for checking the Google C++ Style Guide. Refer to
[googlecpp-demo](https://github.com/naivesystems/googlecpp-demo) for more
information.

### Building from source

To build from source, follow the steps below on Fedora 37. Other versions
may also work but are not officially supported in the Community Edition.

1. Install build dependencies

```
dnf install -y autoconf automake clang cmake libtool lld make python3-devel wget which xz zip
```

2. Install Go 1.18 or later by following [the official instructions](https://go.dev/doc/install).

3. Install Bazel 6.0 or later by following [the official instructions](https://bazel.build/install).

4. Build the project

```
make
```

5. Build a container image

```
make -C podman_image build-en
```

This will build an image named `naive.systems/analyzer/misra:dev_en` for MISRA
C:2012. You may specify other targets if needed. Read the code for more details.

NaiveSystems Analyze can be built on a variety of Linux distros. For example,
the Community Edition in this repository can be built in GitHub Actions with the
official runner image of Ubuntu 22.04 LTS. For other operating systems such as
Debian, Ubuntu 18.04/20.04 LTS, CentOS 7/8, or RHEL and its derivatives, please
reach out to us to get the Enterprise Edition.
