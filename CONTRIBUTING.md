# Contributing to Opentelemetry Rust contrib repo

The Rust special interest group (SIG) meets weekly on Tuesdays at 9 AM Pacific
Time. The meeting is subject to change depending on contributors'
availability. Check the [OpenTelemetry community
calendar](https://github.com/open-telemetry/community?tab=readme-ov-file#calendar)
for specific dates and for Zoom meeting links. "OTel Rust SIG" is the name of
meeting for this group.

Meeting notes are available as a public [Google
doc](https://docs.google.com/document/d/12upOzNk8c3SFTjsL6IRohCWMgzLKoknSCOOdMakbWo4/edit).
If you have trouble accessing the doc, please get in touch on
[Slack](https://cloud-native.slack.com/archives/C03GDP0H023).

The meeting is open for all to join. We invite everyone to join our meeting,
regardless of your experience level. Whether you're a seasoned OpenTelemetry
developer, just starting your journey, or simply curious about the work we do,
you're more than welcome to participate!

## Pull Requests

### How to Send Pull Requests

Everyone is welcome to contribute code to `opentelemetry-rust-contrib` via
GitHub pull requests (PRs).

```sh
git clone https://github.com/open-telemetry/opentelemetry-rust-contrib
```

Enter the newly created directory and add your fork as a new remote:

```sh
git remote add <YOUR_FORK> git@github.com:<YOUR_GITHUB_USERNAME>/opentelemetry-rust-contrib
```

Check out a new branch, make modifications, run linters and tests, and
push the branch to your fork:

```sh
$ git checkout -b <YOUR_BRANCH_NAME>
# edit files
$ git add -p
$ git commit
$ git push <YOUR_FORK> <YOUR_BRANCH_NAME>
```

Open a pull request against the main
[opentelemetry-rust-contrib](https://github.com/open-telemetry/opentelemetry-rust-contrib)
repo.

Your pull request should be named according to the
[conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) standard. This ensures that
when the PR is squashed into `main`, the resulting commit message is consistent and makes it easier
for us to generate a changelog  standard.

> **Note**
> It is recommended to run [pre-commit script](precommit.sh) from the root of
the repo to catch any issues locally.

### How to Receive Comments

- If the PR is not ready for review, please put `[WIP]` in the title or mark it
  as [`draft`](https://github.blog/2019-02-14-introducing-draft-pull-requests/).
- Make sure CLA is signed and all required CI checks are clear.
- Submit small, focused PRs addressing a single concern/issue.
- Make sure the PR title reflects the contribution.
- Write a summary that helps understand the change.
- Include usage examples in the summary, where applicable.
- Include benchmarks (before/after) in the summary, for contributions that are
  performance enhancements.

### How to Get PRs Merged

A PR is considered to be **ready to merge** when:

- It has received approval from
  [Approvers](https://github.com/open-telemetry/community/blob/main/community-membership.md#approver).
  /
  [Maintainers](https://github.com/open-telemetry/community/blob/main/community-membership.md#maintainer).
- Major feedbacks are resolved.

Any Maintainer can merge the PR once it is **ready to merge**. Note, that some
PRs may not be merged immediately if the repo is in the process of a release and
the maintainers decided to defer the PR to the next release train. Also,
maintainers may decide to wait for more than one approval for certain PRs,
particularly ones that are affecting multiple areas, or topics that may warrant
more discussion.

## Design Choices

As with other OpenTelemetry clients, opentelemetry-rust follows the
[opentelemetry-specification](https://github.com/open-telemetry/opentelemetry-specification).

It's especially valuable to read through the [library
guidelines](https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/library-guidelines.md).

### Focus on Capabilities, Not Structure Compliance

OpenTelemetry is an evolving specification, one where the desires and
use cases are clear, but the method to satisfy those uses cases are
not.

As such, Contributions should provide functionality and behavior that
conforms to the specification, but the interface and structure is
flexible.

It is preferable to have contributions follow the idioms of the
language rather than conform to specific API names or argument
patterns in the spec.

For a deeper discussion, see:
<https://github.com/open-telemetry/opentelemetry-specification/issues/165>

### Error Handling

Currently, the Opentelemetry Rust SDK has two ways to handle errors. In the situation where errors are not allowed to return. One should call global error handler to process the errors. Otherwise, one should return the errors.

The Opentelemetry Rust SDK comes with an error type `openetelemetry::Error`. For different function, one error has been defined. All error returned by trace module MUST be wrapped in `opentelemetry::trace::TraceError`. All errors returned by metrics module MUST be wrapped in `opentelemetry::metrics::MetricsError`.

For users that want to implement their own exporters. It's RECOMMENDED to wrap all errors from the exporter into a crate-level error type, and implement `ExporterError` trait.

### Priority of configurations

OpenTelemetry supports multiple ways to configure the API, SDK and other components. The priority of configurations is as follows:

- Environment variables
- Compiling time configurations provided in the source code

## Style Guide

- Run `cargo clippy --all` - this will catch common mistakes and improve
your Rust code
- Run `cargo fmt` - this will find and fix code formatting
issues.

## Testing and Benchmarking

- Run `cargo test --all` - this will execute code and doc tests for all
projects in this workspace.
- Run `cargo bench` - this will run benchmarks to show performance
- Run `cargo bench` - this will run benchmarks to show performance
regressions

## FAQ

### Where should I put third party propagators/exporters, contrib or standalone crates?

As of now, the specification classify the propagators into three categories:
Fully opened standards, platform-specific standards, proprietary headers. The
conclusion is only the fully opened standards should live in SDK packages/repos.
So here, only fully opened standards should live as independent crate. For more
detail and discussion, see [this
pr](https://github.com/open-telemetry/opentelemetry-specification/pull/1144).
