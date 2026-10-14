# FitFindr

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, every
> command, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> python app.py listings --full -n 6      # read the data (Milestone 1)
> python app.py fields                    # what you can filter on
> python app.py ask 'vintage graphic tee under $30'
> ```
>
> All three tools are stubs, so that last command will do nothing useful yet.
> That's the starting position.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     This is your submission. Fill each section in as you finish the milestone
     it belongs to — don't leave it all to the end.

     Unit 3 asks for the first five sections. Unit 4 adds the five below them.
     Leave the unit 4 sections alone until then; they're here so you know
     what's coming.

     Everything is pasted as TEXT. No screenshots, no images, no video links.
     A typed block of output gets full credit; a picture of the same output
     gets none.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 3 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Three or four sentences: what a user asks for, and what they get back. -->

FitFindr helps you shop secondhand by turning a plain request like "vintage graphic tee under $30" into a short list of matching thrift listings, filtered by size and price when you give them. It picks the best match and suggests one or two outfits built from clothes you already own, or general styling ideas if your wardrobe is empty. Finally, it writes a short, casual caption you could post with a photo of the fit, naming the item, its price, and where you found it. If nothing matches your search, it tells you so instead of making up an item.



---

## Tool Inventory

<!-- Four lines per tool. This is worth 2 points and it's the single most
     common place students lose them.

     "Returns a list" earns NOTHING. The description has to say what is IN
     the list.

     The empty case isn't optional either — it's the thing your loop branches
     on, and if you don't decide it here you'll discover it as a crash in
     Milestone 5. -->

### `search_listings`

- **What it does:** The search_listings tool searches the listing files and return items matching a description
- **Inputs:** 
     - description  (str)
     - size         (str)
     - max_price    (float)

<!-- name and type each: `max_price` (float), not "a price" -->
- **Returns:** It returns a list of items that match the given description input. The best match will be return first.
- **When it has nothing:** When no items are matched, it returns an empty list
### `suggest_outfit`

- **What it does:** It will take a thrifted item and the user's wardrobe and then give one or two outfit suggestions
- **Inputs:** 
     - new_item     (listing dict)
     - wardrobe     (dict)
- **Returns:** Return a non-empty string with outfit suggestions.
- **When it has nothing:** When no outfit ideas are suggested, it returns a general styling advice

### `create_fit_card`

- **What it does:** writes a short caption someone would actualy post
- **Inputs:**
     - outfit (dict)
     - new_item (dict)
- **Returns:** It returns a 2-4 sentence caption
- **When it has nothing:** If `outfit` is empty or whitespace, return a descriptive message 
---

## Planning Loop

<!-- Your branch rule, stated as a rule — the condition AND both paths — plus
     the file and function that holds it.

     Like this:
       "If search_listings returns an empty list, put a message in the session
        and stop. Otherwise take the first result and go to suggest_outfit."
        — agent.py::run_agent

     The grader checks your code against what you claim here, so the file and
     function have to be real. -->

**Branch rule:** If search_listings returns an empty list, I put a message in `session["error"]` saying what to change (budget, size, or wording) and stop. Otherwise I take the first result and go to suggest_outfit.

**Where it lives:** `agent.py::run_agent`

**How the query is parsed:** Regex in `parse_query()`: a dollar amount becomes max_price, "size X" becomes size, and the remaining words minus filler become the description.

**What moves through the session:** `parsed` → `search_results` → `selected_item` → `outfit_suggestion` → `fit_card`, with `error` set instead if the search comes back empty.

---

## Sample Run

<!-- Two things go here.

     1. One FULL query and its output, pasted as text.
     2. Your three per-tool terminal tests — the command and what it printed. -->

**One full query**

```
$ python app.py ask 'vintage graphic tee under $30'

```

```

  Found:    Graphic Tee — 2003 Tour Bootleg Style — $24.0 on depop

  Outfit:   **Outfit 1: 90s Grunge Casual**
Pair the Graphic Tee — 2003 Tour Bootleg Style with your Baggy straight-leg jeans, dark wash, and finish with Black combat boots. 

**Outfit 2: Streetwear Layering**
Wear the Graphic Tee — 2003 Tour Bootleg Style layered under the Vintage black denim jacket, paired with the Wide-leg khaki trousers andChunky white sneakers.

  Fit card: Scored this sick Graphic Tee — 2003 Tour Bootleg Style online and I’m honestly obsessed. I threw it on with baggy dark wash denim and chunky boots for that effortless 90s grunge look. It has the absolute best worn-in feel for everyday streetwear.
