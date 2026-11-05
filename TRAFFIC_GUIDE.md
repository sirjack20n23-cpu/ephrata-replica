# TRAFFIC_GUIDE.md: Traffic and road rules for Claude (Cities: Skylines II, city "Barney")

Who this is for: you are Claude Code, controlling Barney through the CitiesIIAgentBridge mod. You can place roads and zoning and read the map. You cannot terraform. Every rule below comes from transcripts of the creators' videos (pulled Nov 2026) and credits its source. [S#] points to the source list at the end. If a rule needs a tool the bridge doesn't have (traffic-light toggles, lane connector, Traffic mod, road upgrades), tell the user which in-game tool to use and let them do it.

## 1. Road hierarchy (the core rule)
- There are four classes: **highway → arterial → collector → local**. Highways are for mobility and local roads are for access. In suburbs the hierarchy is followed strictly: highway → arterial → collector → local. [S1 City Planner Plays]
- **Each class connects only to the class directly above or below it.** Arterials connect only to highways, other arterials and collectors. [S7 TotalXclipse] Local roads should not meet arterials, and collectors should not meet highways. Connect the highway to the city **only through strong arterials**. [S9 YUMBL]
- Lane count doesn't decide a road's class. Its connections do. Add lanes for capacity, but a road is an arterial because of what it connects to. [S9 YUMBL]
- Arterials collect traffic from districts and carry it to the highway. Keep intersections on them few, except with other arterials or collectors. Few services should front an arterial. [S6 T4rget]
- **Space arterial junctions far apart.** Too many junctions close together stall traffic, and the jam backs into the city or onto the highway. [S7 TotalXclipse] Leave enough distance between intersections, and give drivers several routes to the same place. [S13 NunoLac]
- Put trip-generating buildings (a clinic, for example) on a **local road just off the arterial**, not on the arterial itself. [S1]
- Speed limits in CS2: one-lane one-way highways (ramps) are 80 km/h, and other highway pieces are 120 km/h. You can't change them. [S1]
- A parallel arterial that mirrors the highway takes pressure off the highway. [S9] Timeister also used an exit onto a parallel 4-lane road to balance load, and warned that "more lanes doesn't necessarily mean more better." [S11]
- For Ephrata, map the real roads to these classes: US-222/US-322 as highway or arterial, Main St (PA-272) and State St as arterials or collectors, residential streets as local.

## 2. Intersections, signals, stop signs
- By default, medium and large roads get a **signal at every intersection**. Remove signals along arterials so the higher-class road flows freely (right-click with the traffic-light tool). [S1] Where one or two 2-lane roads meet a large road, signals usually aren't worth it. Use stop signs instead. Keep signals only where big roads meet. [S12 Ollotize]
- A stop sign in vanilla applies to **every leg** of the junction. You can't pick which legs get one. [S1]
- Avoid large 4-lane and 6-lane four-way junctions. [S12] Four-way junctions are a common cause of traffic problems. If you can't avoid one, signal it or make it a roundabout. [S13]
- **Add lanes where a road enters a junction**, or use asymmetric roads, so turning traffic gets dedicated lanes and queues shrink. [S12] Real Civil Engineer used a 3-lane asymmetric road where two lanes fed into one. [S4]
- Use T-junctions and slip roads to get traffic on and off collectors and arterials. [S7]
- Ban turns that cause trouble. City Planner Plays restricted left turns at a messy spot. [S2] Biffa removed U-turns and unsafe crossings, and set dedicated turn lanes with the lane connector tool. [S14]
- Custom signal phases (from mods or newer versions): Biffa used split phasing, exclusive pedestrian phases and weighted phases, and said they solved his worst junction. [S14] Mike Builds paired a fixed lane-connection setup with "smart traffic lights." [S16]

