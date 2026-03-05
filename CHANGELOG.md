# Changelog

## 2.0.9 (2026-03-05)

- Update: Service wrap is now enabled only for Torrentio. This prevents Torrentio from returning results when the Torrentio service is down.
- Fix: `Pin Top 1 Resolution` & `Pin Top 1 Resolution/Quality` now properly returns Library stream if library stream happens to be sorted on top.
    - If you don't want Top 1 to always pick your library stream (as that defaults to top sort), then you may need to choose No Library Boost under Sort Option
- Partial Template Update: New option to "Import Only Synced URLs".
    - If selected, this will import only the synced URLs for your core filter selection, keeping your existing regular SEL fields intact (such as optional SELs from the Complete Setup or manual additions).

## 2.0.8 (2026-03-04)

- Update: Integrated changelog directly into templates.
- Update: Usenet overhaul; all Usenet options are now under their own main header.
    - New Boost Uncached Usenet to alter how uncached Usenet vs. Debrid content sorting is handled.
    - Selecting "Boost Uncached Usenet" will move all sorting to 'Global' only.
    - Selecting either Usenet sort option adds an extra SEL in preferred stream expressions to merge Usenet/Debrid results.
- Formatter: Added a modified version for Stremio on Apple TV (thanks to @dividedby & @stepthomas) in the formatter choice selection.
- Fixed: Bug with Usenet passthrough always adding SEL when nothing is selected.

## 2.0.7 (2026-03-04)

- Change: Service wrap turned off due to issues in some AIOStreams instances.


## 2.0.6 (2026-03-03)

- Update: All passthrough streams under Passthrough Options now bypass all SEL filtering limits (e.g., those set under Additional Limit Options).
- Fixed: Language & SEL score now removed from Cached Sort when set to "No Boost."
- New: 1080p Remux filter (@thoaster).
- New: "Keep Custom SEL Scores" (@deluxas) under Misc Options; allows keeping custom edits of RSEs.
- Minor: Updated various menu descriptions for clarity.
- Minor: Cleaned up formatter for 'External Downloader' view.

## 2.0.5 (2026-03-03)

- Fixed: Anime-only language pin syntax error on saving (@blarns).
- Fixed: SEL URLs being overridden in partial template when no SEL selected (@heinzgruber).
- New: Added ☑ ɴᴢʙ Passthrough and ☑ ɴᴢʙ-Only Passthrough.
- Minor: Edited "Boost Cached Usenet" description.
- Minor: Fixed missing space after ᴀʟᴛ ʙᴇsᴛ ʀᴇʟᴇᴀsᴇ in formatter (@archdukeofsalt).

## 2.0.4 (2026-03-03)

- Fixed: Pin/passthrough selection detection. Switched to direct variable usage instead of `inputs.something == true` (@shmoush).

## 2.0.3 (2026-03-03)

- Fixed: 4K Remux not working (moved from ReSE to ESE field) (@pedronolix64gomes).

## 2.0.2 (2026-03-03)

- Update: Default language passthrough amount set to 5 (adjustable).
- Fixed: Removed placeholder `streams` field from ReSE that prevented additional Limit SELs from working.

## 2.0.1 (2026-03-02)

- Fixed: ReSE visual tags passthrough syntax error on saving (@stevenhxo).

## 2.0.0 (2026-03-02)

**Cross-posts with my Discord Post. Release Notes for v2.0.0.**

