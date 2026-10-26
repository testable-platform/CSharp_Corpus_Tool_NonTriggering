# OrderKit - CS_VNETCOREAPP30_SDKMSBUILD_NUGETPACKAGEREFERENCE_MONO

One branch of the C# tool-evaluation corpus. Test **data**, not a product: tool
execution is not part of the build, and evaluation happens later.

## Branch

`CS_VNETCOREAPP30_SDKMSBUILD_NUGETPACKAGEREFERENCE_MONO`

## Cell

| Axis | Value |
|---|---|
| Bundler | SDK-style MSBuild |
| Package manager | NuGet (PackageReference) |
| Architecture | Monolith |

Projects:

- `OrderKit` (`src/OrderKit`)

## Target framework

`netcoreapp3.0` - .NET Core 3.0

In support until 3 March 2020, and above the netstandard2.0 boundary, so Coverlet and OpenTelemetry are held out by NuGet alone (exit 4) rather than by the target framework (exit 3). This is also the first target FIRST family off the .NET Framework line, and the crossing the netfx472 record predicted. packages.config does not exist on .NET Core at all, so SIX of the 24 cells are now impossible rather than four -- the grid did not survive the crossing intact, and that is the finding, not a defect. Out of support since 3 March 2020 (3.0 was superseded by 3.1 within months), which is why it is the shortest-lived target framework in the corpus.

## Language version

C# 8.0, taken as the default of the target framework. `LangVersion` is deliberately
never written to any project file, which is what keeps the family axis one number
instead of two.

The domain layer is written to that language subset exactly, and the lock is
verified by execution rather than asserted:

```
csc -langversion:8.0   ->  clean
csc -langversion:7.3   ->  error CS8025: Feature 'async function' is not available
```

## Build

```bash
./build.sh
```

Restore will not succeed on a host where NuGet is unreachable; see `dataset.json`
`blockedHosts`.

## Test

```bash
./build.sh          # the test step is wired into the build
```

19 tests are declared. They are **not** executed in this corpus's build, because no
test framework can be restored.

## Tools non triggering (Synthetic Data)

One of five per-branch data folders in the original combined `testable-platform/csharp-corpus` repo -- this repo carries only this one.

Clean makes each tool run and report nothing wrong. Invalid makes it run and
report something. This folder holds data with nothing in it for any tool to
catch -- and, where no program would reach the tool at all, nothing for it to
start on. It is the negative control that tells *correctly detected nothing*
apart from *the scan never ran*.

`Tools non triggering (Synthetic Data)/` holds 17 tool-named folders, matching the names in Clean
and Invalid so the data sets line up name-for-name:

* **8 tools read source**, so they get their own minimal program, for netcoreapp30 -- one class or one function, no branching, no
  duplication, no dependency, no dead export, no magic number.
* **9 tools cannot be answered by a program** -- they read a lockfile,
  a coverage report, compiled bytecode or the commit history -- so they carry
  the subject matter as a plain record instead, with the reason stated in that
  folder's own README.

**No manifest, no lockfile, no tool configuration, no runner and no
`trigger.yaml` anywhere in it**, so the folder adds no discovered project and
no task to a run.

See `Tools non triggering (Synthetic Data)/README.md` for the per-tool table, the mechanism each tool is
inert by, and what was measured.
