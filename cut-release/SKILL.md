---
name: "cut-release"
description: "Cut a release of Ale's GitHub projects: list commits since the last tag grouped for release notes, then sign and push an annotated tag carrying the final notes. Use when asked to prepare or ship a release."
---

# Cut a release

For Ale's open source repos (italypaleale/traefik-forward-auth, francis, revaulter, and others that follow the same pattern). Only `git` is required; `gh` is optional.

A release is a signed, annotated, `v`-prefixed Git tag whose message is the release notes. Pushing the tag triggers the repo's release workflow (`on: push: tags: v*`), which builds artifacts and creates the GitHub release using the tag message as the notes. Tags with a pre-release suffix (`v1.2.0-rc.1`) are published as pre-releases. Published releases are immutable, so a wrong tag cannot be quietly redone.

## Rules

- Ale writes the final release notes. Your job before that is gathering and grouping commits, not polishing prose.
- Do not create the tag until Ale has given you the final notes and the release type (patch, minor, major, or pre-release).
- Always an annotated, signed tag (`-s`). Never fall back to unsigned or lightweight tags.
- Never force-push, delete, or move a tag. Never run `git push --tags` or push branches.
- Do not edit, commit, or create files in the working tree. Do not open PRs, issues or comments. If the repo seems to need a version bump or changelog edit, tell Ale instead.
- The tag message is published verbatim. Add nothing to it: no title, no footer, no attribution lines.

## 1. Preflight

```sh
git fetch origin --tags --prune
BR=$(git symbolic-ref --short refs/remotes/origin/HEAD | sed 's#^origin/##')   # normally main
git ls-remote --symref origin HEAD   # confirm $BR is still the remote's default branch
git symbolic-ref --short HEAD   # must be $BR; a detached HEAD is not acceptable
git status --porcelain          # must be empty
git rev-parse HEAD "origin/$BR"  # must be identical, and HEAD must be on $BR
SHA=$(git rev-parse HEAD)       # pin the commit used for the review and tag
git remote get-url origin      # identify OWNER/REPO and confirm the release destination
```

Stop on a failed fetch. If `origin/HEAD` is missing or stale, discover the default branch with `git ls-remote --symref origin HEAD`; do not assume `main`. If the tree is dirty, HEAD is not the default branch, or it differs from origin, stop and tell Ale. Do not pull, reset or stash on your own.

Read the release workflow in `.github/workflows`, including any reusable workflow it calls. Confirm that `v*` tags trigger it, the tag message supplies the release notes, and pre-release suffixes are handled as expected. If the workflow is missing or behaves differently, explain the mismatch and ask how to proceed. Do not run `gh release create` yourself.

Optionally check CI on HEAD (`gh run list --branch "$BR" --limit 3`) and report it. Do not block on it.

## 2. Find the last release

Pre-releases must sort below their final release, which needs `versionsort.suffix=-`. Without it `v0.2.0-rc.1` sorts above `v0.2.0`.

```sh
git -c versionsort.suffix=- tag --merged "$SHA" --list 'v[0-9]*' --sort=-v:refname | head -10
git -c versionsort.suffix=- tag --merged "$SHA" --list 'v[0-9]*' --sort=-v:refname | grep -E '^v[0-9]+\.[0-9]+\.[0-9]+$' | head -1   # last stable
```

Report the last tag and the last stable tag (they differ after an rc). Tags look like `v1.2.3` and `v1.3.0-rc.1`. If there are no release tags, this is the first release: review all commits reachable from `$SHA` and let Ale choose the initial version. If only pre-releases exist, a first stable release also covers the full history.

## 3. Collect commits

Range base:

- Stable release: the last **stable** tag, even if rc tags came after it. The final notes cover the whole cycle (e.g. v0.2.0 after two rcs summarizes everything since v0.1.2).
- New pre-release: the last tag of any kind.
- If it is unclear which applies, show both commit counts and ask.

```sh
git log --no-merges --reverse --format='%h%x09%ae%x09%s' "$BASE..$SHA"
# With no applicable base tag, pass "$SHA" instead of "$BASE..$SHA".
git show -s --format=%B <sha>   # when a subject is vague, e.g. "Various fixes:"
```

