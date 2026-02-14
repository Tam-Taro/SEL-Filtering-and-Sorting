# SEL-Filtering-and-Sorting

Tired of Stremio pages flooded with streams you'll never use? My **SEL**-driven AIOStreams setup keeps all the good stuff while hiding the clutter. It started as a personal project during the early v2 AIOStreams betas, when I wanted one config I could share with family and friends: mid-resolution options for slower devices, premium remuxes for my own setup, all in a single, smart template.

With [Stream Expression Language (SEL)](https://github.com/Viren070/AIOStreams/wiki/Stream-Expression-Language) in AIOStreams, that "perfect balance" finally became possible. After months of tinkering, testing new features, chasing bugs, and trading tips in the [AIOStreams Discord](https://discord.gg/zRq8dVh5rJ), this guide shares my day-to-day config plus ready-to-import templates focused on SEL filtering and sensible sorting.

## Who this is for

Use this setup as-is, or as a base to tweak for your tastes. It's especially useful if you:

- Want **fewer, higher-quality results** without blunt filters like `Exclude Uncached` or simple `Result Limits`
- Prefer **dynamic filtering** that tightens when lots of results exist (1080p/4K first) but keeps 480p/720p fallbacks when needed
- Spent **hours fiddling** with priorities/sort orders and still aren't happy? This is battle-tested so you can import, try, then adjust instead of starting from scratch.

> [!IMPORTANT]
> 1. My AIOStreams template *does not* include any catalogs. This is because many of us prefer AIOMetadata (separate addon from AIOStreams), so just scroll down to the [AIOMetadata section](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting/tree/main?tab=readme-ov-file#%EF%B8%8F-whats-included-for-aiometadata) for all your metadata and catalog management needs.
>
> 2. My AIOStreams template is based on latest nightly features of AIOStreams. Most cases I'll be pushing features that are not yet released in stable AIOStreams. For my latest to work, choose a nightly AIOStreams as found on [this page](https://status.dinsden.top/status/stremio-addons). If selfhosting, change your container tag from :latest to :nightly, and do `docker compose up -d --force-recreate --pull always aiostreams`.
  <details>
        <summary>PS. I just switched to TorBox on their BF deal, so now I can share my code like everyone else woo!! For the best stremio experience, you need a debrid service, and TB is current top recommendation especially if you're like me, and like to share your stremio setup with family and friends. </summary></summary>
  
    f1cdd3f8-aeee-48f1-849b-64fc7e5aeb3c
  > Use my [referral](https://torbox.app/subscription?referral=f1cdd3f8-aeee-48f1-849b-64fc7e5aeb3c) and we both get +84 days on a yearly sub, if it's your first ever purchase.
    
  </details>

---
## Quick Setup Overview
1. Choose an AIOStreams instance from [this page](https://status.dinsden.top/status/stremio-addons). Nightly is **strongly** recommended.
2. [Import templates](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting/tree/main?tab=readme-ov-file#-how-to-import): Paste `https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/main/Tamtaro-All-Templates-for-AIOStreams.json` into *AIOStreams → Save & Install :floppy_disk: → Import Template*
    - "Confirm Import" to save all templates into your Template Wizard cache, so you don't need to re-paste the url for template updates. 
    - Start with "Complete SEL Setup" (Debrid or P2P options) or "SEL Setup without Addons +- Formatter". See all available templates below.
    - Enter Debrid services (skip for P2P template), TMDB and TVDB credentials 
    - Load Template, Save your AIOStreams into Stremio.
3. Personalize AIOStreams: 
    - Adjust `Preferred Languages` in Filters > Language. 
    - Enable Torbox (default disabled) or add any addons as desired.
    - Browse & try [optional SELs](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting/tree/main?tab=readme-ov-file#-optional-sels).  
    - Import `Extended SEL Only` Template (included in imported url) if you want more results.
    - Adjust Ranked Stream Expressions score as you wish for more nuanced sorting. See ⁠his [GitHub](https://github.com/Vidhin05/Releases-Regex) for more details on customization.
4. [AIOMetadata for catalogs/meta](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting/tree/main?tab=readme-ov-file#%EF%B8%8F-whats-included-for-aiometadata): Import one of my JSONs (with/without anime) via trusted AIOMetadata instances from [this page](https://status.dinsden.top/status/stremio-addons). 
    - Refer to full AIOMetadata guide at end of page.
---
## ✨ Release Notes

February 8, 2026: What's new in template v1.5.0!

<details>
  <summary>v1.5.0</summary>
  <p></p>

**Addons**
- Current Addons (no change from 1.4) 
- Debrid Template: STore, TB Search (optional), SeaDex, STorz, Comet, MediaFusion, Knaben, AnimeTosho, Torrentio.
- P2P Template: Comet, STorz, MediaFusion, Torrentio, TorrentsDB, Peerflix, Nuvio Anime, Nuvio Streams, WebStreamr.  

**Filters/SELs**
- Switched out "Bad Regex" SEL with  "Low SEL Score" in ESE, filtering low RSE scores when enough high ones exist.
- Previous SELs that relied on regexMatched(streams, "Bad") no longer worked, so I rewrote them to use scores instead
- Once known for my SEL's brevity, I managed to achieve that again by cutting 2k chars off my  SELs

**Ranked Stream Expressions**
- Imported from [Vidhin's Regexes - Advanced v0.2.2](https://github.com/Vidhin05/Releases-Regex), which also comes with corresponding Ranked Stream Expressions (and scores) for release groups and other attributes, used in sorting. See his GitHub for more details on customization. 
- The regex portion is the synced url which auto updates daily. When Vidhin releases new updates beyond the version 0.2.2 included in my setup template, you simply import his new template version after you've imported my template first. This will update any RSEs or scoring changes he has made since.

**Sorting**
- Current Sort Order   
  - Global: Cached.    
  - Cached: SeaDex → Library → Resolution → Quality → Stream Expression Matched → **Stream Expression Score** → Language → Encode → Bitrate (new) → Seeders.    
 - Uncached: Same, with Seeders before Language.  
- Changes:    
  - Removed Visual Tag & Audio Tag from sort order  
  - Switched Bitrate for previous Size
**Formatter** 
- Size, folder size and bitrate now uses ::sbytes
- Normalized RSE score as stars out of 5 
- Edited last line to include & format rseMatched and score in small caps
- Removed some old attributes that overlap with rseMatched
 
**Miscellaneous**  
- New template option "Complete Setup without Formatter"
- Extended SEL Only Template now keeps slightly more streams than before (slice 6 instead of 5)
- as before, Standalone SEL Only template has Excluded, Included and Preferred Stream Expressions.  
- Ranked Stream Expressions are handled by Vidhin's Template
- More Optional SELs on [Github](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting#-optional-sels) to further customize your filtering.

**AIOMetadata**
- No change to AIOMetadata since v1.4.0

</details>
<details>
  <summary>v1.4.0</summary>
  <p></p>
	
February 4, 2026: What's new in template v1.4.3!
- G's Low Bitrate ESE now fixed to only trigger if triggering it results in greater than 10 total remaining cached.
- STorz now switched back to using Torznab endpoint.`"torznabUrl": "https://stremthru.13377001.xyz/v0/torznab",`
  - this solves the false parsing of iTunes as Italian language resulting in false sorting
- ongoingSeasonPacks ESE now fixed to work in more niche cases
- removed pre-DigitalRelease ESE until further notice. Previously it was using daysSinceRelease which is using theatrical release datae not digital release date.
- Fixed some syntax issues causing digitalRelease ISE to not work properly (it supposes to passthrough low res quality releases like cams for movies not yet released in digital, and removing fake web-dls and remuxes)

February 2, 2026: What's new in template v1.4.0!
- Addons adjusted slightly: Default urls, SeaDex addon prioritized, optional Torbox.
- Filtering further improved: New recent features such as passthrough, bitrate and math in SEL got incorporated into old and new SELs alike. 
- Formatter upgraded: Bitrate display, shorter titles, everything tweaked for a cleaner than ever look.
- Sort Order updated: Stream Expression Score added replacing Regex Patterns from before.
- Full list of Optional SELs (more to come) are found [here](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting?tab=readme-ov-file#-optional-sels).

**Addons** 
- Current Addons
  - Debrid Template: STore, TB Search (new, optional), SeaDex (new), STorz, Comet, MediaFusion, Knaben, AnimeTosho, Torrentio.
  - P2P Template: Comet, STorz, MediaFusion, Torrentio, TorrentsDB, Peerflix, Nuvio Anime, Nuvio Streams, WebStreamr.  
- Changes:
  - Timeout reduced to 7000 ms for all addons (from 7500 ms).
  - Default empty base urls. Will use whichever default your AIOS hosters decide. Select to change base url as needed.  
  - STorz switched back to Munif's addon instead of Torznab addon so base url field can be left empty.
  - SeaDex newly added for Debrid template and prioritized highly in the order.  
  - New optional addon Torbox Search (torrent) comes disabled.
  
**Filters**  
- New bitrate filter (adapted by G from [TRaSH guide](https://trash-guides.info/Radarr/Radarr-Quality-Settings-File-Size/#standard)) removes low bitrate streams based on queryType detection and quality/resolution thresholds (e.g., 4K WEB-DL >4.6Mbps).  
- New exclusions for keywords "r00", "iso" in excludedKeywords.  
- Digital Release Filter enabled for all query types (movies and series and anime).  
- Preferred Languages updated: "Original" added after English. Japan and Korean now removed as "Original" takes their place.  

**Formatter**  
- Title truncated to 15 chars (from 20). Date is now off for tv series, allows full visibility of ep/season detail in first line. 
- Season/Episode formatting improved: replaces E/S with lowercase, numbers 0-9.
- Added bitrate display (Mbps/Kbps) when available.
- Indexer names truncated to 13 chars for usenet. 
- New feat ::smallcaps now simplified formatter cutting character count by over 1/3: numbers 0-9 instead of detailed labels (e.g., Anime BD T1 → 1).  
- Although uSmallLanguageCodes is used, `Original` language will turn into corresponding language marked visible under this formatter (eg., ꜰʀ still appear on a French movie even tho French wasn't in language filter).

**SEL**  
- Excluded Stream Expressions 
  - New filters: Low bitrate adapted from G, ongoingSeasonPack (hides ongoing season packs), pre-Digital (hides CAM/TS/TC/SCR for new movies).  
  - Improved Seeders filter using q1/q2/percentile for P2P/uncached debrid (more statistical).  
  - Anime filter refined to remove unnecessary 4k and extra seadex results.  
  - New "Movie 4k" filter: removes all 4k except 4K WEB-DL if certain conditions are met, like presence of 1080p REMUX with no corresponding 4k REMUX.  
  - Bad NZBs filter updated.  
  - Cached/uncached slices remain at 3 per category for Standard 3 per category for Extended.
    
**Preferred/Included Stream Expressions**  
- Advanced tiered stream types (Tier 1: cached lib/SeaDex/debrid/torbox usenet; Tier 2: cached nzbdav/http/p2p; Tier 3: uncached usenet; Tier 4: uncached debrid).
  - This is used to sort results, prioritizing various type of cached streams according to its reliability, using Stream Expression Matched in Sort Order.
  - Now incorporated the fix mentioned   
- New: SeaDex passthrough, Library exemption, digitalRelease, 0Cached title passthrough.  

**Ranked Stream Expressions**
 - Imported from Vidhin's Public regex template, which also comes with corresponding Preferred Regex Patterns for release groups as before. See [his github ](https://github.com/Vidhin05/Releases-Regex)for more detail.

**Sorting**  
 - Current Sort Order  
   - Global: Cached.  
   - Cached: SeaDex → Library → Resolution → Quality → Stream Expression Matched → **Stream Expression Score (new)** → Language → Visual Tags → Encode → Audio Tag → Size → Seeders.  
   - Uncached: Similar, with Seeders before Language.  
 - Changes:  
   - Added "streamExpressionScore" after Stream Expression Matched (ranks regex tiers numerically, e.g., Remux T1=1950, Bad=-10000).  
   - Regex patterns updated with scores for anime/non-anime.  

**Miscellaneous**  
- digitalReleaseFilter enabled for series.
- TVDB API now back to mandatory during template onboarding, as various AIOS requires it for better accuracy.
- Descriptions updated across templates to reflect v1.4.0, new features.
- Standalone SEL Only template will now have Excluded, Included and Preferred Stream Expressions.
- Sharing more Optional SELs on [Github](https://github.com/Tam-Taro/SEL-Filtering-and-Sorting/tree/main?tab=readme-ov-file#-optional-sels) to further customize your filtering.

See v1.3.1 for unchanged items.

</details>

<details>
  <summary>v1.3.0</summary>
  <p></p>

December, 24,  2025
What's new in template v1.3.0!
- Addons tuned: Default fetching, 7500 ms timeout.,
- Filters refined: AI tag excluded, SeaDex integrated.,
- Formatter upgraded: Best/Alt release tags, NZB health checks, cleaner layout.,
- SEL improved: New Optional SELs for NZB/DV/Travel filters, easier customization.,
- Sorting updated: SeaDex now top priority.	

December 24, 2025: v1.3.0

__Addons__
- Current Addons
  - Debrid Template:
   - `STore, Torrentio, Comet, MediaFusion, STorz, Knaben, AnimeTosho`
  - P2P Template:
   - `Torrentio, Comet, MediaFusion, STorz, TorrentsDB, Peerflix, Nuvio Anime, Nuvio Streams, WebStreamr`
- Changes:
  - Addon Fetching Strategy now set to Default
  - Timeout for all addons set at 7500 ms
  - Removed meta resource from Torrentio & MediaFusion
  - STorz now uses Forced Query Search Mode

__Filters__
- Removed some lower end items in resolution and visual tag
- Added `AI` into Excluded Visual Tag field (subsequently removed AI filter portion from Anime Filter SEL as no longer needed)
- Stream type now has debrid → usenet priority (only used for deduplication purpose in this setup)
- Miscellaneous now has SeaDex integration enabled

__Formatter__
- Added support for SeaDex, will say "ʙᴇsᴛ ʀᴇʟᴇᴀsᴇ" or "ᴀʟᴛ ʙᴇsᴛ ʀᴇʟᴇᴀsᴇ" on streams considered as Best/Alt release per SeaDex website
- Added support for UsenetStreamer's health checked results; will say ☑ ɴᴢʙ, ☑ ᴇʟғ ɴᴢʙ, ᴜɴᴠᴇʀɪғɪᴇᴅ ɴᴢʙ, or ☒ ɴᴢʙ
- Cleaned up formatting for KissKh streams from StreamAsia (currently offline)
- Last line of formatter will now be in small caps font
- Check #formatter [thread](https://discord.com/channels/1225024298490662974/1408573854393303242/1408573854393303242) for full details

__Miscellaneous__
- Disabled Pre-cache Next Episode completely. This feature caused too many #support instances of "why is this file in my dashboard"
- Switched Language out for releaseGroup in Auto Play Attributes, so now that field has `resolution, quality, releaseGroup`
- Default logo set to custom SEL logo for AIOS

__SEL__
- Excluded Stream Expressions 
  - Added KissKH Filter, Bad NZB Filter, and a few placeholders (blank lines) before my SEL lines for easier customization
    - Perfect for modifying your SEL setup results if you wish to do more filtering before my SEL filtering engine kicks in. See my Optional SELs for some ideas.
  - Reduced "Bad" Regex Filter condition from 15 to 10 non-"Bad" so they're more likely to be removed
  - Inserted seadex(streams) as exemption into various SEL lines so they don't get removed
  - For the full SEL (Standard and Extended), open up the template jsons to see them.
  - *New optional SELs*: A couple optional SELs to put into your placeholders as you see fit
    - __☑ ɴᴢʙ-Only Filter__: A UsenetStreamer filter for those that only want to see health checked results from UsenetStreamer. The SEL line assumes your UsenetStreamer addon is named US, USN, UsenetStreamer, or Usenet Streamer, so avoid naming any other addon with those names.
    - ```text
      /*☑ ɴᴢʙ-Only Filter*/ negate(addon(message(streams,'includes','✅','🧝'),'US','UNS','Usenet Streamer','UsenetStreamer'),addon(streams,'US','UNS','Usenet Streamer','UsenetStreamer'))
	 - __DV-Only Non-Remux Filter__: remove some DV Only streams that give playback issues (purple screen) on some devices. I use this one as my pc doesn't render DV Profile 5 files very well (particularly from Apple TV Web-DLs)
    - ```text
      /*DV Only Non-Remux Filter*/ negate(quality(streams,'BluRay REMUX'), visualTag(streams,'DV Only'))
   - __Size Filter for Travel__: A size filter for a bandwidth conscious setup (aka for traveling). It's a smart size filter that gets applied only if there are >5 results found within each size range for each resolution and query type. So if there are <5 4k movie streams found within 5GB to 15GB, no size filter is applied for 4k streams. The size range was chosen somewhat arbitrarily, with a mindset of not chasing for best quality but for best bandwidth/quality in practice. It's a way to ensure small fake 4k streams don't get pushed to the top, but also a way to remove very large streams (like remuxes) that you're not gonna need when traveling. Please don't complain about the range if you're using this, just tweak the size range to your preference.
     - <details>
       <summary>Size Filter for Travel</summary>
                              
            /* Size Filter for Travel */
            merge(
            /* 2160p / 1440p */
            count(resolution(size(cached(streams),
            queryType=='anime.series'?'1.5GB':queryType=='series'?'2GB':queryType=='anime.movie'?'4GB':'5GB',
            queryType=='anime.series'?'5GB':queryType=='series'?'8GB':queryType=='anime.movie'?'12GB':'15GB'),
            '2160p','1440p'))>5?
            negate(merge(library(streams),seadex(streams),
            resolution(size(streams,
            queryType=='anime.series'?'1.5GB':queryType=='series'?'2GB':queryType=='anime.movie'?'4GB':'5GB',
            queryType=='anime.series'?'5GB':queryType=='series'?'8GB':queryType=='anime.movie'?'12GB':'15GB'),
            '2160p','1440p')),
            resolution(streams,'2160p','1440p')):[],
            
            /* 1080p */
            count(resolution(size(cached(streams),
            queryType=='anime.series'?'500MB':queryType=='series'?'800MB':queryType=='anime.movie'?'1.5GB':'2GB',
            queryType=='anime.series'?'2GB':queryType=='series'?'3.5GB':queryType=='anime.movie'?'4GB':'5GB'),
            '1080p'))>5?
            negate(merge(library(streams),seadex(streams),
            resolution(size(streams,
            queryType=='anime.series'?'500MB':queryType=='series'?'800MB':queryType=='anime.movie'?'1.5GB':'2GB',
            queryType=='anime.series'?'2GB':queryType=='series'?'3.5GB':queryType=='anime.movie'?'4GB':'5GB'),
            '1080p')),
            resolution(streams,'1080p')):[],
            
            /* 720p */
            count(resolution(size(cached(streams),
            queryType=='anime.series'?'250MB':queryType=='series'?'400MB':queryType=='anime.movie'?'700MB':'700MB',
            queryType=='anime.series'?'1GB':queryType=='series'?'1.8GB':queryType=='anime.movie'?'2GB':'2.5GB'),
            '720p'))>5?
            negate(merge(library(streams),seadex(streams),
            resolution(size(streams,
            queryType=='anime.series'?'250MB':queryType=='series'?'400MB':queryType=='anime.movie'?'700MB':'700MB',
            queryType=='anime.series'?'1GB':queryType=='series'?'1.8GB':queryType=='anime.movie'?'2GB':'2.5GB'),
            '720p')),
            resolution(streams,'720p')):[],
            
            /* ≤576p / Unknown */
            count(resolution(size(cached(streams),
            queryType=='anime.series'?'150MB':queryType=='series'?'200MB':queryType=='anime.movie'?'300MB':'300MB',
            queryType=='anime.series'?'400MB':queryType=='series'?'600MB':queryType=='anime.movie'?'800MB':'1GB'),
            '576p','480p','360p','240p','144p','Unknown'))>5?
            negate(merge(library(streams),seadex(streams),
            resolution(size(streams,
            queryType=='anime.series'?'150MB':queryType=='series'?'200MB':queryType=='anime.movie'?'300MB':'300MB',
            queryType=='anime.series'?'400MB':queryType=='series'?'600MB':queryType=='anime.movie'?'800MB':'1GB'),
            '576p','480p','360p','240p','144p','Unknown')),
            resolution(streams,'576p','480p','360p','240p','144p','Unknown')):[]
            )

  
- Preferred Stream Expression
  - `cached(service(streams, 'torbox'), type(streams, 'debrid'), message(type(streams,'usenet', 'stremio-usenet'),'includes','✅', '🧝'))`
  - `merge(cached(service(streams, 'nzbdav', 'altmount', 'easynews')), type(streams, 'http', 'p2p', 'stremio-usenet'))`
  - `uncached(type(streams, 'usenet'))`
  - `uncached(type(streams, 'debrid'))`
  - Changes: 
   - Usenet from easynews dropped to tier 2, Usenet with health checked status moved to tier 1.
   - Added support for stremio-usenet  

__Sorting__
- Current Sort Order
  - __Global__: `Cached`
  - __Cached Sort Order__: `SeaDex → Library → Resolution → Quality → Stream Expression Matched → Regex Patterns → Language → (Seeders for P2P setup) → Visual Tags → Encode → Audio Tag → Size → Seeders`
  - __Uncached Sort Order__: `SeaDex → Library → Resolution → Quality → Stream Expression Matched → Regex Patterns → Seeders → Language → Visual Tags → Encode → Audio Tag → Size`
- Changes: SeaDex now in #1 spot in both Cached & Uncached Sort Order

See Release Notes v1.2.0 for everything else that wasn't changed.

</details>

<details>
  <summary>v1.2.0</summary>
  <p></p>
  
Nov 18, 2025: What's new in template v1.2.0
- Major SEL Engine Rewrite: Went from 3 to 8 filtering blocks for granular control.
- Independent Cached/Uncached Filtering: Separates slice(streams, 3) into slice(cached(streams),3) and slice(uncached(streams,3).
- Revamped Sort Order: Introduces Stream Expressions Matched for nuanced stream ranking.
- Library Stream Exemption: Library streams are no longer counted against the filter limits.
- Revamped Formatter: New icons & Smarter icon auto-switching.
- Bonus: Introduction of the Extended SEL for those who want more results.

<p></p>
<details>
<summary>Formatter</summary> 
<p></p>
  
  - Totally revamped with new icons, new detections, new switching logic, and much more.
  - The title icon will switch from ▤ to ☁︎ to indicate library item
  - ⚡/⏳for cached/uncached
  - ⛊/⛉ for proxied/unproxied
  - ⧉/◧ for season pack/single episode
- Cleaned up formatting for "External Download" streams
- Finial last line that displays extra metadata when found ✎..(regex matched, stream message, edition, network, upscaled, repack, uncensored, and unrated)
</details>

<details>
<summary>Addons</summary>
<p></p>
  
- Removed SubHero as I heard reports that SubHero wasn't working at the moment. I suggest installing subtitle addons directly into your stremio.
- Removed Sootio and Zilean from the Debrid template.
- Switched the StremThru Torz to the Torznab addon to use its API endpoint instead.
  - This is similar to Torz, but it takes advantage of Viren's anime database for title matching for anime.
- Comet, MediaFusion, and StremThru will continue to use @midnightignite 's URLs. Thank her for all your weeb content.
- No catalogs addon is included. This is intentional as I use AIOMetadata for that. Check my guide for configuration and step-by-step setup.
</details>

<details>
<summary>Miscellanous</summary>
 <p></p> 
  
- "Always Pre-cache" is now turned off.
- Trimmed down "Auto Play Attributes" to `resolution, quality, language` per @t… suggestion; this should reduce autoplay failures.
- "Cached to play" is turned on for usenet.
- "Hide Error" is now enabled to hide all errors.
</details>

<details>
<summary>Filters</summary>
<p></p>
  
- "Matching Filter" now includes `Anime` in the Title Matching; Similarity Threshold remains at 0.95. "Season/Episode Matching" now has Strict toggled on.
- "Digital Release Filter" is now disabled. It's redundant as SEL determines when CAM streams are shown only when better qualities are not found.
- "Language Filter" is now simplified; everything was moved to "Preferred Languages" per feedback. If you want stricter language control, as in the previous template, you may still use the Excluded/Required fields. Adjust your language setting as usual.
- The "Stream Type" filter is not used anymore for sorting in this setup. However, the preference order `usenet -> debrid` remains for deduplication purposes.
  - Per Viren, if one addon provides both cached usenet and torrent, then it looks at preferred stream types for deduplication (otherwise, it uses addon order).
- "Preferred Stream Expressions" is now used to rank stream types for sorting purposes, as follows:
  - Reliable Cached (debrid & cached usenet from torbox/easynews service) -> cached usenet from nzbdav/altmount service & http & p2p -> uncached usenet -> uncached debrid
- This order may change as usenet reliability from various sources improves.
- Excluded Stream Expressions (ESEs). This is the big one that you're here for. It's the core engine for filtering in this setup. With your feedback, I rewrote the whole ESEs, expanding upon the previous setup, for a new and improved filtering logic.
</details>

<details>
<summary>Changes to SEL:</summary>
<p></p>
  
- We went from 3 blocks of ESE to 8, divided for clarity so you can easily see what is being filtered Misc -> Enable Statistics.
- #1-2: Seeders Filter (Uncached & P2P)
  - The Seeders filter is the same as before; it will remove low seeder count streams from uncached debrid and P2P (if present). Exceptions are usenet and good regex matched results.
- #3: Anime Filter (4K & AI)
  - A new anime filter to remove AI visual tags (upscaled anime) and to remove 4K from anime series when there are no 4K Blu-ray remuxes present (yes, there are a couple of anime series released in 4K, like `future boy conan`.
- #4: "Bad" Regex Filter
  - Much requested, this now has its own line, so you can quickly remove it using the web UI if you want to keep results from "Bad" release groups.
- #5-7: Cached & Uncached Filter (All Quality/Resolution)
  - This is the other big change to v1.2 SEL. The main filter, slicing by quality/resolution, will now treat cached independently from uncached, keeping the best 3 from each category. Thank you everyone for your feedback on this, @nomercybille and @phamaral to name a couple.
  - New main filter: `{slice(cached(streams),3)}` and `slice(uncached(streams,3)}` for every quality/resolution.
  - The old filter used a simpler `slice(streams, 3)` which meant it almost always kept cached streams as the top 3. While most didn't care about uncached debrid being filtered, those with uncached usenet suffered. They didn't see any uncached usenet in the final result.
- #8: Final Filter (Low Quality & Resolution)
  - This last block is where the final decision is made on how many and what kind of results to show. I spent a lot of time tweaking these numbers here to get the result page just how I like, on as many shows and movies as I could test, with multiple different sets of debrid/usenet services/addons. Huge thanks to @bourboncrow, for whom this wouldn't have been possible.
</details>

<details>
<summary>Sort Order:</summary>
<p></p>
  
- The Sort Order is also revamped to address the new influx of usenet content and some feedback about the old sort order.
- Before you ask: why is nothing on the Global Sort Order page? Well, you must be new here, since I do all my sorting inside "Cached & Uncached Sort Order".
- To address confusion about where to adjust sort order, I initially spent time trying a Global Sort Order setup. Long story short, I had to return to the old "Cached/Uncached Sort order" to achieve the exact order I envision.
- `Stream Expressions Matched` is now a new sort item, replacing `Stream Type` from the previous setup. As explained earlier, this uses the ranking from `Preferred Stream Expressions` which allows for a more complex and nuanced ranking of streams.
- A lot of feedback asked how to show more streams from their language, so I have made the default sort order as language-centric as I can without sacrificing good results priority (i.e., I moved `Language` up to right below `Regex Patterns`; you can certainly move it higher if you wish).
- If SEL is the main engine for filtering, the sort order is the backbone. Adjusting the sort order will affect which top results are kept and which results are removed. If you're not quite happy with what results SEL is keeping, then feel free to adjust the Sort Order to reflect your preference.
</details>

<details>
<summary>Major improvement as a consequence of new SEL + Sort order:</summary>
<p></p>
  
- Library streams are now exempt from all filtering. Previously, if you had three 4K Blu-ray remuxes in your library, the SEL would have selected those three and looked no further for that 4K Blu-ray remux category. The new update means the SEL will look for another three more 4K Blu-ray remux streams.
- For those that have nzbdav/altmount addons that return hundreds of cached usenet, the old SEL + sort order meant their result page was filled with cached usenet from nzbdav (since cached usenet > cached debrid). This is solved with the stream expressions patterns replacing stream type order. Reliable cached results will be favoured accordingly, then uncached usenet, and then lastly uncached debrid.
- For those with uncached usenet, I know you wanted to see more results from your indexers. This new SEL will filter and treat these results separately (via the uncached filter from block #7 so you will see another list of uncached usenet after your usual cached results. What quality or resolution are shown will depend on how many cached results are present.
- For those that saw uncached debrid showing up unnecessarily, I heard you. Low quality cached streams won't be removed in favour of high quality uncached debrid anymore. Any uncached debrid, high or low quality, will only be shown as the last resort when too few results are found.
- For those that asked for more lower-end results, I have relaxed the filtering in block #8 a tiny bit so you're more likely to see a few 720p results sprinkled here and there. Don't worry, this only happens when your list isn't filled out by high quality 4K/1080p.
</details>

<details>
<summary>Extended SEL</summary>
<p></p>
  
  - Last but not least, for making it this far, I made a bonus SEL version called "Extended SEL".
  - A semi-popular request was to relax the filtering from 3 to 5, and I did just that for this version of SEL. Not only that, but I also adjusted various filtering conditions to reflect the higher amount, tweaking the Final Filter specifically to still honour the spirit of the Standard SEL. Poor quality results will only be shown when necessary.
  - In the end, this Extended SEL will keep around 20-30 streams (not including uncached usenet), compared to the Standard SEL which keeps around 15-20. This version may be ideal for those that want a guaranteed 720p result for their mobile needs, while fleshing out the rest of the higher resolution and quality with more streams (from 3 per in Standard to 5 per in Extended).
  - This Extended SEL will only be available as a separate "Extended SEL Only Template" for import, so you must import the main template first with the Standard SEL. Test that out; I'm sure you will be more than happy already with that. Otherwise, open up the template browser again and load the "Extended SEL Only" template to switch between the Standard & Extended SEL versions (nothing else will be overridden, as usual).
</details>
</details>

## ⚙️ Templates Included for AIOStreams

These are setup templates to use with AIOStreams. If you're not sure which AIOStreams instance to start with, check out the list of trusted public instances [here](https://status.dinsden.top/status/stremio-addons). I recommend *nightly* AIOStreams from Midnight, Yeb, Viren, or Kuu. My setup is fine-tuned and tested on latest nightly, so you don't have to worry about features not working. Make sure the instance you chose have a working Torrentio add-on. If not, switch to a different instance.

| Template | Description |
|-----------|--------------|
| **Complete Setup** | Complete configuration with filters, sort orders, streaming addons, and formatter. |
| **Without Addons** | Keeps your existing add-ons while applying the complete setup config. |
| **Without Addons & Formatter** | Applies the complete set up but keeps both your add-ons and formatter untouched. |
| **Complete Setup for P2P** | Complete setup, with p2p/http addons and sort order tailored for those without debrid service |
| **Standard SEL Only** | Imports only the Excluded, Included & Preferred Stream Expressions used in the Complete Setup. | 
| **Extended SEL Only** | Imports the modified SEL that gives slightly more results than Standard SEL. | 
| **Formatter Only** | Imports only the custom formatter used in the template for stream display. |

## 📥 How to Import

1. **AIOStreams → Save & Install 💾 → Import** 
2. Click **Import Template**
3. Copy & Paste the URL to "Tamtaro-All-Templates-for-AIOStreams"
```text
https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/Tamtaro-All-Templates-for-AIOStreams.json
```
4. "Confirm Import" to import all templates available from me.  This will save them into your browser cache for future use. If you already imported previously, this will refresh them to the latest version. 
5. Select one of the templates as you wish to use. I recommend to start with "Complete SEL Setup" as it has the most of my configs shared here.
6. Follow the prompt to configure your debrid credentials (optional if you chose the P2P template). API Keys already configured inside AIOStreams will be prefilled.
7. Enter your TMDB/TVDB credentials for Title Matching and various other features.
8. Load Template, Save your AIOStreams, install into stremio.
9. Go to end of this page for instructions on how to setup your catalogs via AIOMetadata addon (which is a separate addon from AIOStrems).

  > [!NOTE]
  > Remember to personalize your imported config by going to `Filters` -> `Language`. Select your main language as the top spot in Preferred Languages, then sort/rank the rest according to your preference. I suggest keeping Dubbed, Dual Audio, Multi, Unknown in the list as they may contain streams of your preferred languages.
> To further enhance your sorting, Vidhin's regex template is now incorporated, which tags and scores streams based on the quality of the release groups and other attributes. Check his [GitHub](https://github.com/Vidhin05/Releases-Regex) for more instructions on further customization, and updates.

> How to Import new updates of Vidhin's regex template: About -> Template Wizard -> Import Template. Paste the template URL:
> ```https://raw.githubusercontent.com/Vidhin05/Releases-Regex/main/advanced-template.json```. Click go, and then load the template.
> 
> This regex labels and scores all streams according to TRaSH Guides. The template from Vidhin has two portions. The regex portion is the synced url which auto updates daily. The second portion is the Ranked Stream Expressions (RSEs) with scores attached. When Vidhin releases new updates beyond the version included in my setup template, you will need to import his new template version after you've imported my template first. This will update any RSEs or scoring changes he has made since.

## 🔧 Optional SELs

While my SEL setup is a complete setup, it can always be tweaked further for various specific needs. Over the months I've shared these SELs with you guys on Discord, or found them useful, shared by others. Most of these are Excluded Stream Expressions, which you can either put *before* or *after* all my ESEs. Where to put them is important and will be noted.

These go into Excluded *before* all my Excluded SELs, in one of the empty placeholders on top:
  - __☑ ɴᴢʙ-Only Filter__: A UsenetStreamer filter for those that only want to see health checked results from UsenetStreamer. The SEL line assumes your UsenetStreamer addon is named US, USN, UsenetStreamer, or Usenet Streamer, so avoid naming any other addon with those names.
	- ```text
  	  /*☑ ɴᴢʙ-Only Filter*/ negate(addon(message(streams,'includes','✅','🧝'),'US','UNS','Usenet Streamer','UsenetStreamer'),addon(streams,'US','UNS','Usenet Streamer','UsenetStreamer'))```
  - __DV-Only Non-Remux Filter__: remove some DV Only streams that give playback issues (purple screen) on some devices. I use this one as my pc doesn't render DV Profile 5 files very well (particularly from Apple TV Web-DLs)
    - ```text
      /*DV Only Non-Remux*/ isAnime?[]:negate(merge(quality(streams,'BluRay REMUX'),releaseGroup(streams, 'Flights'),regexMatched(streams, 'Hulu')),visualTag(streams,'DV Only'))
  - __TorBox Download Limit__: TB Essential and Standard have a max uncached download size of 200GB, which is increased to 1TB for TB Pro. Use the appropriate SEL if you're with TB.
    - ```text
      /*TB Non-Pro Download Limit*/ size(uncached(streams), '200GB')
    - ```text
      /*TB Pro Download Limit*/ size(uncached(streams), '1TB')
  - __Bitrate Hardcap for Mobile__: Unlike the one below, where if no results for 4k or 1080p at bitrate cap, it fallbacks to show higher bitrate anyway. This SEL however will set a hardcap based on your mobile internet connection. Will assume 4-5G mobile: 4K @ 8Mbps, 1080p @ 3Mbps, 720p or lower @ 2Mbps
    - ```text
      /*Bitrate Hardcap*/
		merge(bitrate(resolution(streams, '2160p'), '8Mbps'),
		  bitrate(resolution(streams, '1440p', '1080p'), '3Mbps'),
		  bitrate(resolution(streams, '720p','576p', '480p', '360p', '240p', 'Unknown'),'2Mbps'))
  - __Bitrate Softcap for Travel__: A bitrate filter for a bandwidth conscious setup. It's a smart bitrate filter that gets applied only if there are >5 results found within each bitrate range, which dynamically increases (to the max bitrate available) until >5 results are found for that resolution. The bitrate range was chosen somewhat arbitrarily, with a mindset of not chasing for best quality but for best bandwidth/quality in practice. This is not a flat hard cap on bitrate, since I like to still see results than no results at all even if they're too big to watch on mobile.
     - <details>
       <summary>Bitrate softcap for Travel</summary>
                         
			/*Bitrate Softcap*/
			merge(bitrate(cached(resolution(streams,'2160p')),
			count(bitrate(cached(resolution(streams,'2160p')),1,'6Mbps'))>5?'6Mbps':
			count(bitrate(cached(resolution(streams,'2160p')),1,'9Mbps'))>5?'9Mbps':
			count(bitrate(cached(resolution(streams,'2160p')),1,'12Mbps'))>5?'12Mbps':
			count(bitrate(cached(resolution(streams,'2160p')),1,'15Mbps'))>5?'15Mbps':
			count(bitrate(cached(resolution(streams,'2160p')),1,'20Mbps'))>5?'20Mbps':
			max(values(cached(resolution(streams,'2160p')),'bitrate'))
			),bitrate(cached(resolution(streams,'1440p','1080p')),
			count(bitrate(cached(resolution(streams,'1440p','1080p')),1,'6Mbps'))>5?'6Mbps':
			count(bitrate(cached(resolution(streams,'1440p','1080p')),1,'9Mbps'))>5?'9Mbps':
			count(bitrate(cached(resolution(streams,'1440p','1080p')),1,'12Mbps'))>5?'12Mbps':
			count(bitrate(cached(resolution(streams,'1440p','1080p')),1,'15Mbps'))>5?'15Mbps':
			count(bitrate(cached(resolution(streams,'1440p','1080p')),1,'20Mbps'))>5?'20Mbps':
			max(values(cached(resolution(streams,'1440p','1080p')),'bitrate'))
			),bitrate(cached(resolution(streams,'720p')),
			count(bitrate(cached(resolution(streams,'720p')),1,'6Mbps'))>5?'6Mbps':
			count(bitrate(cached(resolution(streams,'720p')),1,'9Mbps'))>5?'9Mbps':
			count(bitrate(cached(resolution(streams,'720p')),1,'12Mbps'))>5?'12Mbps':
			count(bitrate(cached(resolution(streams,'720p')),1,'15Mbps'))>5?'15Mbps':
			count(bitrate(cached(resolution(streams,'720p')),1,'20Mbps'))>5?'20Mbps':
			max(values(cached(resolution(streams,'720p')),'bitrate'))
			))
</details>

These go into Excluded *after* all my Excluded SELs:
  - __Global Result Limit__: After all my filtering SELs have ran you get left off with 3 of each category, totalling about 20 streams in all. You can simply cut this number down to any number you want, I will go with 6 to get even amount from 2 categories (eg. 3 x 4k Remux + 3 x 4k Bluray). Library and Seadex results are not counted.
    - ```text
      /*Global Result Limit: 6*/slice(negate(merge(library(streams), cached(seadex(streams))), streams), 6)
  - __Global Result Limit__: Same as above, to be used by folks with torbox usenet only. This passthroughs 6 non-usenet and 6 usenet results.
    - ```text
      ```/*Global Result Limit: 6*/ merge(slice(negate(merge(library(streams),cached(seadex(streams))), (type(streams, 'debrid', 'http', 'p2p'))), 6), slice(negate(merge(library(streams),seadex(streams)), (type(streams, 'usenet'))), 6))
These go into Included Stream Expressions, order doesn't matter here:
  - __Language Passthrough__: If you want some amount of your results in another language to always show up, skipping mostly all filters, then this is the SEL for you. Change `yourLanguage` to whatever your language you want to see ~ 5 streams of, these streams will bypass title matching & our excluded SELs (`title`, `excluded`).  We're not bypassing a lot of other filters like deduplication so you may see less than 5. This is the full list of passthrough filters you can skip: `filter,dedup, limit, excluded, required, title, year, episode, digitalRelease, language`. Make multiple of these SELs for other language passthroughs if desired. Can adjust the number from 5 to whatever you want. If you still don't see your language in results, it's most likely because your addons didn't return any.
    - ```text
      /*yourLanguage*/ passthrough(slice(language(merge(cached(streams), type(streams, 'usenet')) 'yourLanguage'), 0, 5), 'title', 'excluded')
  - __Addon Passthrough__: This SEL will passthrough cached results (or uncached usenet) from `yourAddon` so that they skip the Excluded Stream Expression filters (`excluded`). Same as above, adjust the number 5 to 99+ if you want to passthrough all results. Adjust the passthrough filters if you want to skip more stages, see above for full list.
    - ```cached
      /*yourAddon*/ passthrough(slice(addon(merge(cached(streams), type(streams, 'usenet')), 'yourAddon'), 0, 5), 'excluded')

  - __DV Passthrough__: This will passthrough 5 DV streams in 4k/1080p, and if there are less than 5 of such present, it will also passthrough 5 DV streams in 720p. Specifically, my exclusion SELs won't work on these DV streams. Adjust 5 to 99+ if you want to passthrough all.
    - ```text
      /*DV Passthrough*/ count(resolution(visualTag(merge(cached(streams), type(streams, 'usenet')), 'DV'), '2160p', '1080p')) > 5 ? passthrough(slice(resolution(visualTag(merge(cached(streams), type(streams, 'usenet')), 'DV'), '2160p', '1080p'), 'excluded'), 0, 5) : passthrough(slice(resolution(visualTag(merge(cached(streams), type(streams, 'usenet')), 'DV'), '2160p', '1080p', '720p'), 'excluded'), 0, 5)

### 🧩 Manual Setup of Template v1.1.0 (Outdated )

<details>
  <summary>This section is outdated, refer to my Release Notes for latest changees. The best way to use my config is by importing one of my templates above.</summary>
  <p> </p>
This is my recommended setup that should work for most of you. If you just want a finished template, then import & use one of the templates described above. Otherwise read on to customize your current AIOStreams instance.

### **Sorting**
- __Global Sort Order Type:__  `Cached`
  - We put `Cached` here since we want our results to show cached before uncached
  - If `Cached` is first item, the sort algorithm automatically goes to Cached & Uncached Sort Order, nothing else after `Cached` matters in Global Sort Order
- Cached Sort Order Type: `Resolution → Library → Quality → Regex Patterns → Stream Type → (if p2p only, Seeders here) → Visual Tag → Audio Tag → Encode → Language → Size → Seeders`
  - There is a lot of flexibility here, re-arrange to your preference as to what your top results should prioritize
  - Note that p2p is considered cached and therefore p2p sorting uses this sort order, so if you're using a p2p setup, move `Seeders` up
- Uncached Sort Order Type: `Resolution → Library → Stream Type → Seeders → Regex Patterns → Quality → Language → Size`
  - Generally the same as `Cached Sort Order` except Seeders is higher 
  - There should be very little to no uncached streams in your results (unless you're using usenet which are mostly uncached), therefore you don't need an exclusive list in this uncached sort order

> [!NOTE]
> If your first filter in `Global Sort Order` is `Cached` and you left `Cached/Uncached Sort Order` blank, your sort/filter may not work properly.

## 🎚️ Filtering for Template v1.1 (Outdated)
- Define `Preferrence Order` in each of `Resolution, Quality, Encode, Stream Type, Visual Tag, Audio Tag` and `Language`. 
  - This is important for our Sort Order to work.
> [!NOTE]
> You will have to edit this section to your personal preference.
  - Under `Language` -> `Required Language`, select all your required languages there. This is ensure streams of the language you watch will be included in the results.
  - Set `Preferred Languages` to be the same as your `Required Languages`, reorder/rank them to your preferrence. 
  - By default my setup has the following languages: `English, Japanese, Korean, Dubbed, Dual Audio, Multi, Unknown`. Regardless of your choice, do not remove `Dubbed, Dual Audio, Multi, Unknown` from either lists, as some streams of your language may fall under these language tags.

- Default is recommended in the rest of the filters. If not sure, check my AIOStreams template json for how I configured them 
  - `Quality`: Remove default items in `Excluded` since my SEL will remove CAM/TS/etc when necessary
  - `Visual Tag`:  If your device doesn't support DV add `DV Only` into `Excluded`. Same for 3D. 
  -   `Encode`: `Exclude H-OU, H-SBS` for non 3DTV
 - Optional: Import [Vidhin's json for merged anime regexes](https://github.com/Vidhin05/Releases-Regex/blob/main/README.md) into `Regex → Preferred Regex Patterns → Import from url icon` 
 ```text
https://raw.githubusercontent.com/Vidhin05/Releases-Regex/main/merged-anime-regexes.json
```
   - This regex labels all streams with tier rankings based on reputation/quality of release groups per [TRaSH Guides](https://trash-guides.info/). You will need to reimport the regex occassionally whenever Vidhin pushes an update to the regex because many AIOStreams instances will only allow the use of his latest update regex. 
 - Optional: Under `Matching`, enable `Season/Episode Matching` and `Title Matching`  with `Match Year (1 Year Tolerance)` and `Exact Matching Mode` selected. I suggest boosting the `Similarity Threshold` from `0.85 to 1`. Adjust these settings as needed.
 - Under `Deduplicator`, select all 3 `Detection Methods`. Set to `Aggressive` for `Multi-Group Behaviour`. Leave everything else as default (Single Result). 

> [!IMPORTANT]
> Leave all remainder filters setting, `Excluded` `Included` and `Required` boxes empty. As tempting as it sounds to select `Exclude Uncached` or set your `Result Limits`, my SEL will do that for you. This is the first troubleshooting step if your sort/filter looks off!

## 🎚️🤖 Filtering with Stream Expression Language (SEL) for Template v1.1 (Outdated)

- All filtering (besides title match and dedupe) can be done with stream expressions. My setup leaves the language filtering to AIOStreams basic language filter (altho can be done using SEL - just think the basic filter is good enough).
- There are two schools of thought with regards to SEL Filtering:
  - using it to filter during fetching stage via `Dynamic Group Exit Condition` or,
  - using it after all initial filtering has been done by AIOStreams via  `Excluded Stream Expressions` (ESE) 
- My setup has been fine-tuned using the second method for over 3 months now with feedback from users. I may incorporate the first method into my setup in the future, however for now heavy work of filtering is done using my three lines of ESE. Copy-paste the codes below into `AIOStreams -> Excluded Stream Expressions` or import them using the SEL-only template json at the top of this page.
  <p>
    <details>
        <summary>First line into ESE: Uncached Filter</summary>
  
    ```text
 
     ```   
    </details>
  </p>
  <p>
    <details>
        <summary>Second line into ESE: Main Quality/Resolution Filter</summary>
  
    ```text
    ```
    </details>
  </p>
  <p>
    <details>
        <summary>Third line into ESE: Low Quality/Resolution Filter</summary>
  
    ```text

    ```
    </details>
  </p>

The first block of SEL for your `Excluded Stream Expressions` (ESE) is the Uncached Filter. It checks and removes uncached debrid streams with low resolution and low seeder count, except for good regex-matched. It also removes P2P streams if present for low seeder count. Usenet results are exempt from this Uncached Filter.

The second block of ESE is the Main Quality/Resolution Filter. It uses `slice(...,3)` to further trim streams, keeping top ~3 results of most `Quality` and `Resolution` combination. Because AIOStreams sorts streams before SEL filtering, you can determine how your streams is sorted first so that the `slice` of the top results of any `quality` /`resolution` will always select your preferred "highest-quality streams". For me, that would be Vidhin's regex-matched streams, so I put `Regex Pattern` right underneath `Quality` in Sort Order. If you value size or language for every resolution + quality pair then put `Size` or `Language` right underneath `Quality`, the `slice` will then keep your top 3 streams for that particular `quality`/`resolution` according to your size or language preference, respectively.

Third block of ESE is the Low Quality/Resolution Filter. It checks how many streams made through the Main Filter above, and removes low quality and low resolution streams when there are already enough present. It also removes regex-matched streams from "Bad" quality release groups when there are enough non-"Bad" streams present (for those that use Vidhin's regex).
</details>

---
## ⚙️ What’s Included for AIOMetadata

These are setup configs to use with AIOMetadata. It is a powerful tool for all things metadata and catalogs. If you're not sure where to start, pick an AIOMetadata instance from [the list of trusted public instances](https://status.dinsden.top/status/stremio-addons)  or use the [Elfhosted instance](https://aiometadata.elfhosted.com/configure/). You can't go wrong with either choice.

> [!NOTE]
> My previous AIOStreams template does not include any catalogs. This is where AIOMetadata comes in. It is a separate addon from AIOStreams, installed directly into Stremio (or via StremThru Side Kick), and is the main place most of us like to keep our catalogs for Stremio!


| Complete config | Description | Download |
| :-- | :-- | :-- |
| **With Anime Catalogs** | Complete configuration with anime metadata preset, TV, movies, and anime catalogs. Huge props to Cedya, Snoak \& Mr Professor for their awesome lists! | [Direct Link](https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/AIOMetadata%20Configs/Tamtaro-aiometadata-config-with-anime.json) [Mobile Link](https://www.dropbox.com/scl/fi/7mfhmz9ptddrqtfqegqeu/Tamtaro-aiometadata-config-with-anime.json?rlkey=3jdolg2pjyxf1ju5axg5vnoiu&st=gzvkid51&dl=1)|
| **Without Anime Catalogs** | Complete configuration for TV and movies catalogs. No anime. | [Direct Link](https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/AIOMetadata%20Configs/Tamtaro-aiometadata-config-without-anime.json) [Mobile Link](https://www.dropbox.com/scl/fi/cl6b1z4nof1150aagpoa8/Tamtaro-aiometadata-config-without-anime.json?rlkey=d95gxqr85tna3ujqa933zzhj7&st=wsekixtd&dl=1)|

## ✨ Release Notes

<details>
<summary>Februrary 2, 2026 (v1.2.3):</summary> 
<p></p>
 
 - Replaced old Seasonal with new that is now working, showing Valentine's Day content. Going forward this won't need to change anymore to see seasonal update
 - Without anime config got all the genre catalogs (eg. Action to Thriller) enabled, all decade lists randomized per page
 - On anime config, removed AniDB Latest Started (as a lot of the titles don't have proper metadata), replaced previous custom AniList Trending with builtin Anilist Trending
   - Added Crunchyroll Latest (movies/series) and various genre catalogs onto Home
   - Disabled excess MAL catalogs (Upcoming, Studios) to free up space
</details>

<details>
<summary>December 7, 2025 (v1.2.2):</summary> 
<p></p>
	
 - Enabled AI features, search catalogs.
 - Increased global cache TTL from 40000 to 43200 seconds across catalogs.
 - Replaced 'Tis the Season' with 'Seasonal' where it'll auto-update as season changes
 - Minor sort order tweaks on various catalogs (e.g., some lastairdate/released from desc → asc).
 - Enabled more genres catalogs (eg. Action, Comedy) for config without anime.
</details>

<details>
<summary>December 7, 2025 (v1.2.1):</summary> 
<p></p>
  
- MDBList updated sort direction behaviour, so adjust all lists accordingly to now use `ascending` for those lists with last air/released sort
 - For those not using my AIOM config, make sure the sort direction of any imported MDBLists now says `ascending` (if you adjusted their sort order before).
- Switched out the previous holiday list (from v1.2.0) with my own "Seasonal" list that will stay permanent on the top spot.  
  - Will push new seasonal content onto that same list on my end, so you won't need to keep re-updating your AIOM config.  
  - If you just want this list onto your existing setup, then import it via this URL: `https://mdblist.com/lists/tamtaro/seasonal`.
- AI Search was implemented by @Din recently, it's now part of this config, so you'll need to obtain your own Gemini key for the integration tab prior to importing. 
- Some other config changes
 - Adjusted all imported catalogs TTL to exactly 12 hours (43200s), the "top" lists remain random/shuffled per v1.2.0. 
</details>

<details>
<summary>November 23, 2025 (v1.2.0):</summary>
<p></p>
  
- New holiday list introduced to go along with AIOStreams template v1.2.0: a shuffled mix of Christmas classics.
  - you can directly import the list using this url: `https://mdblist.com/lists/snoak/christmas-movies`
- Turned on `MDBList Watch Tracking` by default, a new MDBList feature recently supported by AIOMetadata.
- Added a new shuffle feature for all the "Top" lists.
- Enabled kitsu search, new AIOM feature
- Shoutouts to Snoak, Cedya, Din, and The Professor for this config.
- Two config versions available as before.
</details>

<details>
<summary>October 30, 2025 (v1.1.0):</summary> 
<p></p>
  
- AIOMetadata config updated to v1.1.0 to match with AIOStreams template v1.1.0 release
- Due to difficulty installing previous config due to `max descriptor size` error, each jsons now has just enough catalogs (~60) enabled to allow easy installation into stremio. 
- Removed traces of disabled catalogs from config without anime content, reducing its json filesize dramatically.
  - left disabled catalogs in config with anime content, for those that want to enable more catalogs
- Adjusted `type` to lowercase (`movie/series`) for better compatibility.
- Rearranged and renamed some catalogs.
</details>

<details>
<summary>October 17, 2025 (v1.0.0):</summary> 
<p></p>
  
- Debut of AIOM configs with SEL Setup Version 2.5.1.
- Removed AIOLists from preconfigured addons in SEL setup, focusing AIOM config for catalogs and AIOStreams config for streaming addons only.
 - Old AIOLists catalogs replaced by AIOMetadata catalogs (thanks to @Snoak and @Mr. Professor).
- Requires separate installation of AIOMetadata addon within Stremio, independent from AIOStreams addon.
- Two JSON configs available: with and without anime catalogs.
  - anime config: 80 enabled out of 150
  - non-anime config: 67 enabled out of 150
- Due to the high amount of catalogs, use StremThru Side Kick to bypass `AddonsPushedToAPI - Max descriptor size reached` error
- Config uses TMDB for movie meta, TVDB for series/anime meta, and kitsu for anime ID.
</details>

## 📥 How to Import

This import will give you catalogs and meta support for Stremio, to be installed outside AIOStreams. Pick one of the two JSON files shared above to import into AIOMetadata following these instructions:

1. **Integrations tab** → Obtain and enter your TMDB, TVDB, MDBList, and Gemini APIs. Use `t0-free-rpdb` for RPDB posters or obtain TOP API for alternate TOP posters. Gemini is needed for AI Search. Fanart.tv API Key is optional. Hit the `Test All Keys` button to ensure they're all ✅.
2. **Configuration tab** → Import Configuration → Import one of my JSON files.
    - Feel free to edit/hide/delete/import various catalogs in Catalogs tab to your liking. Just note that the current config already has close to the max number of catalogs stremio will allow in one AIOMetadata install. See below for more on this.
    - Adjust `Display Language` inside AIOMetadata under General tab if you're not using default English.
3. **Configuration tab** → Save Configuration → Enter a password to save your configuration (if you haven't made an account before) → Install the addon directly into Stremio. **Note**: If you encounter `AddonsPushedToAPI - Max descriptor size reached` error, try step 4 otherwise if your AIOM installed fine directly into stremio, skip to step 5.
4. Copy your `Install URL`. Go to [StremThru Side Kick](https://stremthru.13377001.xyz/stremio/sidekick/), log in there using your Stremio account and use the Install button there to install the addon with the AIOMetadata URL. This *may help* bypass the `AddonsPushedToAPI - Max descriptor size reached` error; otherwise disable some catalogs to reduce size or see below "How to set up 2 AIOMetadata for more catalogs".
5. Go to [https://cinebye.dinsden.top](https://cinebye.dinsden.top), load up your account and remove all three Cinemeta features (Search, Catalogs, and Meta). Then scroll down to the bottom of Cinebye and re-order your addons so that 1. `Cinemeta (cinebyed)` 2. `AIOMetadata` 3. `Rest of addons (AIOStreams, subtitle addons, etc)`. Finally, save the changes by clicking `Sync to Stremio`. The re-ordering can also be done inside StremThru Side Kick under "Move" mode.

> [!NOTE]
> If you make any catalog changes in your AIOMetadata, you will need to re-install AIOMetadata for it to be updated inside Stremio. Instead of re-installing and re-ordering your AIOMetadata again each time, you should use StremThru Side Kick "reload" feature, which effectively does the re-install in one click of a button.

*Some of you seem to want more catalogs*. My config for no anime has most of the "western" content catalogs from AIOMetadata/Snoak's MDBLists enabled and still stayed within the size limit allowed by Stremio for 1 AIOM's UUID installation. For the config with anime, I had to disable half of the "western" catalogs to accommodate for anime catalogs as well. Stremio will only allow around ~60 catalogs per AIOM config before you see the `AddonsPushedToAPI - Max descriptor size reached` error during installation.

### ➕ More Catalogs with 2 AIOMs

If you want more catalogs, do the following:

- Use my config as a base, add/remove/enable all the catalogs you want in the first AIOMetadata UUID you made following my guide.
- Save and export this "completed" copy into a JSON (with keys included).
- Then go back to Catalog tab and disable the second half of your catalogs (those that went over the limit).
- Then save this config again. This will be your **AIOM\#1**.
- Make another AIOM UUID, and import the "completed" JSON copy from earlier into your **AIOM\#2**.
- In this new UUID, disable the catalogs you had enabled previously (aka the first half) and before saving, toggle the `Catalog Mode Only` option right above the `Save Configuration` button (on this second UUID).
- Install both AIOMs into your Stremio and re-order them via StremThru Side Kick accordingly: [1. `Cinemeta (Cinebye'd)` 2. `AIOM#1` 3. `AIOM#2` 4. `Rest of your addons (AIOStreams, subtitle addons, etc)`].
