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

* /en-US/api/league/{id}: Must be logged in to access
* /en-US/api/season/{id}: Complete information for each season. Contains objects `proTeams`, `proPlayers`, and `proMatches`.
* ws://livestats-edge-proxy.dev.lolesports.com:9300/stats: Websocket for live updates for EU and NA games (fantasy games from LCS). When you first connect during a game it give send the rundown of points scored by kills/assists, etc by teams and players. As the socket remains open it will send updates, such as whenever someone hits a CS, or baron is killed. Example:
```
{
	"1000290003": "",
	"1000290002": {
		"teamStatsByMatchId": {
			"0": {
				"teamsById": {
					"100": {
						"teamId": 100,
						"color": "blue",
						"matchVictory": 1,
						"matchDefeat": 0,
						"baronsKilled": 0,
						"dragonsKilled": 0,
						"towersKilled": 5,
						"firstBlood": true
					},
					"200": {
						"teamId": 200,
						"color": "red",
						"matchVictory": 0,
						"matchDefeat": 1,
						"baronsKilled": 0,
						"dragonsKilled": 0,
						"towersKilled": 0,
						"firstBlood": false
					}
				},
				"matchStart": "",
				"gameId": "1000290002",
				"dateTime": "2014-07-26T20:36Z",
				"gameComplete": true
			}
		},
		"playerStatsByMatchId": {
			"0": {
				"playersByFantasyId": {
					"": {
						"fantasyId": "",
						"participantId": 10,
						"kills": 0,
						"deaths": 0,
						"assists": 0,
						"minionKills": 0,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "Test10"
					}
				},
				"gameId": "1000290002",
				"dateTime": "2014-07-26T20:36Z"
			}
		}
	},
	"1000290005": "",
	"1000290004": "",
	"1000290007": "",
	"1000290009": "",
	"1000280007": "",
	"1000280008": "",
	"1000280009": "",
	"1000280020": "",
	"1000280022": "",
	"1000280004": "",
	"1000280021": "",
	"1000280005": "",
	"1000280023": "",
	"1000290013": "",
	"1000290014": "",
	"1000290015": "",
	"1000290010": "",
	"1000290011": "",
	"1000290012": "",
	"1000280017": {
		"teamStatsByMatchId": {
			"2604": {
				"teamsById": {
					"100": {
						"teamId": 100,
						"color": "blue",
						"matchVictory": 1,
						"matchDefeat": 0,
						"baronsKilled": 2,
						"dragonsKilled": 1,
						"towersKilled": 11,
						"firstBlood": true
					},
					"200": {
						"teamId": 200,
						"color": "red",
						"matchVictory": 0,
						"matchDefeat": 1,
						"baronsKilled": 0,
						"dragonsKilled": 2,
						"towersKilled": 2,
						"firstBlood": false
					}
				},
				"matchStart": "2014-07-26T20:55:27.000Z",
				"gameId": "1000280017",
				"dateTime": "2014-07-26T21:00Z",
				"gameComplete": true
			}
		},
		"playerStatsByMatchId": {
			"2604": {
				"playersByFantasyId": {
					"11": {
						"fantasyId": "11",
						"participantId": 1,
						"kills": 3,
						"deaths": 1,
						"assists": 11,
						"minionKills": 245,
						"doubleKills": 1,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "TSM Dyrus"
					},
					"1102": {
						"fantasyId": "1102",
						"participantId": 2,
						"kills": 6,
						"deaths": 0,
						"assists": 6,
						"minionKills": 98,
						"doubleKills": 1,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "TSM Amazing"
					},
					"157": {
						"fantasyId": "157",
						"participantId": 3,
						"kills": 2,
						"deaths": 1,
						"assists": 11,
						"minionKills": 254,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "TSM Bjergsen"
					},
					"178": {
						"fantasyId": "178",
						"participantId": 4,
						"kills": 5,
						"deaths": 1,
						"assists": 8,
						"minionKills": 238,
						"doubleKills": 1,
						"tripleKills": 1,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "TSM WildTurtle"
					},
					"935": {
						"fantasyId": "935",
						"participantId": 5,
						"kills": 0,
						"deaths": 2,
						"assists": 10,
						"minionKills": 21,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "TSM Lustboy"
					},
					"1133": {
						"fantasyId": "1133",
						"participantId": 6,
						"kills": 0,
						"deaths": 3,
						"assists": 4,
						"minionKills": 169,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "EG InnoX"
					},
					"897": {
						"fantasyId": "897",
						"participantId": 7,
						"kills": 1,
						"deaths": 3,
						"assists": 1,
						"minionKills": 74,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "EG Helios"
					},
					"1132": {
						"fantasyId": "1132",
						"participantId": 8,
						"kills": 3,
						"deaths": 3,
						"assists": 2,
						"minionKills": 310,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "EG Pobelter"
					},
					"1582": {
						"fantasyId": "1582",
						"participantId": 9,
						"kills": 1,
						"deaths": 2,
						"assists": 3,
						"minionKills": 268,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "EG Altec"
					},
					"84": {
						"fantasyId": "84",
						"participantId": 10,
						"kills": 0,
						"deaths": 5,
						"assists": 1,
						"minionKills": 25,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "EG Krepo"
					}
				},
				"gameId": "1000280017",
				"dateTime": "2014-07-26T21:00Z"
			}
		}
	},
	"1000280016": "",
	"1000280015": "",
	"1000280014": "",
	"1000280019": "",
	"1000280018": "",
	"1000280012": {
		"teamStatsByMatchId": {
			"2606": {
				"teamsById": {
					"100": {
						"teamId": 100,
						"color": "blue",
						"matchVictory": 1,
						"matchDefeat": 0,
						"baronsKilled": 1,
						"dragonsKilled": 3,
						"towersKilled": 9,
						"firstBlood": false
					},
					"200": {
						"teamId": 200,
						"color": "red",
						"matchVictory": 0,
						"matchDefeat": 1,
						"baronsKilled": 0,
						"dragonsKilled": 1,
						"towersKilled": 4,
						"firstBlood": true
					}
				},
				"matchStart": "2014-07-26T19:30:49.000Z",
				"gameId": "1000280012",
				"dateTime": "2014-07-26T20:36Z",
				"gameComplete": true
			}
		},
		"playerStatsByMatchId": {
			"2606": {
				"playersByFantasyId": {
					"1301": {
						"fantasyId": "1301",
						"participantId": 1,
						"kills": 4,
						"deaths": 0,
						"assists": 4,
						"minionKills": 300,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "LMQ ackerman"
					},
					"1302": {
						"fantasyId": "1302",
						"participantId": 2,
						"kills": 2,
						"deaths": 2,
						"assists": 6,
						"minionKills": 116,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "LMQ NoName"
					},
					"1303": {
						"fantasyId": "1303",
						"participantId": 3,
						"kills": 0,
						"deaths": 2,
						"assists": 10,
						"minionKills": 359,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "LMQ XiaoWeiXiao"
					},
					"1304": {
						"fantasyId": "1304",
						"participantId": 4,
						"kills": 8,
						"deaths": 2,
						"assists": 3,
						"minionKills": 249,
						"doubleKills": 1,
						"tripleKills": 1,
						"quadraKills": 1,
						"pentaKills": 0,
						"summonerName": "LMQ Vasilii"
					},
					"1305": {
						"fantasyId": "1305",
						"participantId": 5,
						"kills": 1,
						"deaths": 1,
						"assists": 11,
						"minionKills": 45,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "LMQ Mor"
					},
					"1144": {
						"fantasyId": "1144",
						"participantId": 6,
						"kills": 3,
						"deaths": 1,
						"assists": 1,
						"minionKills": 302,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "COL Westrice"
					},
					"1150": {
						"fantasyId": "1150",
						"participantId": 7,
						"kills": 2,
						"deaths": 4,
						"assists": 3,
						"minionKills": 111,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "COL Kez"
					},
					"194": {
						"fantasyId": "194",
						"participantId": 8,
						"kills": 1,
						"deaths": 3,
						"assists": 4,
						"minionKills": 281,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "COL pr0lly"
					},
					"1146": {
						"fantasyId": "1146",
						"participantId": 9,
						"kills": 1,
						"deaths": 3,
						"assists": 2,
						"minionKills": 295,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "COL ROBERTxLEE"
					},
					"1147": {
						"fantasyId": "1147",
						"participantId": 10,
						"kills": 0,
						"deaths": 4,
						"assists": 4,
						"minionKills": 16,
						"doubleKills": 0,
						"tripleKills": 0,
						"quadraKills": 0,
						"pentaKills": 0,
						"summonerName": "COL Bubbadub"
					}
				},
				"gameId": "1000280012",
				"dateTime": "2014-07-26T20:36Z"
			}
		}
	},
	"1000280011": ""
}
```