For repos that merge PRs with `Merge pull request` commits, use `git log --first-parent --reverse --format='%h%x09%ae%x09%B%x00' "$BASE..$SHA"` instead, keeping merge commits so their bodies supply PR titles. Include direct commits, but do not also list the branch commits brought in by a merge. For squash or rebase histories, use the non-merge log above.

Do not discard changes solely because a bot authored them: dependency updates may contain security fixes. Group routine bot dependency updates under Changes, promote known vulnerability fixes to Security, and skip only automation commits with no release-relevant change. Subjects are not strictly Conventional Commits (`fix:` and `chore:` appear next to plain sentences), so classify by what the change does. Strip the prefix, keep the `(#NN)` PR number.

Flag human-authored commits by authors other than Ale (compare emails; the maintainer shows up under several names) as possible external contributions so Ale can thank them. For PR merges, inspect original authors or PR metadata rather than assuming the merge author wrote the changes. Past notes do this, e.g. `(#46 - thanks @handle!)`. Resolve handles with `gh` (`gh api repos/OWNER/REPO/commits/SHA --jq .author.login`) or leave them out. Never guess a handle.

## 4. Group and present

Group in this order and omit empty groups:

1. **Security**: vulnerability fixes, auth or crypto flaws, leaked secrets, dependency bumps that fix a known vuln. Keep advisory IDs or links (GHSA/CVE) when the commit or PR mentions them. Never invent one. If a fix might be security-relevant, put it here with a `?` instead of burying it under Fixes.
2. **New features**
3. **Fixes**
4. **Changes**: everything else. Collapse routine upkeep into one line each ("Update third-party dependencies", "Update Go compiler version", docs, CI) instead of one line per bump.

Before presenting, read the latest three releases for the intro style and any standing banner:

```sh
for t in $(git -c versionsort.suffix=- tag --merged "$SHA" --list 'v[0-9]*' --sort=-v:refname | head -3); do
  git tag -l --format='%(refname:short)%0a%(contents)' "$t" | sed '/^-----BEGIN .* SIGNATURE-----/,$d'
done
```

Recent notes typically have a one-line intro (small releases just say "This is a maintenance release:" followed by a short list). Always use exactly the `## Security`, `## New features`, `## Fixes`, `## Changes` headings and `- ` bullets, even if the repo's recent releases use different conventions. Some repos repeat a standing banner (a blockquote about a major version or migration) in every release. If recent releases carry one, include it by default, copied from the latest release and placed where it appears there, and mention that you did so. Ale may tell you to drop or change it.

Print the drafted release notes in the chat as Markdown, inside a fenced ```` ```markdown ```` code block so Ale can copy the raw text. Use the standard headings and `- ` bullets, one bullet per item with its short hash or PR number. Keep the block to the notes themselves; put commentary outside it.

Show Ale: the last tag, `$SHA` shortened, the range base and commit count, the drafted notes (as the Markdown block above), anything you were unsure how to classify, and how many commits you skipped and why. If the selected range is empty, report that before proposing another release. Then ask for the final notes and the release type. List what each type would produce, e.g. patch → v1.2.4, minor → v1.3.0, major → v2.0.0, pre-release → v1.3.0-rc.1. Compute stable bumps from the last stable version. Ale decides the bump. You may point out changes that look breaking (removed flags, config or API changes), but do not pick for him.

When promoting a pre-release, also offer its exact stable version (e.g. `v1.3.0-rc.2` → `v1.3.0`), with notes covering changes since the last stable release. For a new pre-release series, Ale chooses the target version. Use the identifier the repo already uses, and ask only if there is none. Increment the highest existing number for that target and identifier, considering local and remote tags even outside the merged history; otherwise start at `-rc.1`.

If Ale chooses a release type that changes the range base, regenerate the grouped list for that range before treating the notes as final.

## 5. Create the signed tag

Only after Ale has given the final notes and release type.

Fetch again and repeat the branch, clean-tree, and remote-HEAD checks. Both HEAD and `origin/$BR` must still equal the recorded `$SHA`. If anything changed during review, stop and refresh the commit list and notes with Ale; do not silently update `$SHA`.

```sh
TAG=v1.2.4   # Ale's chosen version; keep the recorded $SHA
git check-ref-format "refs/tags/$TAG"
NOTES="$(git rev-parse --git-dir)/RELEASE_NOTES_$TAG.md"   # inside .git, so the tree stays clean
# write Ale's notes into $NOTES exactly as given, with a final newline
git show-ref --verify --quiet "refs/tags/$TAG"            # exit 1 means absent locally
git ls-remote --exit-code --tags origin "refs/tags/$TAG"   # exit 2 means absent remotely
git tag -s "$TAG" --cleanup=verbatim -F "$NOTES" "$SHA"
```

Validate the chosen version against the repo's `v`-prefixed version format. Only create the tag when it is absent both locally and remotely. Treat authentication, network, and other lookup errors as failures, not evidence that the tag is available. If a tag already exists, inspect it and report it; never overwrite it. If it is the verified tag from an interrupted run, resume verification instead of recreating it.

`--cleanup=verbatim` is required. By default git strips lines starting with `#` from the message, which deletes every `## Fixes` heading.

