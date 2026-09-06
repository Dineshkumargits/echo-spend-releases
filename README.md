# echo-spend-releases

Static update manifest for the [Echo Spend](https://play.google.com/store/apps/details?id=com.adkdinesh.echospend) Android app.

Echo Spend is serverless, so `version.json` is how the app answers "is there a
newer build?". It is fetched over HTTPS at most once a day; the app compares
`latestVersionCode` against its own `versionCode` and shows an in-app banner
when it is behind.

**This repo must stay public.** The fetch is unauthenticated — a private repo
returns 404, which the app reads as "check failed" and silently ignores.

## Fields

| Field | Meaning |
|---|---|
| `latestVersionCode` | The only field compared. Integer, matches `versionCode` in the app's `android/app/build.gradle`. |
| `latestVersionName` | Human-facing version, display only. Never parsed. |
| `minSupportedVersionCode` | Force-update gate. Builds below this get an undismissible banner. Keep at `0` normally. |
| `releaseNotes` | Up to 6 shown in the banner. |
| `url` | Where the Update button sends the user. Must be `https://`. |

## Updating

Bump `latestVersionCode` / `latestVersionName` / `releaseNotes` **only after a
Play rollout reaches 100%**. This manifest is a broadcast to every install and
cannot know whether a given user is in a staged-rollout bucket — publishing it
at 10% tells 90% of users to update to something they cannot get yet.

Forgetting to update it fails safe: no banner, nobody is misled.
