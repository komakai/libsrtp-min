# RECIPE: libsrtp-min

How the `main-min` branch of this repository was made from upstream, and
everything that differs from it. It is a submodule of webrtc-third_party-min (at `libsrtp`) for [webrtc-min](https://github.com/komakai/webrtc-min).

## Upstream

- Repository: https://chromium.googlesource.com/chromium/deps/libsrtp.git
- Revision: cd5d177bf1fde755ddb4c7f0d9ff7693f8b49e5e (2026-04-14T17:54:48+02:00)

The first commit on `main-min` is an unmodified snapshot of that revision, so
`git diff <first commit> main-min` shows every change made here.

## Stripped

Removed because nothing in the build needs them (run from the repository
root on the snapshot):

```sh
# Tests and gn files. config/config.h (Chromium's pregenerated config) is kept.
git rm -rq test
git rm -q BUILD.gn options.gni codereview.settings DIR_METADATA OWNERS run_all_tests.sh
```

## Deviations from upstream

- **CMake** (commit "Add CMake build"): `CMakeLists.txt` follows Chromium's
  BUILD.gn: the `libsrtp` library with `libsrtp_config`'s defines and include
  directories, using BoringSSL (a `boringssl` target from boringssl-min) for
  crypto. Chromium's pregenerated `config/config.h` is kept.

## Updating to a new upstream revision

`main-min` isn't a git fork of upstream (no upstream history), so updates are
re-applied rather than merged:

1. Replace the tree with upstream at the new revision and commit it as
   "Snapshot of upstream at <rev>" (`git rm -rq . && git archive` of the new
   revision, or a copy of a fresh checkout).
2. Re-run the strip commands above and commit.
3. Re-apply the deviation commits (`git cherry-pick` them from the previous
   `main-min` history) and fix up any conflicts.
4. Update the source lists in the CMake files for files upstream added,
   removed or renamed (compare with the new BUILD.gn in the snapshot commit),
   then build webrtc-min and compare with a gn build.