Signing uses Ale's own git config; the existing tags carry SSH signatures. Do not pass `-u` or change signing config. Tell Ale before running the command, because his key may prompt him (passphrase, hardware key touch). If signing fails or hangs because it needs interactive input, do not retry in a loop and do not drop the signature. Give Ale the exact command and the notes file path to run himself, and wait until he says it is done.

Verify before going further:

```sh
git cat-file -t "$TAG"                                    # tag (annotated)
git rev-parse "$TAG^{commit}"                            # must equal the recorded $SHA
git cat-file -p "$TAG" | grep -c 'BEGIN .* SIGNATURE'    # 1
diff <(git tag -l --format='%(contents)' "$TAG" | sed '/^-----BEGIN .* SIGNATURE-----/,$d') "$NOTES"   # no differences
git tag -v "$TAG"   # only if allowed signers are configured; otherwise say it was skipped
TAG_OBJECT=$(git rev-parse "refs/tags/$TAG")   # record the verified tag object for push
```

Stop on a message or target mismatch, missing signature, or failed signature verification. If verification was skipped because the trust configuration is unavailable, distinguish signature presence from verified identity.

Summarize for Ale: tag name, target commit (short SHA and subject), first lines of the message, and whether the signature is present.

## 6. Push

Ask Ale for an explicit yes before pushing, naming the tag and commit. Pushing starts the release workflow, which publishes immediately.

After approval, fetch again and repeat the branch, clean-tree, and pinned-commit checks. Verify that the local tag object is still the recorded `$TAG_OBJECT`. If the remote tag already exists, compare its tag object ID with the recorded one: an identical tag is already pushed; a different tag is a conflict. A lookup failure is not permission to push.

```sh
test "$(git rev-parse "refs/tags/$TAG")" = "$TAG_OBJECT"   # stop if the reviewed tag changed
git -c push.followTags=false push origin "refs/tags/$TAG:refs/tags/$TAG"
git ls-remote --exit-code --tags origin "refs/tags/$TAG"   # object ID must equal $TAG_OBJECT
```

Disabling `push.followTags` prevents a local setting from publishing other annotated tags. If push reports a network error, check the remote object ID before retrying; the server may already have accepted it. Never recreate the tag on a retry.

Delete `$NOTES` only after confirming the matching remote tag. Give Ale the release URL (`https://github.com/OWNER/REPO/releases/tag/$TAG`) and, if `gh` is available, the run for this tag and commit (`gh run list --commit "$SHA" --limit 10`). The URL is the expected location until the workflow publishes the release; do not claim publishing succeeded solely because the tag landed. Do not babysit CI unless asked.

If something is wrong after the push, do not delete or move the tag. Tell Ale and let him decide, which usually means a new patch release.
