# Source and release workflow

## Version identities

1. **Package version** (currently pen-stabilizer 0.1.0): source/API release.
   Pre-1.0 API stability is not guaranteed; release notes explain compatibility.
2. **Algorithm revision** (currently 1): recorded behavior identity. A change to
   correction math, defaults or boundary semantics must document whether replay
   changes and update the behavior revision when it does.
3. **Consumer version** (for example PenTraceLab 0.4.1): app features/build.
4. **Git commit pin**: exact dependency contents; authoritative for reproducibility.

Documentation-only commits need not create a package release. Never move an
existing release tag to newer contents. Consumers need not follow documentation
commits just to stay on the same algorithm. Do not depend on a moving main branch.

## Updating the shared filter

- Implement and review an algorithm/API change in pen-stabilizer first.
- Run conformance, invariants, sanitizers and benchmarks. Retain the independent
  original PenTraceLab oracle; using the current shared adapter as the only oracle
  would test the code against itself.
- Compare real recordings locally in PenTraceLab, including slow/fast strokes,
  corners, intentional curves, stationary pressure and timing discontinuities.
- Publish a versioned source release with API/behavior notes and MIT license.
- Explicitly bump each consumer's submodule commit in its own PR, e.g. on a build PC:
  ```sh
  git -C deps/pen-stabilizer fetch origin --tags
  git -C deps/pen-stabilizer switch --detach <reviewed-release-tag>
  git add deps/pen-stabilizer
  ```
- Run consumer tests, build and test its UI on a device. Passing library tests is
  not enough to establish rendering latency, physical accuracy or input correctness.
- Keep the previous consumer commit available for rollback. Rebuild and version
  each app independently; a dependency bump alone does not update its releases.

## Host integration contract

The source API accepts positions, time, a width attribute and continuity information.
The host normalizes coordinate units/clocks and owns sample validity, contact
boundaries, cancellation, pressure policy and drawing. InfiniPaint's adapter clamps
its supported UI parameter range; PenTraceLab can intentionally explore a wider range.

Streaming renderers must support a revisable recent tail. Do not feed the corrected
output back into the input filter. Destructive erasing cannot safely reuse that tail
without a distinct reversible design. Batch and streaming entry points serve different
host workflows but share the core position math.

For other languages, assess native bindings or a tested port later. No binary ABI,
cross-language protocol or Rust rewrite is promised in this project.

## Repository organization

Keep the library small for consumers. Keep capture/diagnostic/rendering code in the
apps. Use this overview to connect them. The PenTraceTools organization now provides the shared profile. Ownership grouping
does not change source pins or combine the projects into one package.
