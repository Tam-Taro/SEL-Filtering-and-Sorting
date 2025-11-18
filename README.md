# SEL-Filtering-and-Sorting
  Since beta of the current v2 AIOStreams, I always wanted the perfect config that would show only the best results, not just for myself, but for my family and friends too. Some prefer lower quality/resolution streams for their lower end TV, while I prefer the latest remuxes. An all-in-one config that I could share. We didn't want to see dozens of results we would never use anyway. This balancing act was finally possible with the release of [Stream Expressions Language (SEL)](https://github.com/Viren070/AIOStreams/wiki/Stream-Expression-Language) capability in AIOStreams.  

  After months of tinkering, always finetuning my setup, testing newest features and bugs, and exchanging numerous tips with you all over at [the AIOStream discord,](https://discord.gg/zRq8dVh5rJ) I will use this space to share some of that with you. You will find a guide to my personal config that I'm happy with and importable templates for AIOStreams, with a heavy focus on SEL as the filtering tool. It is tailored to my preference, but has broadened over time so that most of you should find it as a good starting point, if not the final piece to your stremio puzzle.

Consider using my setup template as is if you:
- Don't want to see too many results, but also don't want to miss out on any high quality streams that may be removed by simple filtering like `Exclude Uncached` or `Result Limits`
- Want a config that will adjust its filtering strictness based on how many results are found. Dont want those old 480p cartoons filtered out? But don't want to see 720p streams when there are a bunch of 1080p & 4K present already? This setup got you covered. 
- Have spent hours trying to prioritize certain streams and sort your results to your liking but still not quite happy yet. This is the right place for you, as I can guarantee to have spent more hours on this setup, and it's free, so why not try?

Lastly, please do note my AIOStreams template *does not* include any catalogs. This is because many of us prefer AIOMetadata (separate addon from AIOStreams), so just scroll to the end of this page, you will find my config for AIOMetadata for all your metadata and catalog management needs.

---
## ✨ Changes to template from v1.1 to v1.2

Formatter
- Totally revamped with new icons, new detections, new switching logic, and much more.
  - The title icon will switch from ▤ to ☁︎ to indicate library item
  - ⚡/⏳for cached/uncached
  - ⛊/⛉ for proxied/unproxied
  - ⧉/◧ for season pack/single episode
- Cleaned up formatting for "External Download" streams
- Finial last line that displays extra metadata when found ✎..(regex matched, stream message, edition, network, upscaled, repack, uncensored, and unrated)

Addons
- Removed SubHero as I heard reports that SubHero wasn't working at the moment. I suggest installing subtitle addons directly into your stremio.
- Removed Sootio and Zilean from the Debrid template.
- Switched the StremThru Torz to the Torznab addon to use its API endpoint instead.
  - This is similar to Torz, but it takes advantage of Viren's anime database for title matching for anime.
- Comet, MediaFusion, and StremThru will continue to use @midnightignite 's URLs. Thank her for all your weeb content.
- No catalogs addon is included. This is intentional as I use AIOMetadata for that. Check my guide for configuration and step-by-step setup.

Miscellanous
- "Always Pre-cache" is now turned off.
- Trimmed down "Auto Play Attributes" to `resolution, quality, language` per @t… suggestion; this should reduce autoplay failures.
- "Cached to play" is turned on for usenet.
- "Hide Error" is now enabled to hide all errors.

Filters
- "Matching Filter" now includes `Anime` in the Title Matching; Similarity Threshold remains at 0.95. "Season/Episode Matching" now has Strict toggled on.
- "Digital Release Filter" is now disabled. It's redundant as SEL determines when CAM streams are shown only when better qualities are not found.
- "Language Filter" is now simplified; everything was moved to "Preferred Languages" per feedback. If you want stricter language control, as in the previous template, you may still use the Excluded/Required fields. Adjust your language setting as usual.
- The "Stream Type" filter is not used anymore for sorting in this setup. However, the preference order `usenet -> debrid` remains for deduplication purposes.
  - Per Viren, if one addon provides both cached usenet and torrent, then it looks at preferred stream types for deduplication (otherwise, it uses addon order).
- "Preferred Stream Expressions" is now used to rank stream types for sorting purposes, as follows:
  - Reliable Cached (debrid & cached usenet from torbox/easynews service) -> cached usenet from nzbdav/altmount service & http & p2p -> uncached usenet -> uncached debrid
- This order may change as usenet reliability from various sources improves.
- Excluded Stream Expressions (ESEs). This is the big one that you're here for. It's the core engine for filtering in this setup. With your feedback, I rewrote the whole ESEs, expanding upon the previous setup, for a new and improved filtering logic.

Here are the differences in this version of SEL:
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

Sort Order:
- The Sort Order is also revamped to address the new influx of usenet content and some feedback about the old sort order.
- Before you ask: why is nothing on the Global Sort Order page? Well, you must be new here, since I do all my sorting inside "Cached & Uncached Sort Order".
- To address confusion about where to adjust sort order, I initially spent time trying a Global Sort Order setup. Long story short, I had to return to the old "Cached/Uncached Sort order" to achieve the exact order I envision.
- `Stream Expressions Matched` is now a new sort item, replacing `Stream Type` from the previous setup. As explained earlier, this uses the ranking from `Preferred Stream Expressions` which allows for a more complex and nuanced ranking of streams.
- A lot of feedback asked how to show more streams from their language, so I have made the default sort order as language-centric as I can without sacrificing good results priority (i.e., I moved `Language` up to right below `Regex Patterns`; you can certainly move it higher if you wish).
- If SEL is the main engine for filtering, the sort order is the backbone. Adjusting the sort order will affect which top results are kept and which results are removed. If you're not quite happy with what results SEL is keeping, then feel free to adjust the Sort Order to reflect your preference.

Major improvement as a consequence of new SEL + Sort order:
- Library streams are now exempt from all filtering. Previously, if you had three 4K Blu-ray remuxes in your library, the SEL would have selected those three and looked no further for that 4K Blu-ray remux category. The new update means the SEL will look for another three more 4K Blu-ray remux streams.
- For those that have nzbdav/altmount addons that return hundreds of cached usenet, the old SEL + sort order meant their result page was filled with cached usenet from nzbdav (since cached usenet > cached debrid). This is solved with the stream expressions patterns replacing stream type order. Reliable cached results will be favoured accordingly, then uncached usenet, and then lastly uncached debrid.
- For those with uncached usenet, I know you wanted to see more results from your indexers. This new SEL will filter and treat these results separately (via the uncached filter from block #7 so you will see another list of uncached usenet after your usual cached results. What quality or resolution are shown will depend on how many cached results are present.
- For those that saw uncached debrid showing up unnecessarily, I heard you. Low quality cached streams won't be removed in favour of high quality uncached debrid anymore. Any uncached debrid, high or low quality, will only be shown as the last resort when too few results are found.
- For those that asked for more lower-end results, I have relaxed the filtering in block #8 a tiny bit so you're more likely to see a few 720p results sprinkled here and there. Don't worry, this only happens when your list isn't filled out by high quality 4K/1080p.
- Last but not least, for making it this far, I made a bonus SEL version called "Extended SEL".
  - A semi-popular request was to relax the filtering from 3 to 5, and I did just that for this version of SEL. Not only that, but I also adjusted various filtering conditions to reflect the higher amount, tweaking the Final Filter specifically to still honour the spirit of the Standard SEL. Poor quality results will only be shown when necessary.
  - In the end, this Extended SEL will keep around 20-30 streams (not including uncached usenet), compared to the Standard SEL which keeps around 15-20. This version may be ideal for those that want a guaranteed 720p result for their mobile needs, while fleshing out the rest of the higher resolution and quality with more streams (from 3 per in Standard to 5 per in Extended).
  - This Extended SEL will only be available as a separate "Extended SEL Only Template" for import, so you must import the main template first with the Standard SEL. Test that out; I'm sure you will be more than happy already with that. Otherwise, open up the template browser again and load the "Extended SEL Only" template to switch between the Standard & Extended SEL versions (nothing else will be overridden, as usual).


## ⚙️ Templates Included for AIOStreams

These are setup templates to use with AIOStreams. If you're not sure which AIOStreams instance to start with, check out the list of trusted public instances [here](https://status.dinsden.top/status/stremio-addons).

| Template | Description |
|-----------|--------------|
| **Complete Setup** | Complete configuration with filters, sort orders, streaming addons, and formatter. |
| **Without Addons** | Keeps your existing add-ons while applying the complete setup config. |
| **Without Addons & Formatter** | Applies the complete set up but keeps both your add-ons and formatter untouched. |
| **Complete Setup for P2P** | Complete setup, with p2p/http addons and sort order tailored for those without debrid service |
| **Standard SEL Only** | Imports only the Excluded Stream Expressions used in the Complete Setup. | 
| **Extended SEL Only** | Imports the modified SEL that gives slightly more results than Standard SEL. | 
| **Formatter Only** | Imports only the custom formatter used in the template for stream display. |

---

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
7. Enter your TMDB/TVDB credentials for Title Matching and various other features. Use `t0-free-rpdb` for RPDB key.
8. Load Template, Save your AIOStreams and enjoy!

  > [!NOTE]
  > Remember to personalize your imported config by going to `Filters` -> `Language`. Select your main language as the top spot in Preferred Languages, then sort/rank the rest according to your preference. I suggest keeping Dubbed, Dual Audio, Multi, Unknown in the list as they may contain streams of your preferred languages.
> To further enhance your sorting and filtering, I highly recommend importing Vidhin's regex which tags streams based on the quality of the release group. Scroll down for more information.

## 🧩 Recommended Setup for Template v1.1.0 (Outdated )
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

## 🎚️ Filtering
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

## 🎚️🤖 Filtering with Stream Expression Language (SEL)

- All filtering (besides title match and dedupe) can be done with stream expressions. My setup leaves the language filtering to AIOStreams basic language filter (altho can be done using SEL - just think the basic filter is good enough).
- There are two schools of thought with regards to SEL Filtering:
  - using it to filter during fetching stage via `Dynamic Group Exit Condition` or,
  - using it after all initial filtering has been done by AIOStreams via  `Excluded Stream Expressions` (ESE) 
- My setup has been fine-tuned using the second method for over 3 months now with feedback from users. I may incorporate the first method into my setup in the future, however for now heavy work of filtering is done using my three lines of ESE. Copy-paste the codes below into `AIOStreams -> Excluded Stream Expressions` or import them using the SEL-only template json at the top of this page.
  <p>
    <details>
        <summary>First line into ESE: Uncached Filter</summary>
  
    ```text
  /*Uncached low-seeder filter (except usenet or good regex-matched uncached)*/
  count(type(streams,'debrid','usenet')) > 0? 
  ( count(type(cached(streams),'debrid','usenet')) < 10 ? [] :  merge(count(regexMatched(streams)) > 0 ? seeders(merge(regexMatched(negate(uncached(type(streams,'usenet')), uncached(streams)),'','Bad'), type(streams,'p2p')),0,10) : seeders(merge(negate(uncached(type(streams,'usenet')), uncached(streams)), type(streams,'p2p')),0,10),
  /*Uncached resolution filter*/
  count(resolution(cached(streams),'2160p','1440p')) > 10 ? resolution(uncached(streams),'720p','576p','480p','360p','240p','144p','Unknown') :  
  count(resolution(cached(streams),'2160p','1440p','1080p')) > 10 ? slice(resolution(uncached(streams),'720p','576p','480p','360p','240p','144p','Unknown'), 5) : 
  count(resolution(cached(streams),'2160p','1440p','1080p','720p')) > 10 ? slice(resolution(uncached(streams),'576p','480p','360p','240p','144p','Unknown'), 5) : [] ) ): 
  /*P2P low-seeder filter*/	
  count(type(streams,'p2p')) < 10 ? [] : 
  count(seeders(type(streams,'p2p'), 50)) > 20? seeders(type(streams,'p2p'), 0,50) : count(seeders(type(streams,'p2p'),25)) > 20? seeders(type(streams,'p2p'), 0,25) : count(seeders(type(streams,'p2p'), 10)) > 10? seeders(type(streams,'p2p'),0,10) : count(seeders(type(streams,'p2p'), 1)) > 10? seeders(type(streams,'p2p'), 0,1) : []
     ```   
    </details>
  </p>
  <p>
    <details>
        <summary>Second line into ESE: Main Quality/Resolution Filter</summary>
  
    ```text
  /*High quality & resolution filter*/
  merge(count(resolution(quality(streams,'Bluray REMUX'),'2160p')) > 3 ? slice(resolution(quality(streams,'Bluray REMUX'),'2160p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Bluray REMUX'),'1440p','1080p')) > 3 ? slice(resolution(quality(streams,'Bluray REMUX'),'1440p','1080p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Bluray REMUX'),'720p')) > 3 ? slice(resolution(quality(streams,'Bluray REMUX'),'720p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Bluray'),'2160p')) > 3 ? slice(resolution(quality(streams,'Bluray'),'2160p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Bluray'),'1440p','1080p')) > 3 ? slice(resolution(quality(streams,'Bluray'),'1440p','1080p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Bluray'),'720p')) > 3 ? slice(resolution(quality(streams,'Bluray'),'720p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'WEB-DL'),'2160p')) > 3 ? slice(resolution(quality(streams,'WEB-DL'),'2160p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'WEB-DL'),'1440p','1080p')) > 3 ? slice(resolution(quality(streams,'WEB-DL'),'1440p','1080p'), 3) : [], 
  count(resolution(quality(streams,'WEB-DL'),'720p')) > 3 ? slice(resolution(quality(streams,'WEB-DL'),'720p','Unknown'), 3) : [],
  count(resolution(quality(streams,'WEBRip'),'2160p')) > 3 ? slice(resolution(quality(streams,'WEBRip'),'2160p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'WEBRip'),'1440p','1080p')) > 3 ? slice(resolution(quality(streams,'WEBRip'),'1440p','1080p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'WEBRip'),'720p')) > 3 ?  slice(resolution(quality(streams,'WEBRip'),'720p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'HDRip','HC HD-Rip','DVDRip','HDTV'),'1080p')) > 3 ? slice(resolution(quality(streams,'HDRip','HC HD-Rip','DVDRip','HDTV'),'1080p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'HDRip','HC HD-Rip','DVDRip','HDTV'),'720p')) > 3 ? slice(resolution(quality(streams,'HDRip','HC HD-Rip','DVDRip','HDTV'),'720p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Unknown'),'2160p')) > 3 ? slice(resolution(quality(streams,'Unknown'),'2160p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Unknown'),'1440p','1080p')) > 3 ? slice(resolution(quality(streams,'Unknown'),'1440p','1080p','Unknown'), 3) : [], 
  count(resolution(quality(streams,'Unknown'),'720p')) > 3 ? slice(resolution(quality(streams,'Unknown'),'720p','Unknown'), 3) : [] )

        )
    ```
    </details>
  </p>
  <p>
    <details>
        <summary>Third line into ESE: Low Quality/Resolution Filter</summary>
  
    ```text
  /*Low quality, resoution & "Bad" regex filter*/
  merge(count(quality(streams,'Bluray REMUX','Bluray','WEB-DL','WEBRip')) > 10 ? quality(streams,'HDRip','HC HD-Rip','DVDRip','HDTV','CAM','TS','TC','SCR','Unknown'):
  count(quality(streams,'Bluray REMUX','Bluray','WEB-DL','WEBRip','HDRip','HC HD-Rip','DVDRip','HDTV')) > 10 ? quality(streams,'CAM','TS','TC','SCR','Unknown') : [], 
  count(resolution(streams,'2160p','1440p','1080p')) > 9 ? resolution(streams,'720p','576p','480p','360p','240p','144p','Unknown'):
  count(resolution(streams,'2160p','1440p','1080p')) > 4 ? slice(resolution(streams,'720p','576p','480p','360p','240p','144p','Unknown'),5):  
  count(resolution(streams,'2160p','1440p','1080p','720p')) > 9 ? resolution(streams,'576p','480p','360p','240p','144p','Unknown') : 
  count(resolution(streams,'2160p','1440p','1080p','720p')) > 4 ? slice(resolution(streams,'720p','576p','480p','360p','240p','144p','Unknown'),5) : [],
  count(negate(regexMatched(streams,'Bad'), regexMatched(streams))) > 4 ? regexMatched(streams,'Bad') : [] )

    ```
    </details>
  </p>

The first block of SEL for your `Excluded Stream Expressions` (ESE) is the Uncached Filter. It checks and removes uncached debrid streams with low resolution and low seeder count, except for good regex-matched. It also removes P2P streams if present for low seeder count. Usenet results are exempt from this Uncached Filter.

The second block of ESE is the Main Quality/Resolution Filter. It uses `slice(...,3)` to further trim streams, keeping top ~3 results of most `Quality` and `Resolution` combination. Because AIOStreams sorts streams before SEL filtering, you can determine how your streams is sorted first so that the `slice` of the top results of any `quality` /`resolution` will always select your preferred "highest-quality streams". For me, that would be Vidhin's regex-matched streams, so I put `Regex Pattern` right underneath `Quality` in Sort Order. If you value size or language for every resolution + quality pair then put `Size` or `Language` right underneath `Quality`, the `slice` will then keep your top 3 streams for that particular `quality`/`resolution` according to your size or language preference, respectively.

Third block of ESE is the Low Quality/Resolution Filter. It checks how many streams made through the Main Filter above, and removes low quality and low resolution streams when there are already enough present. It also removes regex-matched streams from "Bad" quality release groups when there are enough non-"Bad" streams present (for those that use Vidhin's regex).

---

## ⚙️ What’s Included for AIOMetadata
These are setup configs to use with AIOMetadata. It is a powerful tool for all things metadata and catalogs. If you're not sure where to start, pick a AIOMetadata instance from [the list of trusted public instances](https://status.dinsden.top/status/stremio-addons) or use the [Elfhosted instance](https://aiometadata.elfhosted.com/configure/). You can't go wrong with either choice.

| Complete config | Description |Download|
|-----------|--------------|---|
| **With Anime Catalogs** | Complete configuration with anime metadata preset, tv, movies and anime catalogs. Huge props to Cedya, Snoak & Mr Professor for their awesome lists!|[Direct Link](https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/AIOMetadata%20Configs/AIOMetadata-config-with-anime.json)|
| **Without Anime Catalogs** | Complete configuration for tv and movies catalogs. No anime. |[Direct Link](https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/AIOMetadata%20Configs/AIOMetadata-config-without-anime.json)|

## 📥 How to Import
AIOMetadata setup configuration [For Meta/Catalogs]

- Pick one of these two json files to import into AIOMetadata following the instructions below:

   1. Integrations tab -> Obtain and enter your TMDB, TVDB, and MDBLists APIs . Use `t0-free-rpdb` for RPDB. Fanart.tv API Key is optional. Hit the `Test All Keys` button to ensure they're all ️:white_check_mark:.
   2. Configuration tab -> Import Configuration -> Import one of my json files. Feel free to edit/hide/delete various catalogs in Catalogs tab to your liking.
   3. Configuration tab -> Save Configuration -> Enter a password to save your configuration (if you haven't made an account before) -> Install the addon directly into Stremio. Note: if you encounter `AddonsPushedToAPI - Max descriptor size reached` error, try step
   4. Copy your `Install URL`. Go to [Stremthru Side Kick](https://stremthru.13377001.xyz/stremio/sidekick/), log in there using your stremio account and use the Install button there to install the addon with the AIOMetadata URL. This *may help to* bypass the `AddonsPushedToAPI - Max descriptor size reached` error, otherwise disable some catalogs to reduce size. 
- 5. For best compatibility with AIOMetadata, go to https://cinebye.dinsden.top, load up your account and remove all three Cinemeta features (Search, Catalogs, and Meta). Then scroll down to the bottom of cinebye and re-order your addons so that 1. Cinemeta 2. AIOMetadata 3. Rest of addons. Finally, save the changes by clicking `Sync to Stremio`.
- You can change your default Display Language inside AIOMetadata under General tab. Note that if you notice tv series not displaying any episode information in your language, try to upload the episodes overview for your show via tvdb directly. Alternatively you can switch the TV Series metadata provider to TMDB as it may have the missing information in your language.
