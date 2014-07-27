## na.lolesports.com

* [/api/league/{id}.json](http://na.lolesports.com/api/league/1.json). Notabale returned fields:
    * `leagueImage`: URL link to the league image
    * `defaultTournamentId`: Integer id of the tournament. Can use this to get tournament information at na.lolesports.com/api/tournament/{id}.json
    * `internationalLiveStream`: Contains an array of international stream objects:
        * `language`: Possibly the language the stream is shoutcasted in
        * `display_language`: Possibly the language the client is set to
        * `streams`: Array of streams, with a `title`, `url`, and a `randomize` field. I'm not sure what this field is used for.
    * `shortName`: Short name of the league. Interestingly, there is no long name field.
* [/api/match/{id}.json](http://na.lolesports.com/api/match/{id}.json). Notable returned fields:
    * `tournament`: Object with the tournament id (links to `/api/tournament/{id}.json`), tournament full name, and round of the tournament.
    * `dateTime`: Scheduled match time in ISO 8601 format
    * `winnerId`, `maxGames`, `isLive`, `isFinished`
    * `contestants`:
        * `blue`, `red`: have the same format:
            * `id`: Team id (links to `/api/team/{id}.json`)
            * `name`, `logoURL`, `acronym`
            * `wins`, `losses`: Total wins and losses at the current point in time (not when the game was played)
    * `liveStreams`: Does the game have a live-stream up?
    * `polldaddyId`: I assume this is the poll that is updated when people tweet or vote on a winning team
    * `games`: For LCS games, just has the field `game0`:
        * `id`: Links to `/api/game/{id}.json`
        * `winnerId`, `hasVod` (0 or 1)
    * `name`: Ex: "Curse vs Cloud9 "
* [/api/tournament/{id}.json](http://na.lolesports.com/api/tournament/104.json). Notable returned fields:
    * `namePublic`, `name`
    * `contestants`: Contains fields such as `contestant1`, `contestant2`, etc:
        * `id` (links to `/api/team/{id}.json`), `name`, `acronym`
    * `isFinished`, `dateBegin`, `dateEnd`
    * `noVods`: 0 if Vods are available, 1 if not
    * `season`: Eg 2014 for current NA Summer Split
* [/api/team/{id}.json](http://na.lolesports.com/api/team/1.json). Notable return fields:
    * `name`, `bio`, `logoUrl`, `teamPhotoUrl`, `acronym`
    * `profileUrl`: Link to na.lolesports.com/node/{id}
    * `noPlayers`: ?
    * `roster`: Contains fields such as `players0`, `players1`, etc:
        *  `playerId`, `name`, `isStarter` (0 or 1)
        *  `role`: One of "Top Lane, "AD Carry", "Mid Lane", "Support", "Jungler"
* [/api/game/{id}.json](http://na.lolesports.com/api/game/3038.json). Notable return fields:
    * `dateTime`, `winnerId`, `maxGames`, `gameNumber`, `gameLength` (in seconds), `matchId`
    * `vods`: Contains an object `vod`, which has `type` (youtube normally), `URL`, and `embedCode`
    * `contestants`: Contains `blue` and `red` which have `id`, `name`, and `logoURL`
    * `players`: Contains `player0`, `player1`, etc:
        * `id`, `teamId`, `name`, `photoURL`, `kda`, `kills`, `deaths`, `assists`, `endLevel`, `minionsKilled`, `totalGold`, `championId`
        * `spell{slot}{spellId}`: Ex: spell012: Spell 12 for slot 0 (normally D hotkey)
        * `items{slot}{itemId}`: Ex: items 03153: Item 3153 for slot 0 (indexed at 0)

## fantasy.na.lolesports.com






