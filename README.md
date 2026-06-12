# RealRTCW Coop AMP Template

Private AMP Generic Module template for a RealRTCW cooperative dedicated server.

This is designed to be loaded from your own GitHub repository rather than submitted to CubeCoders/AMPTemplates. Keep the template files at the repository root so AMP can resolve the `@IncludeJson[...]` references.

## Files

- `realrtcw-coop.kvp` - AMP root template.
- `realrtcw-coopconfig.json` - panel settings manifest.
- `realrtcw-coopmetaconfig.json` - writes `coop/coopmain/server.cfg`.
- `realrtcw-coopports.json` - default UDP game/RCON port.
- `realrtcw-coopupdates.json` - SteamCMD app download and Linux Wine init.

## Loading In AMP From Your GitHub Repo

1. Create a new GitHub repository, for example `AMPTemplates-RealRTCW-Coop`.
2. Put these files in the repository root and push to your default branch.
3. In AMP, go to the template/source management area for Generic templates and add your GitHub repository URL as a custom template source.
4. Refresh templates, create a new Generic instance, and select `RealRTCW Coop`.
5. Run Update, then Start.

If your AMP build asks for a raw template URL instead of a repository URL, use the raw URL to `realrtcw-coop.kvp` on your branch.

## Notes And Limits

- RealRTCW is Steam app `1379630`, and the Steam depot includes `coop/RTCWCoopDED.x64.exe` plus `coop/coopmain`.
- Steam describes RealRTCW as a free mod, but it requires the original Return to Castle Wolfenstein install to play. SteamCMD behavior can vary depending on account ownership and package visibility. If anonymous SteamCMD fails, disable anonymous update behavior in the template or let AMP prompt for a Steam account that owns the required content.
- Linux hosts run the Windows dedicated server through Wine. The template uses `cubecoders/ampbase:wine-stable`, matching the style used by other Greelan-authored Wine-backed templates.
- RTCWCoop registers `coopmap` and `coopdevmap`, so the startup map command uses those instead of stock `map`/`devmap`.
- RTCWCoop game types are `0` Battle, `1` Speedrun, `2` Classes, and `3` Cooperative. The source default is `2` Classes, so the AMP default follows that.
- Player join parsing uses `ClientUserinfoChanged` so AMP can capture the username. Disconnect logs usually only include the client slot, so the leave regex captures `sessionid` rather than username.
- The default generated config is `coop/coopmain/server.cfg`. Advanced one-off cvars can still be edited directly in AMP's file manager after first start.

## Verified Reference Points

- RealRTCW Steam app ID: `1379630`.
- RealRTCW depot path for the dedicated coop server: `coop/RTCWCoopDED.x64.exe`.
- RTCWCoop source cvars used for the settings panel include `g_gameskill`, `g_spawnpoints`, `g_sharedlives`, `g_limbotime`, `g_reinforce`, `g_fastres`, `g_antilag`, `g_voteFlags`, and rotating MOTD cvars.
- RTCWCoop/iortcw log strings used for regex: `ClientUserinfoChanged`, `ClientDisconnect`, and `say`/`sayteam`/`say_limbo`.
