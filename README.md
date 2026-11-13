# 48046

Reproduction for [Renovate PR 48046](https://github.com/renovatebot/renovate/pull/48046).

## Current behavior

Renovate removes mise sidecars for `pypi` dependencies. To reproduce, bump the `pypi:zensical` version in `mise.toml` to `0.1.1` and run `mise lock`:

```console
$ mise lock
→ Targeting 7 platform(s) for mise.lock: linux-arm64, linux-arm64-musl, linux-x64, linux-x64-musl, macos-arm64, macos-x64, windows-x64
→ Processing 3 tool(s): python@3.11.18, uv@0.13.8, pypi:zensical@0.1.1
mise WARN  pypi:zensical dependency graph skipped: uv >= 0.12.10 is unavailable; install uv and run `mise lock` again
✓ Lockfile ready at mise.lock
✓ Pruned 1 stale version entry from mise.lock: pypi:zensical@0.1.0

$ git status
On branch main
Your branch is based on 'origin/main', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    .mise/locks/pypi-zensical/0.1.0/pyproject.toml
	deleted:    .mise/locks/pypi-zensical/0.1.0/uv.lock
	modified:   mise.lock
	modified:   mise.toml
```

## Expected behavior

Renovate should update the sidecar correctly:

```console
$ mise lock
→ Targeting 7 platform(s) for mise.lock: linux-arm64, linux-arm64-musl, linux-x64, linux-x64-musl, macos-arm64, macos-x64, windows-x64
→ Processing 3 tool(s): python@3.11.18, uv@0.13.8, pypi:zensical@0.1.1
lock                    pypi:zensical@0.1.1 linux-x64-musl                                                                                                                                                 ✔
Using CPython 3.11.18 interpreter at: /path/to/python
Resolved 13 packages in 273ms
Using CPython 3.11.18 interpreter at: /path/to/python
Would create project environment at: .venv
Would download 3 packages
Would install 11 packages
 + click==8.5.1
 + jinja2==3.1.6
 + markdown==3.11.3
 + markupsafe==3.0.4
 + pathspec==1.1.1
 + pygments==2.21.0
 + pymdown-extensions==13.1
 + pyyaml==6.0.3
 + tomli==2.5.0
 + zensical==0.1.1
 + zensical-ui==0.0.42
✓ Lockfile ready at mise.lock
✓ Pruned 1 stale version entry from mise.lock: pypi:zensical@0.1.0

$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	renamed:    .mise/locks/pypi-zensical/0.1.0/pyproject.toml -> .mise/locks/pypi-zensical/0.1.1/pyproject.toml
	renamed:    .mise/locks/pypi-zensical/0.1.0/uv.lock -> .mise/locks/pypi-zensical/0.1.1/uv.lock
	modified:   mise.lock
	modified:   mise.toml
```
