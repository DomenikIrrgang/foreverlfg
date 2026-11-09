# Changelog

## Unreleased

### Fixed

- Reading chat for groups costs far less: the parser builds its word lists when the library starts (`Parser.Prepare`, which the group finder part calls after reading the tool's activity names) instead of on the first channel line, and for each line looks only for the phrases that start with one of its words, without building a new string per phrase. What a line is read as doesn't change.

## 0.1.1 (2026-11-07)

### Fixed

- Group finder results the game hands over partly secret no longer stop the list with `bad argument #1 to 'ipairs' (table expected, got secret)`: a secret field (the activities, the member count, the comment, the leader's name, roles) is read as missing, and the rest of the result still shows.

## 0.1.0 (2026-10-27)

The first version on the wowlua registry, moved out of SuuUI, where it was the Group Finder's library.

### New

- Groups and players looking for groups from the game's group finder and the public chat channels, merged per leader into one list (`Entries`, `Entry`) with events when an entry is added, updated, refreshed or removed.
- What concerns you: matches by role, activity, quest, zone, boss, player and friend (`Match`, `MatchReasons`), watched activities, and the leaders you whispered.
- Chat in English, Korean and Chinese, with the parser and its language modules usable on their own.
- Your listing (`Listing`, `Delist`), whispers and invites within what Forever allows.
