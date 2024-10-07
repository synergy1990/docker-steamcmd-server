# SteamCMD in Docker optimized for Unraid
This Docker will download and install SteamCMD and the according game that is pulled via specifying the Tag.

**Please see the different Tags/Branches which games are available.**

## Example Env params for Counter-Strike 1.6
| Name | Value | Example |
| --- | --- | --- |
| STEAMCMD_DIR | Folder for SteamCMD | /serverdata/steamcmd |
| SERVER_DIR | Folder for gamefile | /serverdata/serverfiles |
| GAME_ID | For all the OLD games like CS 1.6, HLDM, DoD, TFC, you need to set it to 90 | 90 |
| GAME_NAME | HLDS gamename | cstrike |
| GAME_PARAMS | Values to start the server | -secure +maxplayers 32 +map de_dust2 +rcon_password myrcon |
| UID | User Identifier | 99 |
| GID | Group Identifier | 100 |
| GAME_PORT | Port the server will be running on | 27015 |
| VALIDATE | Validates the game data | blank |
| USERNAME | Leave blank for anonymous login | blank |
| PASSWRD | Leave blank for anonymous login | blank |

## The short names for the old names are:
| Name | Short |
| --- | --- |
| Half-Life | valve |
| Counter-Strike | cstrike |
| CS Condition Zero | czero |
| Deathmatch Classic | dmc |
| Opposing Force | gearbox |
| Ricochet | ricochet |
| Team Fortress Classic | tfc |
| 007 Nightfire | nightfire |
| Gunman Chronicles | rewolf |
| Day of Defeat | dod |

# Notice:
- Half-Life Deathmatch and Counter-Strike 1.6 work fine - couldn't get Deathmatch Classic to run: Could be that you have cannot run this server anonymously, so could be that you have to OWN the game and login to SteamCMD.
- You can see the Counter-Strike: Source example at ich777's project.

## Run example for Counter-Strike 1.6
```
docker run --name CounterStrike -d \
	-p 27015:27015 -p 27015:27015/udp \
	--env 'GAME_ID=90' \
	--env 'GAME_NAME=cstrike' \
	--env 'GAME_PORT=27015' \
	--env 'GAME_PARAMS=-secure +maxplayers 32 +map de_dust2 +rcon_password myrcon' \
	--env 'UID=99' \
	--env 'GID=100' \
	--volume /path/to/steamcmd:/serverdata/steamcmd \
	--volume /path/to/cstrikesource:/serverdata/serverfiles \
	ich777/steamcmd:latest
```

## Docker-Compose for Counter-Strike 1.6:
```
services:
  cstrike_server:
    image: ich777/steamcmd:latest
    container_name: cstrike_server
    ports:
      - 27015:27015
      - 27015:27015/udp
    environment:
      GAME_ID: 90
      GAME_NAME: cstrike
      GAME_PORT: 27015
      GAME_PARAMS: -secure +maxplayers 32 +map de_aztec +rcon_password myrcon
      UID: 99
      GID: 100
    volumes:
      - steamcmd:/serverdata/steamcmd
      - serverfiles:/serverdata/serverfiles
    restart: unless-stopped

volumes:
  steamcmd:
  serverfiles:
```

This Docker was mainly edited to get old Goldsrc-engine games to run as well as Source-engine games, since they need a different executable (<b>hlds_run</b> instead of srcds_run).

This Docker is forked from ich777, thank you for this wonderfull Docker.
