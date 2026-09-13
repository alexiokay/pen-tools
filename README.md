# Pen Tools — now PenTraceTools

**The shared homepage is now [github.com/PenTraceTools](https://github.com/PenTraceTools).**
This earlier overview remains as a navigation link; the two source repositories
have moved to the organization without changing their pinned release contents.

A shared home for a reusable pen-position filter and the application used to
record, compare and investigate its behavior. **Experimental, source-first,
and independently versioned.** This repository is the overview, not a third
implementation or a binary installer.

| Component | Start here | Role |
| --- | --- | --- |
| **pen-stabilizer** | [Source & integration](https://github.com/PenTraceTools/pen-stabilizer) · [v0.1.0](https://github.com/PenTraceTools/pen-stabilizer/releases/tag/v0.1.0) | MIT C++17 header-only local-normal position correction |
| **PenTraceLab** | [Source & build instructions](https://github.com/PenTraceTools/pen-trace-lab) · [CI builds](https://github.com/PenTraceTools/pen-trace-lab/actions) | Windows pen/touch capture, raw/filtered comparison, replay and metrics |
| **InfiniPaint integration** | [Fork: graphite-ui](https://github.com/alexiokay/infinipaint-Custom/tree/graphite-ui) · [Upstream proposal #98](https://github.com/ErrorAtLine0/infinipaint/pull/98) | A drawing-app consumer; not the owner of the algorithm |

InfiniPaint is created and maintained by [ErrorAtLine0](https://github.com/ErrorAtLine0/infinipaint).
The linked integration is a fork/proposal, not an upstream endorsement or merged feature.

## One algorithm source, different hosts

PenTraceLab compiles the library source and calls its batch interface for the local
candidate. InfiniPaint compiles the same pinned header and calls its streaming
interface while drawing. Other Lab comparison candidates are independent alternatives,
not a cascade applied on top of the shared filter.

The library handles position correction. Hosts retain ownership of pen APIs,
timestamps/units, pressure mapping, drawing, undo and file formats. No required DLL,
service, driver, runtime download, IPC protocol or Rust rewrite is involved.

## Which version?

Current documented integration baseline:

- Library: **v0.1.0**, algorithm revision **1**, commit
  **adbdce4e902433fcd14fba16e08863f2ec909f79**.
- PenTraceLab app: **0.4.1**, with that exact submodule pin.
- InfiniPaint: **graphite-ui** and upstream proposal **#98** pin the same source.
  PR #97 adds pressure policies and deliberately has no filter dependency.
- App versions and library versions are separate. Updates never silently change
  an already-built executable.

See [versioning and contribution workflow](VERSIONING.md). Read each consumer's
gitlink for its authoritative pin; this table describes a baseline, not a claim
that every branch or downloaded build is current. GitHub source ZIPs do not include
submodule contents. On a build PC, use:
```sh
git clone --recurse-submodules <consumer-repository-url>
# After pulling a new consumer revision:
git submodule update --init deps/pen-stabilizer
```

PenTraceLab portable artifacts are under its successful Actions runs; an app
version in source does not imply a matching GitHub Release exists.

## What the filter does — and does not do

The newest reported tip remains exact. A recent tail can revise with bounded
local-normal correction; older positions freeze. There is no forced line snapping
or post-lift beautification. Larger neighborhoods can soften intended detail.

Raw Windows reports are not physical ground truth. Speed estimates derive from
those reports; no software-only test measures the pen's electrical signals or
proves the exact intended trajectory. Synthetic tests establish mathematical
invariants and regressions, not a guaranteed fix for slow diagonal wobble.

## Where to contribute

- Algorithm/API bugs and conformance tests: **pen-stabilizer**.
- Recording, device metadata, comparison UI and diagnostic metrics: **PenTraceLab**.
- Drawing integration, pressure controls and rendering: **InfiniPaint fork/PRs**.
- Shared documentation and component navigation: **this repository**.

Link cross-component issues rather than copying implementations. Keep private
recordings local unless their owner explicitly chooses to share them.

The library and diagnostic app are now owned by PenTraceTools, with independent
issues and releases. InfiniPaint remains an external consumer under its existing owner.
The organization profile replaces this repository as the primary landing page.
