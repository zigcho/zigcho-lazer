# zigcho!lazer

this is the client side of zigcho. it lives here now instead of being buried under the server repo.

i keep one reviewable patch against an exact official osu! commit instead of dumping their entire source tree into ours. the upstream pin is in `upstream-commit.txt`; `apply-endpoints.sh` and `apply-endpoints.ps1` refuse to touch a different checkout.

## what the patch changes

- gives the client its own name, storage directory, application ids and IPC identity
- points account, API, chat, multiplayer, spectator, BSS, wiki, news and media requests at the right `kai.ovh` services
- keeps the official updater away from the portable zigcho build
- adds the response and profile work needed for scores, leaderboards, medals, maps, replays, rooms and ranked play
- keeps production endpoints HTTPS-only while leaving one explicit loopback Debug lane

the server contract still lives in [`zigcho/zigcho`](https://github.com/zigcho/zigcho). changing a client model here does not magically make the server route correct, and a route returning 200 does not prove the client accepted or rendered it.

## how a build is made

1. GitHub checks out this repo and the exact `ppy/osu` commit beside it.
2. the patch is checked before it is applied. a stale or half-applied patch stops the run.
3. the focused upstream client tests exercise the Zigcho-specific screens and response models.
4. Windows, macOS, Linux, Android and iOS build on their own hosted runners.
5. every package records the client commit and upstream osu! commit, then gets a SHA-256 sidecar and format checks before it can become a release.

## builds

release builds happen on GitHub runners, not on somebody's laptop.

```sh
gh workflow run lazer-clients.yml
```

the workflow applies the patch from a clean checkout, runs the focused client tests once, then builds and verifies:

- Windows x64
- macOS arm64
- Linux x64
- Android arm64
- iOS arm64

every artifact has a SHA-256 sidecar. desktop builds are portable folders with no installer or updater. the Android APK uses runner signing; the iOS IPA is intentionally unsigned and needs signing when it is installed.

`run-local-debug.sh` is only for the loopback QA lane. production builds always use HTTPS on the normal `kai.ovh` hosts.

## layout

- `zigcho-client.patch` is the complete client delta
- `upstream-commit.txt` pins the only accepted osu! source revision
- `client-version.txt` is the portable build version
- `apply-endpoints.*` owns safe patch application
- `package-*` and `verify-*` own release layout, metadata and checksums
- `.github/workflows/` builds and publishes the packages

## licence

Zigcho's work in this repo uses the [Zigcho Public Use License](LICENSE). public use, modification and free redistribution are allowed with credit and a link back. do not sell it, charge for access or pretend the work is entirely yours.

the official osu! code remains Copyright ppy Pty Ltd under its MIT licence. that licence covers code, not the osu!/ppy names or game resources. the exact boundary is in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