## 3. Roundabouts
- A well-placed roundabout eases congestion. Fewer signals often help more than adding them. [S13]
- If a roundabout is backing up, **make it bigger and add lanes**. "That usually helps." [S10 Erdgeist]
- Don't run a highway straight into a roundabout. Real Civil Engineer saw traffic jam when a highway emptied into one. [S4] A turbo roundabout keeps lanes separate so drivers can't change lanes mid-circle. [S4]
- Approach capacity limits a roundabout. YUMBL's teardrop/dumbbell interchanges worked "fine to a point" and then backed up. [S8]
- Trams cut straight through the middle of a roundabout, so choose one or the other at a junction. [S15 RedGingerRR]

## 4. Highways, ramps and interchanges
- Every on-ramp or off-ramp between the highway and city streets is a **service interchange**. Freeway-to-freeway connections are system interchanges. [S8 YUMBL]
- Small interchanges for low traffic: dumbbell or dog-bone (two roundabouts on either side). Every lane gets used, but it has yields. YUMBL likes the single-point interchange but criticizes its many back-to-back merges. [S8]
- Merging: everyone merges first, and the road splits afterward, with no merge just before a split. Real Civil Engineer built this layout to prevent bottlenecks. [S4] Adding a lane to the on-ramp made merging easier. [S10] Upgrade to 3 lanes around on- and off-ramps. [S10]
- Exits: the turning traffic takes the first lane and the other lanes carry on, so build exits as a lane drop. [S5 TotalXclipse]
- Lane math doesn't have to be perfect. A middle lane can be "choose your own adventure." [S2 City Planner Plays] Still, YUMBL found lanes that went unused because the lane math was wrong, and fixed them by cutting the road to one lane. [S8]
- Cross highways with bridges or tunnels, not at-grade crossings. [S7]
- Avoid four-way interchanges onto arterials from the only highway into town. [S6 T4rget]

## 5. Parking
- Parking is "a very important topic in this game." [S1] Adding a parking facility fixed severe traffic in City Planner Plays' episode "I Fixed Crushing Traffic with PARKING". He also built parking ramps and accessible/EV parking. [S3]
- The wide-sidewalk upgrade removes street parking. So do a grass median or trees, which Timeister added to stop vehicles parking. [S1][S11]
- Parking lots right on a main street block traffic as cars cross to enter and leave. Use the **divider/median upgrade**. [S7]
- When Biffa added small parking lots in a dense skyline, a broken-down bus blocked a lane. Give buses room. [S17]

## 6. Cargo and trucks
- Trucks queueing to turn left into a cargo train terminal backed up the road. Timeister made the entry road **one-way**, so trucks loop counter-clockwise, and connected an unconnected cargo station to the outside rail. Without that link it's just storage. [S11]
- City Planner Plays hit a bug: without a normal cargo vehicle available, a depot won't spawn trucks. [S3]

## 7. Transit to cut traffic
- Use layered transit: buses for local trips, trams for medium density, metros across the city, trains for long trips. Connect bus routes to tram and metro stations, and leave room to expand. Too many buses clog roads. [S15 RedGingerRR]
- Buses stuck in traffic can't keep to schedule. City Planner Plays added bus turn lanes and bus lanes. [S2] He also made transit fare-free to boost ridership. [S3] A 3-lane small road can carry a bus/taxi lane. [S7]
- Timeister fixed gridlock from a highway connection at the map edge by adding a bus network in the town that had no transit. [S11]

## 8. How Claude applies this
When building, including the Ephrata replica (with EPHRATA.md):
1. Classify every road in `ephrata_roads.json` as highway, arterial, collector or local before placing it. Place arterials first, then collectors, then local roads.
2. Check every connection against the hierarchy. If a local road would meet an arterial, reroute it through a collector, or flag it to the user when real Ephrata does it (historic grids often break the rules; keep fidelity, but list each one).
3. Keep at least ~3-4 blocks between arterial junctions where the real map allows. Merge closely spaced junctions, or turn minor legs into right-in/right-out.
4. Put trip-heavy zones (commercial, industrial) on collectors or on local roads next to arterials, not directly on the highway approach.
5. After placing roads, give the user a checklist: remove default signals along arterials, add stop signs on minor junctions, add a median wherever on-street parking hurts.
6. Highway ramps: one-lane ramps, an added lane at each merge, merges before splits, and no highway ending in a roundabout.

