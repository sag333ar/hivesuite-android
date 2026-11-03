# Pending release notes

This file is the **single source of truth** for the next version's changelog
entry. `./deploy.sh` reads the `## Next` section below on every version
bump, copies each bullet into `src/config/changelog.ts`, and resets the
section so the cycle can start fresh.


## The rule

> Every Claude session that adds, changes, fixes, or polishes anything a
> user can see, hear, or feel **must append at least one bullet** to the
> `## Next` section before the session wraps up.

Why: the in-app drawer Changelog is the only place most users learn what
changed. Git commit subjects are too terse (`"fixed issues"`,
`"updated package"`) to make a good release note. This file lets the
session author write the line that will actually ship to users — in plain
English, with the real "why" intact.

We never publish a "no user-facing changes" / "maintenance release"
placeholder. If a deploy attempt finds an empty `## Next` section AND no
qualifying git commits across app + kit + server, `deploy.sh` aborts.

## Format

Each bullet is `- <type>: <text>`, where `<type>` is one of:

- **feature** — a new capability the user can use that didn't exist before.
- **improvement** — an existing capability now works better, faster, or in
  more places.
- **fix** — a bug that was hurting users is gone.

Free-form lines (without the `<type>:` prefix) are accepted and default to
`improvement`. Group related bullets — one paragraph each is fine — and
write for a non-technical reader who's just looking at the in-app
changelog. Mention the screen / button / flow so it's clear where to find
the change.

A tagline can be set with a single H3 inside `## Next`, e.g.
`### tagline: Share your game leaderboards`. It's optional.

## Next

<!-- HiveReactKit#15 -->
- fix: On mobile, the HP and RC delegation lists in a profile's Wallet now show who each delegation goes to (avatar and @username, tap to open their profile), and rows no longer run off the right edge.

<!-- hivesuite.app#186 -->
- feature: You can now log in with HiveSigner in the Android and iOS apps — HiveSigner opens in your browser and sends you straight back into HiveSuite, logged in.
<!-- hivesuite.app#187 -->
- fix: Posting a post or snap after the app has been open for a while no longer fails with a bare "Not logged in". HiveSuite now restores your wallet session (or switches back to your account) before signing, and if your HiveSigner or HiveAuth login has truly expired it tells you to log in again.
<!-- hivesuite.app#188 -->
- fix: Leaderboard: the admin reward-pool Send button no longer disappears after the page has been open a while; a failed reward-status check now shows a Retry button.
<!-- hivesuite.app#190 -->
- fix: Admin moderation page: the reported-users list now loads reliably for admins without needing a hard refresh.
<!-- hivesuite.app#196 -->
- improvement: Fairer leaderboard rewards — if one person runs several accounts (same device or browser, on-chain links, or repeatedly the same network), only one account is paid; the others are listed under Disqualified and flagged for admin review before payout.
<!-- hivesuite.app#197 -->
- improvement: Inbox now loads much further back (so older unreplied replies show up), adds a Load more button at the end of the list, and shows how far back history is loaded.
<!-- hivesuite.app#199 -->
- fix: Android app no longer crashes right after login. Push registration is skipped unless the build includes Firebase config (set VITE_PUSH_FIREBASE_CONFIGURED=true once google-services.json is added).
<!-- hivesuite.app#200 -->
- improvement: The multi-account vote picker now shows the post's current payout value and whether it's still open for votes (paid-out posts can't be voted on), shows "already voted X%" next to accounts that already voted (pick them again to change the vote), and pre-selects the account you're currently using. It also now appears on @sagarkothari88's posts and comments for everyone else.
<!-- hivesuite.app#201 -->
- improvement: 3Speak viewer rewards consent is now read and saved directly with 3Speak, so your choice is the same on every device and on 3speak.tv. A new "3Speak viewer rewards" card in Settings lets you turn it on or off (turning it off warns that 3Speak deletes your stored watch records). HiveSigner users get a link to 3Speak settings instead. Video posts now also include the 1% encoder.pay beneficiary 3Speak requires, and uploads use 3Speak's new embed2 host.
<!-- hivesuite.app#204 -->
- fix: The Android app no longer crashes right after login. Push notifications are set up properly with Firebase on Android and iOS, and the apps now send crash reports so we can fix problems faster.
<!-- HiveReactKit#11 -->
- improvement: If all of your logged-in accounts have already voted on a post, tapping vote now says "You've already voted on this content" instead of opening the account picker.
<!-- HiveReactKit#12 -->
- fix: Growth tab: the daily HP total in the chart tooltip no longer adds your account HP balance; it now shows only that day's HP change.
<!-- HiveReactKit#13 -->
- fix: Removed other apps' 3Speak keys that were built into the upload tools; HiveSuite uploads use only its own key, kept on our server.
<!-- hivesuite.app#206 (re-applies #187) -->
- fix: When a post, snap, vote or comment from an account logged in with a private posting key is rejected by Hive, you now see the real reason (for example "doesn't have enough Resource Credits (RC)") instead of "failed to sign and/or broadcast transaction".
- fix: Opening the user menu right after logging in with a private key no longer logs you out.

<!-- hivesuite.app#207 -->
- fix: Settings > 3Speak viewer rewards: if you logged in with a private posting key, you can now turn rewards on or off right in HiveSuite. Before, it said your login method couldn't sign the message.
<!-- HiveReactKit#14 + hivesuite.app#208 -->
- fix: Tapping upvote on a post, comment or reply that all your accounts have already voted on now shows the same "You've already voted on this content" message everywhere.
<!-- hivesuite.app#209 -->
- fix: The Download APK page no longer shows any APK download links on iPhone or iPad, including older releases.

## How the deploy script consumes this

1. `./deploy.sh` parses the `## Next` section.
2. Each `- type: text` line becomes a `{ type, text }` entry in
   `src/config/changelog.ts` for the new version, in the order written.
3. After a successful generation, `## Next` is wiped (replaced with a
   bare heading) and that single change is committed alongside the new
   `package.json` version + the `Updated version` push. Bullets are
   permanently preserved in the changelog itself.
