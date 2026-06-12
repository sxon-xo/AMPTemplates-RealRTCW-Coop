# RealRTCW Coop AMP Template

Private AMP Generic Module template for a RealRTCW cooperative dedicated server.

This is designed to be loaded from your own GitHub repository rather than submitted to CubeCoders/AMPTemplates. Keep the template files at the repository root so AMP can resolve the `@IncludeJson[...]` references.

## Files

- `realrtcw-coop.kvp` - AMP root template.
- `realrtcw-coopconfig.json` - panel settings manifest.
- `realrtcw-coopmetaconfig.json` - writes `coop/coopmain/server.cfg`.
- `realrtcw-coopports.json` - default UDP game/RCON port.
- `realrtcw-coopupdates.json` - SteamCMD bootstrap preload, RTCW base game download, RealRTCW/RTCWCoop download, and Linux Wine init.

## Loading In AMP From Your GitHub Repo

1. Create a new GitHub repository, for example `AMPTemplates-RealRTCW-Coop`.
2. Put these files in the repository root and push to your default branch.
3. In AMP, go to the template/source management area for Generic templates and add your GitHub repository URL as a custom template source.
4. Refresh templates, create a new Generic instance, and select `RealRTCW Coop`.
5. Run Update, then Start.

If your AMP build asks for a raw template URL instead of a repository URL, use the raw URL to `realrtcw-coop.kvp` on your branch.

## Notes And Limits

- Return to Castle Wolfenstein is Steam app `9010`. RealRTCW is Steam app `1379630`.
- RealRTCW is a free modification for Return to Castle Wolfenstein, so the update sequence installs the paid RTCW base game first and then overlays RealRTCW/RTCWCoop into the RTCW `9010` directory.
- SteamCMD must use a Steam account that owns Return to Castle Wolfenstein. The template prompts for Steam credentials rather than using anonymous login.
- Startup points `fs_basepath` at the `9010` install directory for the RTCW base assets and `fs_homepath` at `9010/coop` so the generated `coop/coopmain/server.cfg` is used.
- Linux hosts should run the Windows dedicated server through Wine inside AMP's container runtime. The template uses `cubecoders/ampbase:debian` with Wine installed as instance packages so SteamCMD runs in the same base environment as native templates such as Barotrauma.
- The Linux update flow preloads Valve's SteamCMD bootstrap packages with `curl` before AMP's SteamCMD stage. This is scoped to the RealRTCW instance working directory and avoids the first-run SteamCMD bootstrap parser error seen on this host.
- The update sequence uses AMP's normal SteamCMD stage after the preload. It does not modify other templates or a host-level shared SteamCMD install.
- RTCWCoop registers `coopmap` and `coopdevmap`, so the startup map command uses those instead of stock `map`/`devmap`.
- RTCWCoop game types are `0` Battle, `1` Speedrun, `2` Classes, and `3` Cooperative. The source default is `2` Classes, so the AMP default follows that.
- Player join parsing uses `ClientUserinfoChanged` so AMP can capture the username. Disconnect logs usually only include the client slot, so the leave regex captures `sessionid` rather than username.
- The default generated config is `coop/coopmain/server.cfg`. Advanced one-off cvars can still be edited directly in AMP's file manager after first start.

## Verified Reference Points

- Return to Castle Wolfenstein Steam app ID: `9010`.
- RealRTCW Steam app ID: `1379630`.
- RealRTCW depot path for the dedicated coop server: `coop/RTCWCoopDED.x64.exe`.
- RTCWCoop source cvars used for the settings panel include `g_gameskill`, `g_spawnpoints`, `g_sharedlives`, `g_limbotime`, `g_reinforce`, `g_fastres`, `g_antilag`, `g_voteFlags`, and rotating MOTD cvars.
- RTCWCoop/iortcw log strings used for regex: `ClientUserinfoChanged`, `ClientDisconnect`, and `say`/`sayteam`/`say_limbo`.
