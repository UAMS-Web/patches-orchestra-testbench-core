<!--
  Synced from UAMS-Web/uams-claude-skills: shared/.claude/skills/patch-package/templates/upstream-watches-README.md
  Edit it there, not here; the next sync overwrites this copy.
  Delivered to this repository through the profile(s): patches.
-->
# Upstream watches

<!-- cspell:ignore Packagist -->

One file per upstream issue or pull request this repository is waiting on. The `watch-upstream` workflow (`.github/workflows/watch-upstream.yml`) reads every `*.json` file here once a day and comments on the row's tracking issue when something moves. This README is also what keeps the directory tracked while it holds no rows, which is a valid state: the workflow then has nothing to do.

## Row format

Name the file `<tracker>-<owner>-<repo>-<number>.json`, for example `412-laravel-framework-58123.json` for tracker `412` watching `laravel/framework#58123` (`/` in the repository becomes `-`). The loader rejects a file whose name does not match its contents, so the name is always unique and says what it watches.

```json
{ "tracker": 412, "repo": "laravel/framework", "kind": "pr", "number": 58123, "composer": "laravel/framework", "released": true }
```

| Key | Required | Meaning |
| --- | --- | --- |
| `tracker` | yes | Issue number in this repository that receives the comments. Closing it turns the watch off. |
| `repo` | yes | The upstream repository, `owner/name`. |
| `kind` | yes | `issue` or `pr`. |
| `number` | yes | The upstream issue or pull request number. |
| `composer` | no | A `vendor/package` name. Adds a signal for a Packagist release newer than the version in `composer.lock`. |
| `released` | no | `true` on a `pr` row only. Adds a signal for the first release on the pull request's base-branch line that contains the merge commit. |

Unknown keys, a missing key and a wrong type fail the run with the file named, so a malformed row is never silently skipped.

## Adding and retiring a row

- **Add** a row in the same pull request that adds the local patch it exists for, so the patch and its removal trigger land together.
- **Retire** a row in the pull request that drops the patch: delete the file and close the tracking issue. Closing the tracker alone stops the comments; deleting the file stops the workflow reading it.
- Two rows under one tracker share an off switch, so give threads separate trackers whenever either can outlive the other.

## Notes on current rows

- `1-orchestral-testbench-core-443.json`: tracker `1` watches `orchestral/testbench-core#443`, an open issue reporting that parallel workers share `bootstrap/cache/services.php` and Windows runs fail with "Access is denied". When it moves, read the thread on #1 for a fix or a release carrying one. The row has no `composer` key: this repository pins `orchestra/testbench-core` in `composer.json` and carries no `composer.lock`, so the Packagist signal has no locked version to compare against. Two things to follow up when the upstream thread records a pull request: #1 covers three patches but names only this one upstream item, and a `pr` row for the fix takes `released: true`.

A row belongs here when an open ticket in this repository waits on an upstream issue or pull request, and the ticket is its tracker. Add one line per row saying what the upstream thread is and what to do when it moves.
