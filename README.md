## Step 1.1 — Niche Selection

Chosen niche: Mechanical keyboards

I am building an online store for mechanical keyboards.

This niche is good for showing advanced filtering and custom metafields because mechanical keyboards have many variants. A single keyboard can come in different sizes (60%, 65%, TKL, full size), different switch types (linear, tactile, clicky), different keycap colors, and different layouts (ANSI or ISO). Customers also care about specific details like switch sound, keycap material, and connection type (wired or wireless). This gives me many attributes to turn into filters and metafields, which shows real store complexity.

# Step 1.2 — Audience and Page Scope

Target audience:
My target audience is young adults aged 18–35 who work from home or study and want a better typing experience. They are tech-savvy shoppers who enjoy customizing their setup and are willing to pay more for quality and unique designs.

# 3 must-have custom pages:

1. Build Your Keyboard (Custom Builder Page)
This page lets customers pick their case, switches, and keycaps. It needs a Metaobject for each build option (like "Switch Type" or "Keycap Set") so the info stays organized and reusable.
2. Switch Guide Page
This page explains different switch types (linear, tactile, clicky) with sound and feel details. It needs a Metaobject for "Switch Profile" that stores name, sound level, feel, and an image for each switch.
3. Community Builds / Gallery Page
This page shows keyboards built by real customers. It needs a Metaobject for "Featured Build" that stores the builder's name, keyboard parts used, and photos.

## 1.2-----------------------------------------------------------------------------------------------------
# Step 1.1 — Filter Inventory
#	Filter Name	 File	                      What It Changes
1	money	     blocks/price.liquid	           Turns the raw price number into a real price with currency (e.g. R499.00)
2	image_url	 sections/main-product.liquid	Resizes the main product image so it loads faster and fits the page
3	truncate	 sections/main-product.liquid	Cuts the product description short so it does not take too much space
4	strip_html	 sections/product-list.liquid	Removes HTML tags from the collection description so only clean text shows
5	upcase	     snippets/product-card.liquid	    Makes the product title show in all capital letters on the collection card
# Step 1.2 — Conditional Logic Plan

Object used: product.compare_at_price

Logic:
If product.compare_at_price is greater than product.price, the product is on sale.

True branch: Show a "Sale" badge and show both the old price (crossed out) and the new lower price.
False branch: Show only the normal price, no badge.

File it will live in: blocks/price.liquid