# foreverlfg

Groups and players looking for groups on WoW Forever, from the game's group finder (the tool) and the public chat channels, as one list. A [wowlua](https://wowlua.dev) library with no UI of its own, written for [SuuUI](https://github.com/DomenikIrrgang/suuui)'s Group Finder.

It reads the tool's search results and the LookingForGroup, General, Say and Yell lines (English, Korean and Chinese posts), merges them per leader, keeps them fresh, and tells you which ones concern you: your role, the activity you watch, your quests, your zone, a friend.

## Use it

```console
$ wowlua add foreverlfg
```

```wowlua
import lfg from "foreverlfg"

lfg.On("Match", function(entry, reasons)
    print(entry.activities[1], #reasons)
end)
lfg.Start()
```

It does nothing until `Start()`. It's built for the `camelot` flavor (WoW Forever).

## API

| | |
|---|---|
| `lfg.Start()` / `Stop()` | Starts reading the tool and chat, joins LookingForGroup (and keeps it off the chat windows); stops |
| `lfg.Configure({ channels, joinLookingForGroup, hideLookingForGroup, chatExpiry, staleAfter, expiringSoon, minConfidence, extraLanguages })` | Its settings (`lfg.config` holds the defaults) |
| `lfg.On(event, fn)` / `Off(event, fn)` | Events: `EntryAdded(entry)`, `EntryUpdated(entry)`, `EntryRefreshed(entry)` (posted again, unchanged), `EntryRemoved(entry, reason)`, `Match(entry, reasons)`, `Contacted(entry, "full" \| "gone" \| "reposted")`, `ToolSearched(category, count)`, `ToolSearchFailed(category)`, `ToolStale(category, age)`, `ListingChanged(listing)`, `ListingExpiring(secondsLeft)`, `Delisted()` |
| `lfg.Entries(filter)` / `Entry(key)` | The merged list (newest first); `filter = { intent, kinds, activities, sources, minConfidence, search, notYou }`. An entry: `key` (the leader's whole name), `intent` ("group" \| "player"), `activities`, `quests`, `roles = { needed, offered }`, `classes`, `need`, `members`, `level`, `leader`, `text`, `confidence`, `sources = { tool, chat }` |
| `lfg.Refresh(category)` | Searches the tool. Only from a click or key press (the game blocks it otherwise); 2 Dungeons, 116 Quests & Zones, 120 Custom |
| `lfg.Listing()` / `Delist()` | Your listing (activities, roles, comment, play style, time left); delisting from a click. Making or changing a listing is blocked for addon code: use the game's own listing panel |
| `lfg.Whisper(entry)` / `CanInvite(entry)` / `Invite(entry)` | An empty whisper to the leader; inviting a single player looking (the tool's rule; there's no sign-up on Forever) |
| `lfg.Interest()` / `Watch(key, on)` / `Watches()` / `MatchReasons(entry)` | What you're after (listing, roles, quests, level, zone, watches); an activity to hear about; why an entry concerns you: "role", "activity", "player", "quest", "zone", "boss", "friend" |
| `lfg.ContactedAt(key)` / `Social(name)` | When you whispered that leader (`lfg.Now()` time), or nil; "friend", "guild" or nil for a player's name |
| `lfg.Catalogue` | `Get(key)`, `All(kind)`, `Expand(key)`, `ForTool(activityID)`: the activities ("dungeon:rfk", "zone:westfall", "raid:mc", "boss:azuregos", "tool:<id>") |
| `lfg.Parser.Parse(text, { languages })` / `lfg.Lang.Register(code, def)` / `lfg.Languages()` | The chat parser on its own; a language module (see `src/Lang/enUS.wowlua`); the languages in use |
| `lfg.IsHiddenChannel(name)` | Channels it reads but keeps off the chat windows (LookingForGroup) |

The types (`Entry`, `Listing`, `Interest`, `MatchReason`, `LfgEvent`, …) are exported too: `import type Entry, MatchReason from "foreverlfg"`.

What the game allows on Forever, as probed in game, and how the library is built: [docs/design.md](docs/design.md).

## Develop

```console
$ wowlua check
$ wowlua test
$ lua tools/lfg_parse.lua -e "LFM tank RFK" "lfg sfk healer"
```

`tools/lfg_parse.lua` replays chat through the parser outside the game, also the lines SuuUI's `/suu probe lfg record` kept.

Publishing a version: set `package.version` in `wowlua.json`, add its section to `CHANGELOG.md`, then `wowlua publish --dry-run` and `wowlua publish` (with a token from [registry.wowlua.dev](https://registry.wowlua.dev), saved by `wowlua login registry`).

## License

MIT