Yoooo @The SELebrities ! I got a big template update for y'all. With the help of our lord and saviour Viren the Third (don't ask what happened to the first and second...), I’ve completely overhauled our AIOStreams SEL Templates! You no longer need to hunt through dozens of different versions of the same thing. It makes your life easy, it makes my life even easier. Took a long time but I've finished integrating most parts of my complicated setup into the new Template Wizard. It's gonna guide you through a custom setup during import process. Options galore! As always, you know what to do. Try the updated template, leave your feedback and requests over at [Discussion](https://discord.com/channels/1225024298490662974/1391478569607368924) thread. Drop a [coffee](https://ko-fi.com/tamtaro) on your way out if you appreciate the work.

To get the technical stuff out of the way, if you're an instance hosters or selfhosters, @nhyyeb @midnightignite @kuukuuma @viren_7 @funkypenguin.co.nz @srvl @a.ves, ensure these are already in your ENV:
TEMPLATE_URLS=["https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/Tamtaro-All-Templates-for-AIOStreams.json", "https://raw.githubusercontent.com/Vidhin05/Releases-Regex/refs/heads/main/all-templates.json"]
TEMPLATE_REFRESH_INTERVAL=3600 #1 hour. Default: 86400 (1 day)
WHITELISTED_SYNC_REFRESH_INTERVAL=3600 #1 hour. Default: 86400 (1 day)

- If you like my template (I know you will), then you can pin/feature it along with Vidhin template in AIOStreams landing page with:
  FEATURED_TEMPLATE_IDS=tamtaro.complete,Vidhin05.english-template
- If you're a selfhoster you can additionally add `SEL_SYNC_ACCESS=all` to allow all synced URLs to work (beyond the ones in my templates)

┈┈┈┈┈┈┈┈․° ☣ °․┈┈┈┈┈┈┈┈

# Quick Setup Overview

1. **Choose an AIOStreams instance**: Nightly is recommended but not required. #links for options.
   - **Selfhosters**: Ensure `SEL_SYNC_ACCESS=all` is set in your environment variables to receive latest SEL-related hotfixes automatically.
2. **Import Template**: Paste this URL into _AIOStreams → Save & Install :floppy_disk: → Import Template_:
   `https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json`
   - **Confirm Import** to save the templates. Most public instances should already have template.
   - Start with **"Tamtaro Complete SEL Setup"** (works for both Debrid/Usenet or P2P).
   - Follow the prompts to personalize your filtering, services, and credentials. Leave all options default to get my recommended setup.
3. **Advanced Customization**:

- Add Usenet addons if you use them.
- Browse complete list of [Optional SELs](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting/tree/main?tab=readme-ov-file#-optional-sels), most of which are already incorporated inside template.
- Adjust **Ranked Stream Expression** scores via [Vidhin's GitHub](https://github.com/Vidhin05/Releases-Regex) for more nuanced sorting.

4. **Catalogs**: Import my JSONs (with/without anime) via trusted instance of AIOMetadata (#links). This is a separate addon from AIOStreams.
   - Refer to GitHub (end of page) for full set steps.

Pro Tip: If you want to keep your current list of addons but want my full sorting and filtering logic, use the Complete SEL Setup but simply check off "No Addons" in Addon Options, during the import step!
┈┈┈┈┈┈┈┈․° ☣ °․┈┈┈┈┈┈┈┈

##### What’s New: The Complete SEL Setup Experience

There are now only two templates: _Tamtaro Complete SEL Setup_ and _Tamtaro Partial SEL Setup_. See [here](https://discord.com/channels/1225024298490662974/1410118448306192425/1410123621900484678) for full description of each. When you import these templates now, AIOStreams will present you with an onboarding menu. You can toggle features on or off based on your specific needs:

- Addons Options
  - Each mode comes with a preset of recommended addons:
  - P2P Setup (No services selected): _Meteor, Comet, StremThru, TorzS, MediaFusion, Torrentio, TorrentsDB, Peerflix, Sootio, Nuvio Streams, Nuvio Anime, WebStreamr_
  - Debrid Mode (Services Selected): _SeaDex, Library, Meteor, Comet, STorz, Torrentio, MediaFusion, Knaben, AnimeTosho, Sootio_
- Dynamic Addon Options in the Wizard
  - TorBox Search is automatically added if you select Torbox service, and add the NZB version if Pro tier is selected during onboarding.
  - Selecting "No Anime" in the wizard will remove Anime Addons and related anime config from your final build.
  - HTTP Addons are automatically included in the P2P setup but remain an optional toggle for Debrid users who want backup streams for niche titles.
  - If you select "No Addons" during the import, the wizard will ignore the lists above and keep your current addon page exactly as it is, only updating your filtering and sorting.
  - You can now choose your Global Addon Timeout, which will apply across all imported addons.
    ┈┈┈┈┈┈┈┈․° ☣ °․┈┈┈┈┈┈┈┈

- Preferred Language option is now built into the template.
  - These are placed first in the language ranking. Original, Dual Audio, Multi, Dubbed, and Unknown are automatically appended after your selections. Fine-tune the full order in Filters → Language after import. The included formatter will display only your preferred languages.
  - Anything inside Preferred is duplicated inside Required. So streams without any of your preferred languages, beside title's Original language, will be removed.
- Excluded Visual Tags
  - By defaults, AI and 3D visual tags are excluded. If your device does not support DV, select 'DV Only' to exclude all DV streams with no HDR fallbacks. You will remove all HDR+DV streams if you simply exclude 'DV'.
  - Removing various visual tags was a popular request, so you can now decide this during Import, instead of navigating through AIOS -> Filters afterwards.
- Sorting Options
  - Streams are sorted under Global dropdown inside Cached:
    - Debrid/Usenet : _SeaDex → Resolution → Quality → Library → Stream Expression → Stream Expression Score → Language → Encode → Bitrate → Seeders_.
    - P2P Setup: _SeaDex → Resolution → Quality → Library → Stream Expression → Stream Expression Score → Seeders → Language → Encode → Bitrate_.
  - You can fine-tune the Sort Order by applying "Boosts" to specific categories
      - Library Boost: Prioritizes streams already in your library within each category. This is useful for quickly identifying specific episodes or manually added content.
      - Language Boost: Gives priority to streams matching your "Preferred Languages". Higher boosts may rank lower-quality streams higher if they contain your preferred language.
      - SEL Score Boost: Prioritizes results with higher SEL scores, regardless of resolution or quality.
      - Seeders Boost (P2P Only): Ensures streams with the highest seeder counts appear at the top.
    ┈┈┈┈┈┈┈┈․° ☣ °․┈┈┈┈┈┈┈┈

- Recommended Optional SELs
  - You can now select these SELs to be added directly inside your config. No more copy-pasting and editing them off GitHub.
    - ☑ NZB-Only: health-checked Zyclops/UsenetStreamer results only.
    - DV Profile 5: removes DV Only Non-Remux streams.
    - 4K Remux: removes 4k Remux due to device compatability or file size.
    - Bitrate Hardcap (Mobile): removes bitrate over 4K@8Mbps · 1080p@3Mbps · 720p@2Mbps.
    - Bitrate Softcap (Travel): dynamic, doesn't remove higher-bitrate streams if choices are few
- Pin & Passthrough Options
  - These options allow specific streams to bypass Standard SEL filter and optionally appear at the top. Rest of the results will still follow default SEL setup.
    - Language Passthrough: Ensures a set number of streams (you choose) in a specific language always show up, even if they would normally be filtered out. Option to pin these, or to only apply for Anime.
    - Visual Tag Passthrough: Allows up to 5 results for specific tags (like DV, HDR, or SDR) to bypass filters and optionally be pinned to the top.
    - Usenet Passthrough: Allows a specified number of top Usenet results per category to bypass filters, though they typically remain ranked below cached debrid stream.
    - Usenet Boost: Adds a Preferred Stream Expression to allow cached usenet be ranked on the same tier as cached Debrid.
    - Top 1 Pin: Can be configured to pin the single best result per Resolution (max 3) or per Quality/Resolution (max 6).
- Formatter Choice
  - Toggle between my recommended clean formatter, a "Full RSE" for debugging, or keep your existing formatter entirely
    <img width="629" height="459" alt="stremio-shell-ng_fB6TuLQwCy" src="https://github.com/user-attachments/assets/673977d0-16c1-4c48-a7e6-58b5bca45fc2" />

That's it for this All-in-One Complete template. Most Optional SELs can be added right inside the Template Wizard. Lots of options to play around with, dozens of unique setup possible. The Partial Setup template has just the SEL Only and the Formatter only, so nothing else in your config will get changed.

### Direct Links
* [**Yeb's Nightly**](https://aiostreams-nightly.fortheweak.cloud/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json) | [**Kuu's Nightly**](https://aiostreams-nightly.206111.xyz/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json)| [**Midnight's Nightly**](https://aiostreamsfortheweebs.midnightignite.me/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json) | [**Viren's Nightly**](https://aiostreams.viren070.me/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json)
* [**OMNI**](https://aiostreams.12312023.xyz/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json) | [**ATBP Hosting**](https://aio.atbphosting.com/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json) | [**StremioFR**](https://aiostreams.stremiofr.com/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json) | \[[**ElfHosted**](https://aiostreams.elfhosted.com/stremio/configure?menu=about&template=https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json) ⚠️ (*No P2P/Torrentio*)\]