```

**The three tools, tested one at a time**

```
$ python -c "from tools import search_listings; print(search_listings('graphic tee', max_price=30))"

```
```
[{'id': 'lst_002', 'title': 'Y2K Baby Tee — Butterfly Print', 'description': 'Super cute early 2000s baby tee with butterfly graphic. Fitted crop length. Tag says medium but fits like a small.', 'category': 'tops', 'style_tags': ['y2k', 'vintage', 'graphic tee', 'cottagecore'], 'size': 'S/M', 'condition': 'excellent', 'price': 18.0, 'colors': ['white', 'pink', 'purple'], 'brand': None, 'platform': 'depop'},{'id': 'lst_006', 'title': 'Graphic Tee — 2003 Tour Bootleg Style', 'description': 'Vintage-style bootleg tee with faded graphic. Slightly boxy fit. 100% cotton, soft and worn-in.', 'category': 'tops', 'style_tags': ['graphic tee', 'vintage', 'grunge', 'streetwear', 'band tee'], 'size': 'L', 'condition': 'good', 'price': 24.0, 'colors': ['black'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_017', 'title': 'Mesh Long-Sleeve Top — Black', 'description': 'Sheer black mesh long-sleeve. Great for layering under a graphic tee or over a bralette. Stretchy material, fits true to size.', 'category': 'tops', 'style_tags': ['y2k', 'grunge', 'goth', 'layering'], 'size': 'S/M', 'condition': 'excellent', 'price': 15.0, 'colors': ['black'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_033', 'title': 'Vintage Band Tee — Faded Grey', 'description': 'Faded grey band-style tee with distressed graphic. Crew neck. Fits boxy. Well-loved but no holes or major damage.', 'category': 'tops', 'style_tags': ['vintage', 'grunge', 'band tee', 'graphic tee', 'streetwear'], 'size': 'L', 'condition': 'fair', 'price': 19.0, 'colors': ['grey', 'charcoal'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_011', 'title': 'Low-Rise Cargo Pants — Khaki', 'description': 'Y2K era low-rise cargo pants. Lots of pockets. Khaki color, slightly distressed at the hems. Great for layering with a long tee.', 'category': 'bottoms', 'style_tags': ['y2k', 'cargo', '2000s', 'streetwear'], 'size': 'W29', 'condition': 'fair', 'price': 27.0, 'colors': ['khaki', 'tan'], 'brand': None, 'platform': 'poshmark'}, {'id': 'lst_015', 'title': 'Vintage Graphic Hoodie — Faded Black', 'description': 'Faded black pullover hoodie with barely-visible vintage graphic on the chest. Cozy interior. Some pilling but adds to the worn-in look.', 'category': 'tops', 'style_tags': ['vintage', 'grunge', 'graphic', 'streetwear'], 'size': 'L', 'condition':'fair', 'price': 26.0, 'colors': ['black', 'charcoal'], 'brand': None, 'platform': 'depop'}]
```

```
$    python -c "from tools import suggest_outfit; from utils.data_loader import get_example_wardrobe, load_listings; print(suggest_outfit(load_listings()[0], get_example_wardrobe()))"

```
```
**Off-Duty Prep**
Pair the Vintage Levi's 501 Jeans with the white ribbed tank top and the oversized grey crewneck sweatshirt layered over your shoulders, finished with chunky white sneakers and the black crossbody bag. 

**Denim on Denim**
Style the Vintage Levi's 501 Jeans w
```
```
$ python -c "from tools import create_fit_card; from utils.data_loader import load_listings; print(create_fit_card('jeans and white sneakers', load_listings()[0]))"


```
```
Scored these Vintage Levi's 501 Jeans online and I'm honestly obsessed with how the medium wash breaks in. Paired them with crisp white sneakers for that effortlessly cool, running-errands-in-the-city streetwear aesthetic. Nothing beats finding classic denim that actually fits like a glove right out of the package.
```

---

## How I Used AI

<!-- Two specific moments. What you asked, what came back, what you changed.

     "I used Claude to help me code" is not enough.

     "I gave Claude my search_listings spec. It returned None on no match
     instead of an empty list, so I changed it" is the level we want. -->

**Moment 1**

- *What I asked for:* I asked Claude to check whether my search_listings returned an empty list.
- *What came back:* It confirmed every no-match case returned an empty list, but it also pointed out that my scoring only used the description field and didn't strip punctuation from the user's query, so "tee," would not match "tee".
- *What I changed:* I kept the description-only matching on purpose because that was my spec, and had the query cleaned the same way as the listing text so punctuated searches return the same results as plain ones.

**Moment 2**

- *What I asked for:* I asked Claude to guide me through suggest_outfit step by step and check my code as I wrote it instead of handing me the full solution.
- *What came back:* Its review caught that I was calling ", ".join() on a wardrobe item's category, which is a string, so the prompt would have listed "b, o, t, t, o, m, s" instead of "bottoms".
- *What I changed:* I removed the join and used the category string directly, and kept the final prompt's rule that the model may only name pieces from my wardrobe list so it can't invent clothes I don't own.

<!-- ═══════════════════════ UNIT 4 — THE TEST ═══════════════════════

     Don't fill these in during unit 3.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Run Log — Before

<!-- Five criteria, five tries each, in this exact format.

     Five, because your criteria are written out of five. Mark each try PASS
     or FAIL, count the passes, and read that count against your target — a
     row targeting 4 of 5 with three PASS cells is MISSED (3/5).

     `python run_eval.py --label before` runs everything and writes the table
     into results/. Paste it here and fill in the verdicts. -->

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1. Matching query completes all three tools | 4 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 2. Impossible query stops before suggest_outfit | 5 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 3. Selected item reaches suggest_outfit and create_fit_card unchanged | 5 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 4. Fit card is 2–4 sentences with the real price and platform | 4 of 5 | FAIL | FAIL | FAIL | FAIL | FAIL | MISSED (0/5) |
| 5. Every search result is at or under max_price | 5 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |

**Real output from one try**, pasted as text, naming the file and function
that produced it:

```
  query: vintage graphic tee under $30
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **Monochrome Grunge** Pair the Graphic Tee — 2003 Tour Bootleg Style with your baggy straight-leg jeans, dark …
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**Monochrome Grunge**\nPair the Graphic Tee — 2003 Tour …
      out: Scored this sick 2003 Tour Bootleg Style online and honestly haven't taken it off since it arrived. I'm leanin…
  try 1: completed — fit card 301 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **Off-Duty Grunge** Layer the Graphic Tee over the white ribbed tank top and pair it with the baggy straight-l…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**Off-Duty Grunge**\nLayer the Graphic Tee over the whit…
      out: Scored this vintage 2003 tour bootleg style graphic tee on Depop and I am officially obsessed. I love layering…
  try 2: completed — fit card 338 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **90s Grunge Casual** Wear the graphic tee with your baggy straight-leg jeans, black combat boots, and the bla…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**90s Grunge Casual**\nWear the graphic tee with your ba…
      out: Scored this 2003 tour bootleg style graphic tee on Depop and I am officially obsessed with the faded wash. Thr…
  try 3: completed — fit card 294 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **90s Streetwear** Pair the Graphic Tee — 2003 Tour Bootleg Style with baggy straight-leg jeans, dark wash and…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**90s Streetwear**\nPair the Graphic Tee — 2003 Tour Boo…
      out: Scored this sick Graphic Tee — 2003 Tour Bootleg Style online and I’m already obsessed with the washed-out con…
  try 4: completed — fit card 300 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **90s Grunge Casual** Wear the Graphic Tee — 2003 Tour Bootleg Style with your Baggy straight-leg jeans, dark …
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**90s Grunge Casual**\nWear the Graphic Tee — 2003 Tour …
      out: Scored this sick Graphic Tee — 2003 Tour Bootleg Style online and I'm obsessed with how heavy the fade is. I w…
  try 5: completed — fit card 281 chars

impossible query stops early  (example wardrobe)
  query: designer ballgown size XXS under $5
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 1: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 2: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 3: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 4: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 5: stopped early — Nothing matched "designer ballgown". You could raise your bu

empty wardrobe  (empty wardrobe)
  query: denim jacket under $50
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **The 90s Streetwear Double** Layer your cropped denim jacket over a simple black graphic tee and baggy black …
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**The 90s Streetwear Double**\nLayer your cropped denim jac…
      out: Found this amazing light wash jacket on Poshmark and I'm already obsessed with how it fits. I went for a 90s s…
  try 1: completed — fit card 249 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **Denim on Denim**   Pair your cropped Wrangler jacket with dark indigo straight-leg jeans to create a high-co…
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**Denim on Denim**  \nPair your cropped Wrangler jacket wit…
      out: Scored this amazing light wash, cropped denim jacket on Poshmark and I am so obsessed with how it fits. I pair…
  try 2: completed — fit card 275 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **Double Denim Streetwear** Pair the light wash cropped jacket with medium-wash straight-leg jeans and a crisp…
  [rate limit] 15 requests used this minute. Waiting 2s. This is normal.
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**Double Denim Streetwear**\nPair the light wash cropped ja…
      out: Scored this light wash cropped denim jacket on Poshmark and it’s already the MVP of my closet. I love playing …
  try 3: completed — fit card 322 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **1. Double Denim Streetwear**   Pair the light wash jacket with black or dark indigo straight-leg jeans to cr…
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**1. Double Denim Streetwear**  \nPair the light wash jacke…
      out: Scored this vintage light wash jacket on Poshmark and I am already obsessed with the fit. Threw it on over an …
  try 4: completed — fit card 236 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
  [rate limit] 15 requests used this minute. Waiting 1s. This is normal.
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **1. Monochrome Streetwear** Pair the light wash cropped jacket with matching light-wash vintage denim jeans a…
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**1. Monochrome Streetwear**\nPair the light wash cropped j…
      out: Scored this light wash cropped jacket on Poshmark and honestly, I'm obsessed with how soft it is. I paired it …
  try 5: completed — fit card 263 chars
```

---

## Verdicts and Diagnoses

<!-- MET or MISSED per criterion against LAST UNIT's target, plus a sentence on
     how you decided.

     Then, for every miss: which of the four places it happened — a tool, the
     loop's branch, the session, or the model's output — AND the mechanism.

     Not a diagnosis:  "The fit card was bad."
     A diagnosis:      "The fit card criterion missed on 2 of 5 items. Both had
                        an empty brand field. My prompt puts the brand in the
                        first sentence, so the card opened with a blank and read
                        like a fragment. The tool worked; the prompt assumed a
                        field that isn't always there."

     Look for a pattern. Three misses on the same tool is one problem, not
     three. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Matching query completes all three tools | 4 of 5 | MET (5/5) | All five tries of "vintage graphic tee under $30" made it to a fit card without stopping early. |
| 2 | Impossible query stops before suggest_outfit | 5 of 5 | MET (5/5) | Every ballgown try stopped at 0 results with a message telling me to raise the budget, change the size, or use broader words. |
| 3 | Selected item reaches suggest_outfit and create_fit_card unchanged | 5 of 5 | MET (5/5) | The trace showed the same Graphic Tee title in the select, outfit, and fit card steps every time. |
| 4 | Fit card is 2–4 sentences with the real price and platform | 4 of 5 | MISSED (0/5) | None of the five cards mentioned the $24 price, and three said "online" instead of Depop. |
| 5 | Every search result is at or under max_price | 5 of 5 | MET (5/5) | I checked all 8 results for the "under $30" search and every price was $30 or less. |

**Diagnoses**

**Criterion 4 — fit card missing the price and platform (MISSED, 0/5).** The search, loop, and session all passed the right item along, so the problem was in `tools.py::create_fit_card`: its system prompt got garbled to "Mention the itetform exactly once each," so the model was never really told to include the price and platform. That's why every single card left out the $24 and most just said "online" instead of Depop. It's one bug, not five, and fixing that one line in the prompt should fix it.


---

## Loop Trace

<!-- One full run, printed step by step, with the MCP call visible in it.

     `python app.py ask '...' --trace` once you've added the trace.step()
     calls in Milestone 2.

     Worth pasting BOTH the happy path and the empty-search path. The empty
     one should be visibly shorter, because it stops. If your two traces are
     the same length, your branch isn't working — and this is the fastest way
     anyone will ever find that out. -->

**Happy path**
```
$ python app.py ask 'what goes with white shorts?' --trace
```
```
[1] parse_query
      in:  'what goes with white shorts?'
      out: {'description': 'what goes white shorts', 'size': None, 'max_price': None}
[2] search_listings (via MCP)
      in:  {'description': 'what goes white shorts', 'size': None, 'max_price': None}
      out: 5 items: High-Waisted Denim Shorts — Cutoff, Platform Sneakers — White Chunky Sole, Vintage Polo Shirt — Forest Green … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: High-Waisted Denim Shorts — Cutoff ($24.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='High-Waisted Denim Shorts — Cutoff', wardrobe_items=10
      out: **Casual Summer Contrast** Pair the high-waisted denim shorts with the white ribbed tank top and the vintage b…
[5] create_fit_card
      in:  item='High-Waisted Denim Shorts — Cutoff', outfit='**Casual Summer Contrast**\nPair the high-waisted denim sho…
      out: Scored these vintage high-waisted denim shorts on Poshmark and I am already living in them all summer long. Th…

```


**Empty search**

```
[1] parse_query
      in:  'how much is an orange?'
      out: {'description': 'how much is orange', 'size': None, 'max_price': None}
[2] search_listings (via MCP)
      in:  {'description': 'how much is orange', 'size': None, 'max_price': None}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit

  Nothing matched "how much is orange". You could use broader words, like 'tee' or 'jacket' instead of a specific style.
```

**On the MCP move:** <!-- what changed in your code, and whether anything
behaved differently afterwards. If the rewire didn't work, say exactly where it
broke — the error text and the last thing that worked. That earns the point in
full. -->



---

## The Improvement

<!-- What you changed, why your diagnosis pointed at it, and the after-run in
     the same table format. One change, measured properly.

     `python run_eval.py --label after` -->

**What I changed:** I fixed the garbled line in the system prompt in `tools.py::create_fit_card` so it now tells the model to mention the item, its exact price (like $24), and the platform once each, and I made the prompt send the price as `$24` instead of `$24.0`.

**Which failure it was meant to fix:** Criterion 4, where none of the five fit cards mentioned the $24 price and most said "online" instead of Depop.

### Run Log — After

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1. Matching query completes all three tools | 4 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 2. Impossible query stops before suggest_outfit | 5 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 3. Selected item reaches suggest_outfit and create_fit_card unchanged | 5 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 4. Fit card is 2–4 sentences with the real price and platform | 4 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 5. Every search result is at or under max_price | 5 of 5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |

 query: vintage graphic tee under $30
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **1. 2000s Grunge Street** Layer the Graphic Tee over the white ribbed tank top, and pair it with the baggy st…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**1. 2000s Grunge Street**\nLayer the Graphic Tee over t…
      out: Scored this 2003 tour bootleg style graphic tee for just $24 on depop and it instantly unlocked my inner Y2K g…
  try 1: completed — fit card 314 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **1. Grunge Streetwear** Pair the graphic tee with the baggy straight-leg jeans, black combat boots, and the b…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**1. Grunge Streetwear**\nPair the graphic tee with the …
      out: Scored this 2003 tour bootleg style graphic tee for just $24 on depop and I’m literally obsessed. I threw it o…
  try 2: completed — fit card 230 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **90s Streetwear** Pair the Graphic Tee — 2003 Tour Bootleg Style with baggy straight-leg jeans, worn with chu…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**90s Streetwear**\nPair the Graphic Tee — 2003 Tour Boo…
      out: Scored this bootleg tour tee and I’m so obsessed with the faded 2003 graphics. I grabbed it for $24 on depop a…
  try 3: completed — fit card 244 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **Off-Duty Grunge** Layer the Graphic Tee — 2003 Tour Bootleg Style under the Black cropped zip hoodie, and pa…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**Off-Duty Grunge**\nLayer the Graphic Tee — 2003 Tour B…
      out: Channeling major off-duty grunge vibes with this 2003 tour bootleg style tee layered under a cropped zip hoodi…
  try 4: completed — fit card 296 chars
[1] parse_query
      in:  'vintage graphic tee under $30'
      out: {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
[2] search_listings (via MCP)
      in:  {'description': 'vintage graphic tee', 'size': None, 'max_price': 30.0}
      out: 8 items: Graphic Tee — 2003 Tour Bootleg Style, Y2K Baby Tee — Butterfly Print, Vintage Graphic Hoodie — Faded Black … +5 more
      →    branch: results found, continuing
[3] select_item
      in:  8 results
      out: Graphic Tee — 2003 Tour Bootleg Style ($24.0, depop)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', wardrobe_items=10
      out: **Double Black Grunge** Pair the Graphic Tee — 2003 Tour Bootleg Style with the baggy straight-leg jeans, blac…
[5] create_fit_card
      in:  item='Graphic Tee — 2003 Tour Bootleg Style', outfit='**Double Black Grunge**\nPair the Graphic Tee — 2003 Tou…
      out: Channeling serious off-duty rockstar energy with this double-black grunge fit today. I scored this 2003 tour b…
  try 5: completed — fit card 308 chars

impossible query stops early  (example wardrobe)
  query: designer ballgown size XXS under $5
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 1: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 2: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 3: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 4: stopped early — Nothing matched "designer ballgown". You could raise your bu
[1] parse_query
      in:  'designer ballgown size XXS under $5'
      out: {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
[2] search_listings (via MCP)
      in:  {'description': 'designer ballgown', 'size': 'XXS', 'max_price': 5.0}
      out: [] (empty)
      →    branch: empty, stopping before suggest_outfit
  try 5: stopped early — Nothing matched "designer ballgown". You could raise your bu

empty wardrobe  (empty wardrobe)
  query: denim jacket under $50
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **90s Denim-on-Denim** Pair the jacket with white or cream high-waisted straight-leg pants and a tucked-in bla…
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**90s Denim-on-Denim**\nPair the jacket with white or cream…
      out: Channeling major 90s skater energy today with this cropped light-wash layer. I scored it for $42 on poshmark a…
  try 1: completed — fit card 254 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **Outfit 1: Monochromatic Denim** * **Title:** Double Denim Streetwear  * **Description:** Pair the cropped ja…
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**Outfit 1: Monochromatic Denim**\n* **Title:** Double Deni…
      out: Finally tracked down the ultimate light wash cropped denim jacket on poshmark for $42, and it's in crazy good …
  try 2: completed — fit card 371 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **1. 90s Streetwear Double-Denim** Pair the cropped jacket with dark-wash straight-leg jeans to create a high-…
  [rate limit] 15 requests used this minute. Waiting 41s. This is normal.
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**1. 90s Streetwear Double-Denim**\nPair the cropped jacket…
      out: Scored this vintage light wash cropped denim jacket for $42 on Poshmark and it is officially my new everyday g…
  try 3: completed — fit card 349 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **Double Denim Minimalist** Pair the light wash jacket with high-waisted black straight-leg jeans to create hi…
  [rate limit] 15 requests used this minute. Waiting 1s. This is normal.
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**Double Denim Minimalist**\nPair the light wash jacket wit…
      out: Scored this cropped light wash denim jacket for just $42 on Poshmark and I am honestly obsessed with how versa…
  try 4: completed — fit card 344 chars
[1] parse_query
      in:  'denim jacket under $50'
      out: {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
[2] search_listings (via MCP)
      in:  {'description': 'denim jacket', 'size': None, 'max_price': 50.0}
      out: 5 items: Denim Jacket — Light Wash, Cropped, 90s Track Jacket — Navy/White Stripe, High-Waisted Denim Shorts — Cutoff … +2 more
      →    branch: results found, continuing
[3] select_item
      in:  5 results
      out: Denim Jacket — Light Wash, Cropped ($42.0, poshmark)
      →    took the top-ranked result
[4] suggest_outfit
      in:  item='Denim Jacket — Light Wash, Cropped', wardrobe_items=0
      out: **1. Double Denim Streetwear** Pair the cropped jacket with dark-wash straight-leg jeans to create intentional…
[5] create_fit_card
      in:  item='Denim Jacket — Light Wash, Cropped', outfit='**1. Double Denim Streetwear**\nPair the cropped jacket wit…
      out: I am completely obsessed with this light wash cropped denim jacket I just scored on Poshmark for $42. I paired…
  try 5: completed — fit card 313 chars

**Did it help, and how do I know:** Yes, criterion 4 went from 0/5 to 5/5, because every fit card in the after-run says "$24 on depop" in three sentences, and the other four criteria stayed at 5/5, so the fix didn't break anything else.

<!-- If it made things worse, say that. Honestly reported, that earns full
     credit and is more interesting than one that worked. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped where
     you did. "I ran out of time" is fine if it's true. Pretending nothing is
     left is not. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 3

       [ ] criteria.md has five numbered criteria, each with a target
       [ ] Each criterion has a reason underneath it
       [ ] All five unit 3 sections above have real content
       [ ] Tool Inventory: all three tools, inputs WITH TYPES, a specific
           return value, and the empty case
       [ ] Planning Loop names the branch rule and agent.py::run_agent
       [ ] Sample Run: one full query plus the three per-tool tests, as text
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN, you submit the same one
           next unit

     SUBMISSION CHECKLIST — unit 4

       [ ] mcp_server.py exists with one tool registered
           (or a written record of exactly where the rewire broke)
       [ ] Run Log — Before, five criteria, five tries each
       [ ] Real output pasted underneath, naming file and function
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, naming a place AND a mechanism
       [ ] Loop Trace, with the MCP call visible in it
       [ ] All three failure modes triggered and handled
       [ ] One improvement, with Run Log — After in the same format
       [ ] What's Still Broken
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository. Your commit history is what
     shows your criteria existed before your results did.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
