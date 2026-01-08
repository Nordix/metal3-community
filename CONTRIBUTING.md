# How to Contribute to Metal3

Metal3 projects are [Apache 2.0 licensed](LICENSE) and accept contributions via
GitHub pull requests.

This guide provides common contributing guidelines for all Metal3 projects.
Individual repositories may have additional specific requirements documented
in their own CONTRIBUTING.md files.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Certificate of Origin](#certificate-of-origin)
   - [Git commit Sign-off](#git-commit-sign-off)
- [Finding Things That Need Help](#finding-things-that-need-help)
- [Contributing a Patch](#contributing-a-patch)
- [Pull Request Labels](#pull-request-labels)
- [Code Review Guidelines](#code-review-guidelines)
- [Versioning and Release Semantics](#versioning-and-release-semantics)
   - [What Goes in a Release](#what-goes-in-a-release)
- [Branches](#branches)
   - [Branch Structure](#branch-structure)
   - [Release Branch Support](#release-branch-support)
- [Breaking Changes](#breaking-changes)
   - [Examples of Breaking Changes](#examples-of-breaking-changes)
   - [Exceptions](#exceptions)
- [Backporting](#backporting)
   - [Backport Criteria](#backport-criteria)
   - [Backport Process](#backport-process)
   - [Supported Versions](#supported-versions)
- [Merge Approval](#merge-approval)
- [Issue and Pull Request Management](#issue-and-pull-request-management)
- [Commands and Workflow](#commands-and-workflow)
- [Release Process](#release-process)
   - [General Release Guidelines](#general-release-guidelines)
   - [Project-Specific Release Steps](#project-specific-release-steps)
- [Google Doc Viewing Permissions](#google-doc-viewing-permissions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Certificate of Origin

By contributing to this project you agree to the Developer Certificate of
Origin (DCO). This document was created by the Linux Kernel community and is a
simple statement that you, as a contributor, have the legal right to make the
contribution. See the [DCO](DCO) file for details.

### Git commit Sign-off

Commit message should contain signed off section with full name and email. For example:

```text
Signed-off-by: John Doe <jdoe@example.com>
```

When making commits, include the `-s` flag and `Signed-off-by` section will be
automatically added to your commit message. If you want GPG signing too, add
the `-S` flag alongside `-s`.

```bash
# Signing off commit
git commit -s

# Signing off commit and also additional signing with GPG
git commit -s -S
```

## Finding Things That Need Help

If you're new to the project and want to help, but don't know where to start,
look for issues labeled with `good first issue` in any of the Metal3 repositories:

- [baremetal-operator](https://github.com/metal3-io/baremetal-operator/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
- [cluster-api-provider-metal3](https://github.com/metal3-io/cluster-api-provider-metal3/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
- [ip-address-manager](https://github.com/metal3-io/ip-address-manager/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
- [ironic-standalone-operator](https://github.com/metal3-io/ironic-standalone-operator/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)

Alternatively, read the documentation, try to write your own code or
examples, and file issues for any problems or gaps in documentation that you
encounter!

## Contributing a Patch

1. If you haven't already done so, sign a Contributor License Agreement
   (see details in the [Kubernetes CLA documentation](https://github.com/kubernetes/community/blob/master/CLA.md)).
2. Fork the desired repository, develop and test your code changes.
3. Submit a pull request.

## Pull Request Labels

All code PRs should be labeled with one of the following to indicate the type of
change:

- ⚠️ (`:warning:`, major or breaking changes)
- ✨ (`:sparkles:`, feature additions)
- 🐛 (`:bug:`, patch and bugfixes)
- 📖 (`:book:`, documentation or proposals)
- 🌱 (`:seedling:`, minor or other)

Individual commits should not be tagged separately, but will generally be
assumed to match the PR. For instance, if you have a bugfix along with a
breaking change, it's generally encouraged to submit the bugfix separately,
but if you must put them in one PR, mark the commits separately.

## Code Review Guidelines

All changes must be code reviewed. Coding conventions and standards are
explained in the official [Kubernetes developer
docs](https://github.com/kubernetes/community/tree/master/contributors/devel).

Expect reviewers to request that you avoid common [Go style
mistakes](https://github.com/golang/go/wiki/CodeReviewComments) in your PRs.

## Versioning and Release Semantics

### What Goes in a Release

Metal3 projects generally follow these versioning semantics:

- A (**minor**) release CAN include:
   - Introduction of new API versions, or new Kinds
   - Compatible API changes like field additions, deprecation notices, etc.
   - Breaking API changes for deprecated APIs, fields, or code
   - Features, promotion or removal of feature gates
   - And more!

- A (**patch**) release SHOULD only include backwards compatible set of bugfixes

## Branches

### Branch Structure

Metal3 projects follow a common branch structure:

- The **main** branch is where development happens. All the latest and greatest
  code, including breaking changes, happens on main.

- The **release-X.Y** branches contain stable, backwards compatible code. On every
  major or minor release, a new branch is created. It is from these branches that
  minor and patch releases are tagged.

- In some cases, it may be necessary to open PRs for bugfixes directly against
  stable branches, but this should generally not be the case.

### Release Branch Support

Each project maintains specific support matrices for their release branches.
See individual project CONTRIBUTING.md files for:

- Supported versions and EOL dates
- API version support policies
- Test coverage policies

## Breaking Changes

Breaking changes are generally allowed in the `main` branch, as this is the
branch used to develop the next minor release.

There may be times, however, when `main` is closed for breaking changes. This
is likely to happen as we near the release of a new minor version.

Breaking changes are **not allowed** in release branches, as these represent
minor versions that have already been released. These versions have consumers
who expect the APIs, behaviors, etc. to remain stable during the lifetime of
the patch stream for the minor release.

### Examples of Breaking Changes

- Removing or renaming a field in a CRD
- Removing or renaming a CRD
- Removing or renaming an exported constant, variable, type, or function
- Updating the version of critical libraries such as controller-runtime,
  client-go, apimachinery, etc.
   - Patch versions of these libraries are generally not considered breaking changes
   - Some version updates may be acceptable for picking up bug fixes, but
    maintainers must exercise caution when reviewing

### Exceptions

There may be times when breaking changes are allowed in release branches.
These are at the discretion of the project's maintainers and must be carefully
considered before merging. An example of an allowed breaking change might be a
fix for a behavioral bug that was released in an initial minor version.

## Backporting

We generally do **not** accept PRs directly against release branches. However,
we do accept backports of fixes/changes already merged into the main branch.

### Backport Criteria

We generally allow backports of the following kinds of changes to all
maintained branches:

- Critical bug fixes, security issue fixes, or fixes for bugs without easy workarounds
- Dependency bumps for CVE resolution (usually limited to CVE fixes; backports of
  non-CVE related version bumps are considered exceptions to be evaluated case
  by case)
- Changes required to support new Kubernetes patch versions, when possible
- Changes to use the latest Go patch version to build controller images
- Changes to bump the Go minor version used to build controller images, if the
  Go minor version of a supported branch goes out of support (e.g., to pick up
  bug and CVE fixes)
- Improvements to existing documentation
- Improvements to the test framework

### Backport Process

Like any other activity in the project, backporting a fix/change is a
community-driven effort and requires that someone volunteers to own the task.

In most cases, the cherry-pick bot can (and should) be used to automate
opening a cherry-pick PR. To use it, add a comment on the PR you want to
backport:

```text
/cherry-pick release-X.Y
```

### Supported Versions

We generally do not accept backports to release branches that are EOL (End of Life).
Check the [Version Support](https://github.com/metal3-io/metal3-docs/blob/main/docs/user-guide/src/version_support.md)
guide for each project's specific support policy.

## Merge Approval

Please see the [Kubernetes community document on pull
requests](https://git.k8s.io/community/contributors/guide/pull-requests.md) for
more information about the merge process.

Metal3 follows the standard Kubernetes workflow: any PR needs `lgtm` and
`approved` labels, and PRs must pass all tests before being merged.

## Issue and Pull Request Management

Anyone may comment on issues and submit reviews for pull requests. However, in
order to be assigned an issue or pull request, you must be a member of the
[Metal3-io organization](https://github.com/metal3-io) on GitHub.

Metal3 maintainers can assign you an issue or pull request by leaving a
`/assign <your Github ID>` comment on the issue or pull request.

## Commands and Workflow

Metal3 projects follow the standard Kubernetes workflow and use Prow for CI/CD.

We use the same priority and kind labels as Kubernetes. See the labels tab in
GitHub for the full list in each repository.

The standard Kubernetes comment commands work in Metal3 projects. See
[Prow command help](https://prow.apps.test.metal3.io/command-help) for a
command reference.

Common commands include:

- `/lgtm` - Adds the `lgtm` label (Looks Good To Me)
- `/approve` - Adds the `approve` label
- `/hold` - Adds the `do-not-merge/hold` label
- `/hold cancel` - Removes the hold
- `/retest` - Reruns failed tests
- `/assign @username` - Assigns an issue or PR
- `/cc @username` - Requests a review

## Release Process

### General Release Guidelines

Metal3 projects follow these general release practices:

- **Minor and patch releases** are generally done immediately after a feature or
  bugfix is landed, or sometimes a series of features tied together.

- **Minor releases** will only be tagged on the *most recent* major release branch,
  except in exceptional circumstances. Patches will be backported to maintained
  stable versions, as needed.

- **Major releases** are done shortly after a breaking change is merged. Once a
  breaking change is merged, the next release *must* be a major revision.
  We don't intend to have many of these, so we may defer merging breaking PRs
  until an appropriate time.

### Project-Specific Release Steps

Each project has specific release procedures and automation. Refer to each
project's documentation:

- [baremetal-operator releasing docs](https://github.com/metal3-io/baremetal-operator/blob/main/docs/releasing.md)
- [cluster-api-provider-metal3 releasing docs](https://github.com/metal3-io/cluster-api-provider-metal3/blob/main/docs/releasing.md)
- [ip-address-manager releasing docs](https://github.com/metal3-io/ip-address-manager/blob/main/docs/releasing.md)
- [ironic-standalone-operator releasing docs](https://github.com/metal3-io/ironic-standalone-operator/blob/main/docs/releasing.md)
- Check individual project repos for their specific release documentation

## Google Doc Viewing Permissions

To gain viewing permissions to Google Docs used in Metal3 projects, please
join the [metal3-dev](https://groups.google.com/forum/#!forum/metal3-dev)
Google group.