When diagnosing traffic (read the map and traffic data, then report before changing anything):
1. Find the jam's head, not its tail. Look for short junction spacing, a lane drop, a merge just before a split, a highway ending in a roundabout, trucks turning left into a depot, or a roundabout over its approach capacity.
2. Fix in this order: remove or replace signals, add turn lanes, ban turns or U-turns, enlarge the roundabout, add a parallel route, add parking, add transit, then rebuild the interchange.
3. Make one change at a time, run the game for a few in-game hours, then compare.
4. Ask the user before any demolition or spending.

## Sources (transcripts fetched 2026-11-05)
- S1 City Planner Plays, The Ultimate Beginner's Guide to Road Building in CS2 (UBG #2): https://www.youtube.com/watch?v=iaLcR3hYt7U
- S2 City Planner Plays, Crushing Traffic Like a Highway Engineer (MC #22): https://www.youtube.com/watch?v=Tn-gm4SXQFQ
- S3 City Planner Plays, I Fixed Crushing Traffic with PARKING?!: https://www.youtube.com/watch?v=2OdT43uTy0I
- S4 Real Civil Engineer, Testing VIRAL highway layouts in CS2: https://www.youtube.com/watch?v=7i1DXP1zm40 ; also Is this the BEST HIGHWAY INTERSECTION in CS2?: https://www.youtube.com/watch?v=asEXZlZnpgw
- S5 TotalXclipse, 3 Efficient and Easy To Build Intersections in CS2: https://www.youtube.com/watch?v=ufTnEEwr_EM
- S6 T4rget, City Layout Tips: Using Road Hierarchy: https://www.youtube.com/watch?v=hsaOYqztdEg
- S7 TotalXclipse, How To Use Road Hierarchy To Create Perfect Layouts in CS2: https://www.youtube.com/watch?v=JUXgwm-B8So
- S8 YUMBL, Understanding the Power of Service Interchanges in CS2: https://www.youtube.com/watch?v=Aja9gNzgJaw
- S9 YUMBL, FIX TRAFFIC ... with Road Hierarchy - CS2: https://www.youtube.com/watch?v=AwnLb2g6iKI
- S10 Erdgeist, Building a Perfect Highway System in CS2: https://www.youtube.com/watch?v=GzgnLOQN5tY
- S11 Timeister, Fixing the WORST Traffic Gridlock Ever | CS2: https://www.youtube.com/watch?v=mLfWa1dnXLg
- S12 Ollotize Gaming, 5 Quick Traffic Flow Tips | CS2: https://www.youtube.com/watch?v=NuTWRQWFCC4
- S13 NunoLac Games, Cities Skylines 2: How to fix Traffic: https://www.youtube.com/watch?v=egyZjU7Bk54
- S14 Biffa Plays Indie Games, Custom Traffic Light Phases Just SOLVED Our Biggest Problem: https://www.youtube.com/watch?v=4ctLtl780Kw
- S15 RedGingerRR, Master Public Transit Hierarchy in CS2: https://www.youtube.com/watch?v=Lt_n9gOjZBc
- S16 Mike Builds, The ONLY Intersection You Need | CS2: https://www.youtube.com/watch?v=F5_vHJT6DVs
- S17 Biffa Plays Indie Games, Tricks for a Traffic Free High Density Skyline in CS2: https://www.youtube.com/watch?v=5UIkBRhyvi8

Note: the rules are paraphrased from auto-generated captions. Short phrases in quotes are verbatim. Section 8's spacing number (~3-4 blocks) is my own rule of thumb, not a creator's.
