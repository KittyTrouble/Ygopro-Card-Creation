# Adding cards to cards.cdb #

This guide will help you add new cards to the cards.cdb file. This covers all aspects of card that is not the actual scripting of a card. I have written this guide in a manner that will help you create new cards from scratch rather than merely editing existing cards.

## Step 1: Download prerequisite ##

The first step you need is to download Sqlite Expert personal. The link is [http://www.sqliteexpert.com/download.html](http://www.sqliteexpert.com/download.html "here"). Note that there is an option for the pro version, you don't need that, just the free version is fine.

## Step 2: Open cards.cdb ##

This can be done either by right clicking cards.cdb in your ygopro folder and choosing Open With... Sqlite Expert, or by first opening Sqlite Expert and opening it from within. Note if you do the latter, you will need to have it search for all files since .cdb isn't a recognized extension.

## Step 3: Edit texts ##

You can see that to the left, there are 2 sub-databases, `datas` and `texts`. You need to edit both of them to add a card to ygopro. First, we will edit texts, so click `texts` followed by clicking the `data` tab to the right.

![](http://i.imgur.com/jrmCJ6w.png)

You will see a list of entries. To add a new card, click the ![](http://i.imgur.com/s7Hmc4o.png) button to add a new card to the file. You will see a new entry with a bunch of null values. Double-click one of those values and you will see a screen allowing you to edit all of the entries.

Here is a list of the values to edit and what they do:

- `RecNo`: Leave blank, it will take care of this for you.
- `id`: The number found to the lower left of the card. It must be a unique number.
- `name`: The card's name
- `desc`: The card's description
- `str1`-`str16`: add a space to each of them, because leaving them null can crash ygopro, these are the prompts that come up.

## Step 4: Edit datas ##

Next do the same steps in the data sub-database. The records you will edit are as follows:

- `id`: Must match the id you entered in Step 3.
- `ot`: The region. Enter 1 for OCG, 2 for TCG, 3 for TCG/OCG, 4 for Anime, 0 for blank.
- `alias`: Enter 0 normally, if the name is treated as another name (such as the Harpie Ladies) while in Deck, you will enter that card's id here.
- `setcode`: Archetype number. Says that a card belongs in a certain archetype. Each archetype must be given a unique number.
	- Numbers of archetypes are input in decimal, however you must do hexidecimal conversions for some of the combined-archetype stuff.
	- Basic archetypes run from hexadecimal value 0x1-0xFFF (4095 in decimal)
	- Extension archetypes are one hexadecimal digit appended with the 3-digit hexadecimal value of another archetype. For example, if you wanted to create an "Ojama Knight" archetype, with "Ojama" already possessing the hex value 0xF (you need to convert the decimal value to hex first), first add zeros so 0x00F and then pick a digit to go before it. So your "Ojama Knight" archetype could be 0x100F, or converted back to decimal 4111.
	- Dual archetypes are formed by converting two archetypes to hex, expanding them to 4 digits, and appending them to each other in either order. For example, if you wanted to create a monster called "The Fabled Lightsworn", take the archetype value for Fabled (53, converted to 0x0035), the value for Lightsworn (56, converted to 0x0038) and append them to each other (0x00350038) then convert that number back to decimal and you would have your new setcode value of 3473464.
		- This can be done with 3 or more archetypes. See Number 62: Galaxy-Eyes Prime Proton Dragon, which is a combination of Galaxy-Eyes (4219, or 0x107B), Photon (85, or 0x0055), and Number (72, or 0x0048) to make 0x107B00550048 which converts to 18120472592456. 
- `type`: the type of card, for example Spell Card, Fusion Monster, etc. See section 4b for what to enter here.
- `atk`: the ATK of the monster, 0 if spell/trap.
- `def`: the DEF of a monster, 0 if spell.trap.
- `level`: the Level/rank of a monster, 0 if spell/trap.
	- For pendulums, it is a bit trickier than that since the level holds data for both scales as well as the level. This will involve appending the 2-digit hex value of the blue scale, the 2-digit value of the red scale, and 4-digit value of the card's Level. For example, a 10-scale level 1 monster will have (0x0A, 0x0A, 0x0001) combined to make 0x0A0A0001, thus the "level" category would be the decimal value of that, 168427521
- `race`: the type of monster, see 4c, 0 if spell/trap.
- `attribute`: the attribute of the monster, see 4c, 0 if spell/trap.
- `category`: used for finding cards in the deck constructor, 0 if nothing is specified.

## Step 4b: Choosing a card's type ##

A card's type is given by adding combinations of tags together to form the type of card that you want. Each tag is based on a power of 2 so that when you add them together, each combination will always be unique.

You will never really use a tag by itself. The exception are the Spell and Trap tags, which default to Normal Spells/Traps as those have no icon. However, I am including them for completeness as sometimes you might have to do something new.

Here are a few examples: The "Monster" tag is 1, you will add it to all other tags that are monsters. So for "Monster|Normal" you will add the Normal tag, 16, plus the Monster tag, 1, to get 17. Tuner is a tag that can be added to Normal monsters or Effect monsters, so you add the code for Tuner (4096) plus the codes for Normal and Monster or the codes for Effect and monster to get the code you want.

All existing combinations are found in the chart below. However, suppose that you need to include a new combination that does not previously exist. Say, a Fusion Monster who is also a Union Monster, Tuner, and has an Effect. You would look up each of those codes: 1 (Monster) + 32 (Effect) + 64 (Fusion) + 1024 (Union) + 4096 (Tuner), and 5217 would be your new code. Hope that makes sense.

- `1` : Generic Monster (Used in combination with other tags)
- `2` : Spell Card (Normal by default)
- `4` : Trap Card (Normal by default)
- `8` : ??? (Used by tokens)
- `16` : Normal
	- 17 for Normal Monster (16 + 1)
- `32` : Effect
	- `33` for Effect Monster (32 + 1)
- `64` : Fusion
	- `65` for Fusion Monster (64 + 1)
	- `97` for Fusion / Effect Monster (64 + 32 + 1)
- `128` : Ritual
	- `129` for Ritual Monster (128 + 1)
	- `130` for Ritual Spell (128 + 2)
	- `161` for Ritual / Effect Monster (128 + 32 + 1)
- `256` : Trap Monsters (I don't know what this does, but it's not used in any cards I see. Use Continuous Traps instead for Trap Monsters.)
- `512` : Spirit
	- `545` for Spirit Monster (With effect, 512 + 32 + 1)
- `1024` : Union
	- `1057` for Union Monster (With effect, 1024 + 32 + 1)
- `2048` : Gemini
	- `2081` for Gemini Monster (With effect, 2048 + 32 + 1)
- `4096` : Tuner
	- `4113` for Tuner / Normal Monster (4096 + 16 + 1)
	- `4129` for Tuner / Effect Monster (4096 + 32 + 1)
- `8192` : Synchro
	- `8193` for Synchro Monster (8192 + 1)
	- `8225` for Synchro / Effect Monster (8192 + 32 + 1)
	- `12321` for Synchro / Tuner / Effect Monster (8192 + 4096 + 32 + 1)
- `16384` : Token
	- `16401` for Actual Token (Uses 16384 + 16 for Normal + 8 for ??? + 1 for monster)
- `32768` : ??? (I do not know what this is used for.)
- `65536` : Quick-Play
	- `65538` for Quick-Play Spell Card (65536 + 2)
- `131072` : Continuous
	- `131074` for Continuous Spell Card (131072 + 2)
	- `131076` for Continuous Trap Card (131072 + 4)
- `262144` : Equip
	- `262146` for Equip Spell Card (262144 + 2)
- `524288` : Field
	- `524290` for Field Spell Card (524288 + 2)
- `1048576` : Counter
	- `1048580` for Counter Trap Card (1048576 + 4)
- `2097152` : Flip
	- `2097185` for Flip Effect Monster (2097152 + 32 + 1)
- `4194304` : Toon
	- `4194337` for Toon Monster (With effect, 4914304 + 32 + 1)
- `8388608` : Xyz
	- `8388609` for Xyz Monster (8388608 + 1)
	- `8388641` for Xyz / Effect Monster (8388608 + 32 + 1)
- `16777216` : Pendulum
	- `16777233` for Pendulum Normal Monster (16777216 + 16 + 1)
	- `16777249` for Pendulum Effect Monster (16777216 + 32 + 1)

## Step 4c: Choosing a monster's race and attribute ##

Skip this section if the card is not a monster. Multi-typing is the same as in the card type section. However, it is unlikely that such a monster will ever exist. Nonetheless, you could add two types and attributes together to get a new type, if you wanted to.

**List of races**

- `1` : Warrior
- `2` : Spellcaster
- `4` : Fairy
- `8` : Fiend
- `16` : Zombie
- `32` : Machine
- `64` : Aqua
- `128` : Pyro
- `256` : Rock
- `512` : Winged-beast
- `1024` : Plant
- `2048` : Insect
- `4096` : Thunder
- `8192` : Dragon
- `16384` : Beast
- `32768` : Beast-Warrior
- `65536` : Dinosaur
- `131072` : Fish
- `262144` : Sea Serpent
- `524288` : Reptile
- `1048576` : Psychic
- `2097152` : Divine-beast
- `4194304` : Creator God
- `8388608` : Wyrm

**List of attributes**

- `1` : EARTH
- `2` : WATER
- `4` : FIRE
- `8` : WIND
- `16` : LIGHT
- `32` : DARK
- `64` : DIVINE

## Step 5: Including the card's image ##

The image needs to be the full image of the card, not only the "image" portion. You can use [Yu-Gi-Oh CardMaker](yugiohcardmaker.net) if the card is a custom card, otherwise a full scan is needed.

First, shrink your card image size to `177 x 254` and save it as your card id with a `.jpg` extension into your pics folder. Then, shrink it to `44 x 64` and save it with the same name into the `thumbnail` folder inside your `pics` folder.

If your card is a field spell, however, you will also need a `512 x 512` `.png` of just the image part. That goes in the `field` folder located within the `pics` folder.

## Step 6: Scripting ##

Next, you need to script your card. That will be covered in the next tutorial.

----------
Credits: I used the following sources as well as my own experiments to make this tutorial:

- [Adding cards to YGOPro Tutorial @ YGOPro.co](http://www.ygopro.co/tabid/95/g/posts/t/120/Adding-cards-to-YGOPro--Tutorial----Scripting-video-Added.aspx#post381)
- [Tutorial How to add cards to YGOPro](https://www.youtube.com/watch?v=XePYKHv4g1o#t=17)