Step 1.1 — Niche Selection
Chosen niche: Mechanical keyboards

I am building an online store for mechanical keyboards.

This niche is good for showing advanced filtering and custom metafields because mechanical keyboards have many variants. A single keyboard can come in different sizes (60%, 65%, TKL, full size), different switch types (linear, tactile, clicky), different keycap colors, and different layouts (ANSI or ISO). Customers also care about specific details like switch sound, keycap material, and connection type (wired or wireless). This gives me many attributes to turn into filters and metafields, which shows real store complexity.

Step 1.2 — Audience and Page Scope
Target audience: My target audience is young adults aged 18–35 who work from home or study and want a better typing experience. They are tech-savvy shoppers who enjoy customizing their setup and are willing to pay more for quality and unique designs.

3 must-have custom pages:
Build Your Keyboard (Custom Builder Page) This page lets customers pick their case, switches, and keycaps. It needs a Metaobject for each build option (like "Switch Type" or "Keycap Set") so the info stays organized and reusable.
Switch Guide Page This page explains different switch types (linear, tactile, clicky) with sound and feel details. It needs a Metaobject for "Switch Profile" that stores name, sound level, feel, and an image for each switch.
Community Builds / Gallery Page This page shows keyboards built by real customers. It needs a Metaobject for "Featured Build" that stores the builder's name, keyboard parts used, and photos.
1.2-----------------------------------------------------------------------------------------------------
Step 1.1 — Filter Inventory
Filter Name File What It Changes
1 money blocks/price.liquid Turns the raw price number into a real price with currency (e.g. R499.00) 2 image_url sections/main-product.liquid Resizes the main product image so it loads faster and fits the page 3 truncate sections/main-product.liquid Cuts the product description short so it does not take too much space 4 strip_html sections/product-list.liquid Removes HTML tags from the collection description so only clean text shows 5 upcase snippets/product-card.liquid Makes the product title show in all capital letters on the collection card

Step 1.2 — Conditional Logic Plan
Object used: product.compare_at_price

Logic: If product.compare_at_price is greater than product.price, the product is on sale.

True branch: Show a "Sale" badge and show both the old price (crossed out) and the new lower price. False branch: Show only the normal price, no badge.

File it will live in: blocks/price.liquid

## 1.3---------------------------------------------------------------------------------------------------
Step 1.1 — Section Concept

Section name: Switch Showcase (sections/switch-showcase.liquid)

Purpose: This section shows different keyboard switch types (like Linear, Tactile, Clicky) side by side. Each one shows how it feels and how loud it is. It helps customers pick the right switch before buying.

Page: Homepage

Step 1.2 — Block Inventory
#	Block Type	File	What It Shows	Reusable?
1	switch-card	blocks/switch-card.liquid	A card for one switch type: name, image, feel, and short text	Only for this section
2	sound-meter	blocks/sound-meter.liquid	A bar that shows how loud a switch is (1 to 5 level)	Reusable anywhere sound matters
3	spec-badge	blocks/spec-badge.liquid	A small label + value tag (e.g. "Actuation Force: 45g")	Reusable on product pages too
Step 1.3 — Settings Plan
#	Setting ID	Type	File	What It Changes
1	layout_direction	select	sections/switch-showcase.liquid	Switches cards between row layout or grid layout
2	gap	range	sections/switch-showcase.liquid	Changes the space between switch cards
3	switch_color	color	blocks/switch-card.liquid	Changes the accent border color of a switch card
4	noise_level	range	blocks/sound-meter.liquid	Changes how many bars are filled in (how loud it looks)
5	badge_style	select	blocks/spec-badge.liquid	Switches badge look between solid or outline style

## 1.4 ------------------------------------------------------------------------------------------------
# Step 1.1 — Metafield Plan

Resource type: Product

Namespace.key: custom.actuation_force
Type: Number (integer)

What it shows: The actuation force of the keyboard's switch, in grams (e.g. "45g"). It will show as a small spec line under the price on the product page, using our existing Spec Badge block from Day 3.

