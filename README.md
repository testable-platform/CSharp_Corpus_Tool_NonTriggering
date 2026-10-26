# OrderKit - CS_VNETFX45_DOTNETCLI_NUGETPACKAGESCONFIG_MICRO

One branch of the C# tool-evaluation corpus. Test **data**, not a product: tool
execution is not part of the build, and evaluation happens later.

> **This cell cannot exist.** `dotnet restore` has never supported packages.config. NuGet's packages.config path is implemented in MSBuild.exe / nuget.exe only.
>
> The branch ships the artifacts the cell definition demands, plus this notice. It is *not* quietly backed by a working configuration -- a branch labelled one thing that ran another underneath is the exact silent success this corpus exists to catch.

## Branch

`CS_VNETFX45_DOTNETCLI_NUGETPACKAGESCONFIG_MICRO`

## Cell

| Axis | Value |
|---|---|
| Bundler | .NET CLI |
| Package manager | NuGet (packages.config) |
| Architecture | Microservices |

Projects:

- `OrderKit.Domain` (`src/OrderKit.Domain`)
- `OrderKit.Contracts` (`src/OrderKit.Contracts`)
- `OrderKit.Pricing` (`src/Services/OrderKit.Pricing`)
- `OrderKit.Risk` (`src/Services/OrderKit.Risk`)
- `OrderKit.Reporting` (`src/Services/OrderKit.Reporting`)

## Target framework

`net45` - .NET Framework 4.5

The oldest TFM named anywhere in the source metric sheet. Out of support since 12 January 2016; retained deliberately as the corpus floor.

## Language version

C# 5, taken as the default of the target framework. `LangVersion` is deliberately
never written to any project file, which is what keeps the family axis one number
instead of two.

The domain layer is written to that language subset exactly, and the lock is
verified by execution rather than asserted:

```
csc -langversion:5   ->  clean
csc -langversion:4   ->  error CS8025: Feature 'async function' is not available
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

* **8 tools read source**, so they get their own minimal program, for netfx45 -- one class or one function, no branching, no
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
