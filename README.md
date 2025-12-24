# SEL-Filtering-and-Sorting
  Since beta of the current v2 AIOStreams, I always wanted the perfect config that would show only the best results, not just for myself, but for my family and friends too. Some prefer lower quality/resolution streams for their lower end TV, while I prefer the latest remuxes. An all-in-one config that I could share. We didn't want to see dozens of results we would never use anyway. This balancing act was finally possible with the release of [Stream Expressions Language (SEL)](https://github.com/Viren070/AIOStreams/wiki/Stream-Expression-Language) capability in AIOStreams.  

  After months of tinkering, always finetuning my setup, testing newest features and bugs, and exchanging numerous tips with you all over at [the AIOStream discord,](https://discord.gg/zRq8dVh5rJ) I will use this space to share some of that with you. You will find a guide to my personal config that I'm happy with and importable templates for AIOStreams, with a heavy focus on SEL as the filtering tool. It is tailored to my preference, but has broadened over time so that most of you should find it as a good starting point, if not the final piece to your stremio puzzle.

Consider using my setup template as is if you:
- Don't want to see too many results, but also don't want to miss out on any high quality streams that may be removed by simple filtering like `Exclude Uncached` or `Result Limits`
- Want a config that will adjust its filtering strictness based on how many results are found. Dont want those old 480p cartoons filtered out? But don't want to see 720p streams when there are a bunch of 1080p & 4K present already? This setup got you covered. 
- Have spent hours trying to prioritize certain streams and sort your results to your liking but still not quite happy yet. This is the right place for you, as I can guarantee to have spent more hours on this setup, and it's free, so why not try?

Lastly, please do note my AIOStreams template *does not* include any catalogs. This is because many of us prefer AIOMetadata (separate addon from AIOStreams), so just scroll to the end of this page, you will find my config for AIOMetadata for all your metadata and catalog management needs.


  <details>
        <summary>PS. I just switched to TorBox on their BF deal, so now I can share my code like everyone else woo!! For the best stremio experience, you need a debrid service, and TB is current top recommendation especially if you're like me, and like to share your stremio setup with family and friends. </summary></summary>
  
    f1cdd3f8-aeee-48f1-849b-64fc7e5aeb3c
  > Use my [referral](https://torbox.app/subscription?referral=f1cdd3f8-aeee-48f1-849b-64fc7e5aeb3c) and we both get +84 days on a yearly sub, if it's your first ever purchase.
    
  </details>

---
## ✨ Release Notes
Nov 18, 2025: What's new in template v1.2.0
- Major SEL Engine Rewrite: Went from 3 to 8 filtering blocks for granular control.
- Independent Cached/Uncached Filtering: Separates slice(streams, 3) into slice(cached(streams),3) and slice(uncached(streams,3).
- Revamped Sort Order: Introduces Stream Expressions Matched for nuanced stream ranking.
- Library Stream Exemption: Library streams are no longer counted against the filter limits.
- Revamped Formatter: New icons & Smarter icon auto-switching.
- Bonus: Introduction of the Extended SEL for those who want more results.

<details>
<summary>Full release notes (v1.2.0)</summary>
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

These are setup templates to use with AIOStreams. If you're not sure which AIOStreams instance to start with, check out the list of trusted public instances [here](https://status.dinsden.top/status/stremio-addons). I recommend nightly AIOStreams from Midnight, Yeb, Viren, or Kuu. Make sure they have working Torrentio add-on. If not, switch to a different instance.

| Template | Description |
|-----------|--------------|
| **Complete Setup** | Complete configuration with filters, sort orders, streaming addons, and formatter. |
| **Without Addons** | Keeps your existing add-ons while applying the complete setup config. |
| **Without Addons & Formatter** | Applies the complete set up but keeps both your add-ons and formatter untouched. |
| **Complete Setup for P2P** | Complete setup, with p2p/http addons and sort order tailored for those without debrid service |
| **Standard SEL Only** | Imports only the Excluded Stream Expressions used in the Complete Setup. | 
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
7. Enter your TMDB/TVDB credentials for Title Matching and various other features. Use `t0-free-rpdb` for RPDB key.
8. Load Template, Save your AIOStreams and enjoy!

  > [!NOTE]
  > Remember to personalize your imported config by going to `Filters` -> `Language`. Select your main language as the top spot in Preferred Languages, then sort/rank the rest according to your preference. I suggest keeping Dubbed, Dual Audio, Multi, Unknown in the list as they may contain streams of your preferred languages.
> To further enhance your sorting and filtering, I highly recommend importing Vidhin's regex which tags streams based on the quality of the release group.

> How to Import Vidhin's regex: Filters → Regex → Preferred Regex Patterns → Import from url icon
> ```https://raw.githubusercontent.com/Vidhin05/Releases-Regex/main/merged-anime-regexes.json```
> 
> This regex labels all streams with tier rankings based on reputation/quality of release groups per TRaSH Guides. You will need to reimport the regex occassionally whenever Vidhin pushes an update to the regex because many AIOStreams instances will only allow the use of his latest update regex.

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
| **With Anime Catalogs** | Complete configuration with anime metadata preset, TV, movies, and anime catalogs. Huge props to Cedya, Snoak \& Mr Professor for their awesome lists! | [Direct Link](https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/AIOMetadata%20Configs/AIOMetadata-config-with-anime.json) [Mobile Link](https://www.dropbox.com/scl/fi/7mfhmz9ptddrqtfqegqeu/Tamtaro-aiometadata-config-with-anime.json?rlkey=3jdolg2pjyxf1ju5axg5vnoiu&st=gzvkid51&dl=1)|
| **Without Anime Catalogs** | Complete configuration for TV and movies catalogs. No anime. | [Direct Link](https://raw.githubusercontent.com/Tam-Taro/SEL-Filtering-and-Sorting/refs/heads/main/AIOMetadata%20Configs/AIOMetadata-config-without-anime.json) [Mobile Link](https://www.dropbox.com/scl/fi/cl6b1z4nof1150aagpoa8/Tamtaro-aiometadata-config-without-anime.json?rlkey=d95gxqr85tna3ujqa933zzhj7&st=wsekixtd&dl=1)|

## ✨ Release Notes

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
3. **Configuration tab** → Save Configuration → Enter a password to save your configuration (if you haven't made an account before) → Install the addon directly into Stremio. **Note**: If you encounter `AddonsPushedToAPI - Max descriptor size reached` error, try step 4.
4. Copy your `Install URL`. Go to [StremThru Side Kick](https://stremthru.13377001.xyz/stremio/sidekick/), log in there using your Stremio account and use the Install button there to install the addon with the AIOMetadata URL. This *may help* bypass the `AddonsPushedToAPI - Max descriptor size reached` error; otherwise disable some catalogs to reduce size or see below "How to set up 2 AIOMetadata for more catalogs".
5. For best compatibility, go to [https://cinebye.dinsden.top](https://cinebye.dinsden.top), load up your account and remove all three Cinemeta features (Search, Catalogs, and Meta). Then scroll down to the bottom of Cinebye and re-order your addons so that 1. `Cinemeta` 2. `AIOMetadata` 3. `Rest of addons (AIOStreams, subtitle addons, etc)`. Finally, save the changes by clicking `Sync to Stremio`. The re-ordering can also be done inside StremThru Side Kick under "Move" mode.

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