# Step 1.2 — Metaobject Plan

Metaobject type: switch_profile

Fields:

switch_name — single line text (e.g. "Tactile", "Linear", "Clicky")
sound_level — number (1 to 5, how loud the switch is)
feel_description — single line text (short description of how it feels)

What it represents: One reusable switch profile entry, like "Tactile switch info." The same entry can be linked to many keyboard products that use that same switch type, instead of typing the same info again for every product.

How products reach it: Through a metafield of type List of metaobject references, so one keyboard product can link to one or more switch profiles.

# Step 1.3 — Integration Plan

File: blocks/spec-badge.liquid (for the metafield)
File: blocks/switch-card.liquid (for the metaobject)

Blank state:

If custom.actuation_force is blank, the Spec Badge does not render at all — no empty badge shape shows.
If a product has no linked switch profiles, the Switch Card block section does not render any cards — no empty boxes or headings show.

## 1.5---------------------------------------------------------------------------------------
# Step 1.1 — Threshold Plan

Setting: free_shipping_threshold, type number

Scope: Global (config/settings_schema.json). A store normally has one shipping rule for the whole cart, not different rules per section. So this belongs in global cart settings.

Default value: 500 (R500)

Enable/disable checkbox: enable_free_shipping_bar

# Step 1.2 — Messaging Plan

Still short of the threshold:

"You're R{{ remaining_amount }} away from free shipping!"

Threshold met:

"You've unlocked free shipping!"

When off or threshold is 0: The whole progress bar block does not render at all — nothing shows, no empty space left behind.

# Step 1.3 — Integration Plan

Target file: snippets/cart-drawer.liquid (the shipping bar markup goes directly inside this file, since it's rendered as part of the cart-drawer section)

Does this file get re-rendered on cart change? Yes. Looking at assets/component-cart-items.js, when a cart line updates, the code calls morphSection(this.sectionId, ...) using HTML returned from the server. This section ID covers the whole cart drawer, and since cart-drawer.liquid is part of that section's rendered output, our shipping bar will automatically update whenever the section is morphed — no extra work needed.

Do we need new JavaScript? No. The file already listens for cartLinesUpdate events (see #handleCartUpdate), and when the cart changes, it either:

Uses the HTML already sent back from the server (sections[this.sectionId]), or
Calls sectionRenderer.renderSection(...) to fetch a fresh HTML fragment

Since our shipping bar is plain Liquid markup inside this same section, it gets included in that same server-rendered HTML automatically. We don't need to write any new fetch calls or event listeners — the existing flow already covers it.

## 1.6 -----------------------------------------------------------------------------------------------
# Step 1.1 — Collection & Filter Plan

Collection: All Keyboards (handle: all-keyboards)

# 2 filter dimensions:

Switch Type (option: Linear, Tactile, Clicky) — a list filter
Price — using price_range

Data gap: Right now, not every keyboard product has a Switch Type option set as a real variant option. Some products may only have Color as an option. I need to add Switch Type as a proper option on each keyboard product so it shows up as a filter.

Filters block settings I will change:

filter_style → change from default to Drawer, so filters open in a side panel instead of a plain list. This makes shopping easier on mobile.
enable_sorting → turn ON, so customers can sort by price (low to high, high to low).
# Step 1.2 — Swatch Plan

Product: Apex 60% Compact Mechanical Keyboard
Option: Color

Swatch values:

Black — solid black swatch
White — solid white swatch
Purple — solid purple swatch

Where swatches show:

Collection grid card, using blocks/swatches.liquid
Product page variant picker, using blocks/variant-picker.liquid's show_swatches setting

# Step 2.1

We are not creating a new section or block for filtering because sections/main-collection.liquid already calls content_for 'block', type: 'filters', which renders the existing blocks/filters.liquid — a fully built filtering and sorting UI with its own settings.

Swatch data comes directly from each product's option values in Shopify Admin — blocks/swatches.liquid checks product_option.values | map: 'swatch' and only renders swatches if a real swatch image/color has been assigned to that option value.