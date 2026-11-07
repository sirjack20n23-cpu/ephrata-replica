# Cities: Skylines II — Tourist District Guide (city: Barney)

Researched 2026-11-06. Every claim below comes from a linked source. I couldn't verify tourism-specific videos from Biffa, City Planner Plays, Imperator Knoedel or Phil Builds in this pass, so they are not quoted.

## Sources
- [S1] TheGamer, "How To Get More Tourists In Cities: Skylines 2": https://www.thegamer.com/cities-skylines-2-how-to-increase-attract-tourism/
- [S2] CS2 Wiki, Signature buildings: https://cs2.paradoxwikis.com/Signature_buildings
- [S3] YouTube, "Tourism Broken? Try These 5 Things": https://www.youtube.com/watch?v=gIGcYNVmWkg
- [S4] r/CitiesSkylines2, "How does tourism work?": https://www.reddit.com/r/CitiesSkylines2/comments/1qts5ic/how_does_tourism_work/
- [S5] r/CitiesSkylines2, "How tf do i get more tourists": https://www.reddit.com/r/CitiesSkylines2/comments/1wizojw/how_tf_do_i_get_more_tourists_to_my_city/
- [S6] GameRant, Policies: https://gamerant.com/cities-skylines-2-how-to-set-policies/
- [S7] GameSkinny, Best Policies: https://www.gameskinny.com/tips/cities-skylines-2-best-policies-to-use/
- [S8] CS2 Wiki, Progression: https://cs2.paradoxwikis.com/Progression
- [S9] GameFAQs walkthrough by YoyoYoshi: https://gamefaqs.gamespot.com/pc/398681-cities-skylines-ii/faqs/82369

## 1. Principles
1. **Attractiveness drives tourists.** Raise it with tourist attractions, parks, plazas and signature buildings. You can check it in Info Views > Tourism (the blue suitcase icon), where greener areas are more attractive. That panel also shows the tourist count, the weather effect and the average hotel price per night. [S1][S3][S4]
2. **Attractions** unlock through the Parks & Recreation "tourist attraction" node:
   - Observation Tower: 40 attractiveness
   - Bronze Statue: 50
   - Water Park: 75
   - Medieval Castle: 85 (needs a Paradox account)

   [S1]
3. **Signature buildings** can be built once each, for free. They are zoned buildings that need a company to occupy them and must stay profitable. Some of them raise attractiveness, for example Century Castle and Kansai Tower, and the Streamline Diner gives +1% city attractiveness. [S1][S2]
4. **Hotels aren't plopped.** They spawn in commercial zones, and only after the tourist-attraction node is unlocked. Commercial buildings that already exist won't turn into hotels. Zone new commercial land right next to your attractions. In the Tourism view, hotels show in purple. [S1][S3]
5. **The City Promotion policy** unlocks at the Small City milestone. It brings more tourists but also more crime around tourist destinations, so you need police coverage. Pre-Release Programs can help with that. [S6][S7][S8][S5]
6. **Access matters.** Link attractions with bus, tram, train and ferry. An attraction stuck behind congestion does worse than a smaller one that is well connected. [S3][S9]
7. **Tourists spend money all over the area.** They use shops, restaurants and leisure piers, not just the landmarks. [S3]

## 2. Step-by-step build plan
1. Run `save_checkpoint`.
2. Open the Tourism info view and note the current attractiveness, tourist count and hotel locations.
3. Pick one compact district for tourism. Keep it away from quiet residential areas because attractions bring traffic [S1], and away from industrial and cargo routes. (Keeping cargo out is a general traffic practice, not something these sources state.)
4. Cluster 2–3 attractions there with parks and plazas between them.
5. Ring the cluster with new commercial zoning so hotels can spawn. If none appear, bulldoze a few non-hotel commercial buildings and let the lots regrow. [S1]
6. Place any attractiveness signature buildings you've unlocked inside or near the cluster. [S2]
7. Add transit: a tram or bus loop through the district, a train or intercity station, and a ferry or harbour if you have water. [S3]
8. Turn on City Promotion and add police coverage. [S6][S7]
9. Run `simulate_step` and recheck the Tourism view. Then add more hotels where attractiveness is high. [S3]

## 3. Creator and community tips (attributed)
- **[S3] YouTube creator, "5 things":** Watch attractiveness. Build hotels where attractiveness is high. Link places with buses, ferries and trains. Remember that tourists also go to shops and piers.
- **[S4] u/1Blue2Green:** Use attractions, signatures, parks and City Promotion. The hotel spawning mechanic was broken and only partly fixed.
- **[S4] u/Hot_Weekend4399:** Tourists tend to go to the single most attractive building, so cluster your attractions and hotels together.
- **[S5] u/Mazda6GTMan:** With attractiveness maxed, lodging nearly always shows up in commercial demand.
- **[S5] u/Fabulous_Guitar4350:** Vanilla tourism is weak. The "Tourism Overhaul" mod is popular, and a tourism DLC is reportedly planned for 2027.
- **[S9] YoyoYoshi:** Connected attractions beat isolated showpieces.

## 4. Common mistakes
- Expecting old commercial buildings to become hotels. [S1]
- Spreading attractions all over the city instead of clustering them. [S4]
- Running City Promotion without enough police. [S6][S7]
- Putting attractions in residential areas or behind congestion. [S1][S9]
- Expecting the vanilla game to produce huge tourist numbers. [S5]

## 5. Paste-ready prompt for Claude Code
```
You are controlling Cities: Skylines II (city "Barney") through CitiesIIAgentBridge 0.5.0.
Goal: build a compact, transit-served tourist district and raise tourism.
Rules: Run save_checkpoint FIRST (name it "before-tourism"). Make sure no tool is active and Anarchy/ignoreErrors is OFF before building. Work in small steps and run simulate_step after each one. If something gets worse, stop and tell me.
1. Report current city attractiveness, the tourist count and where the hotels are (Tourism view data if the bridge exposes it).
2. Suggest one district location away from quiet housing and away from industrial/cargo routes. Show me the location and wait for my OK.
3. Cluster the unlocked tourist attractions (Observation Tower, Bronze Statue, Water Park, Castle if available) plus parks/plazas, and put any attractiveness signature buildings in or next to the cluster.
4. Zone NEW commercial around the cluster so hotels can spawn. If no hotels appear after simulating, demolish a few non-hotel commercial buildings there and let them regrow.
5. Use get_network_edges to check traffic on roads into the district. Add a tram or bus loop and connect it to the train, intercity bus and ferry stations. Use upgrade_network (e.g. trees/wide sidewalks) on the main tourist street. Keep heavy cargo traffic out of the district.
6. Enable the City Promotion policy and make sure there is police coverage in the district.
7. Run simulate_step and report before/after attractiveness, tourist count and hotel count, then list the next steps.
Use relocate_building rather than demolish when something is badly placed.
```
