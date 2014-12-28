## Step 2: Creating Passive effects ##

First, we will learn the most basic type of effect: a passive effect. This type of effect requires no activations, targetting, trigger conditions, anything like that.

The card I will be using as an example is Comrade Swordsman of Landstar. This is one of the most basic types of effects that you can find. It reads, "All Warrior-Type monsters you control gain 400 ATK."

Its card code is 83602069, so open c83602069.lua using LuaEdit 2010, which you downloaded earlier.

````Lua
function c83602069.initial_effect(c)
	--atk
	local e1=Effect.CreateEffect(c)
	e1:SetType(EFFECT_TYPE_FIELD)
	e1:SetRange(LOCATION_MZONE)
	e1:SetTargetRange(LOCATION_MZONE,0)
	e1:SetCode(EFFECT_UPDATE_ATTACK)
	e1:SetTarget(aux.TargetBoolFunction(Card.IsRace,RACE_WARRIOR))
	e1:SetValue(400)
	c:RegisterEffect(e1)
end
````

So, let's go over this effect line-by-line. First, we see the function declaration, `function c83602069.initial_effect(c)` where the `c83602069` is the same as the filename. This function, ended by the `end` statement, contains information regarding the type of effects that the card has. If a card has multiple effects, they will all be described in this function. More on that later.

Each effect begins with the statement `local e1=Effect.CreateEffect(c)` and ends with `c:RegisterEffect(e1)`, where subsequent effects, if they exist, would replace `e1` with `e2`, `e3`, etc. It is generally accepted to put a brief comment (beginning with `--` before the effect to describe which effect is being created. In this example, `--atk` tells the person reading the script that the effect is the atk-modifying effect, even though in this case there is only one effect.

## Step 2a: Setting the effect type

The first thing you need to do with all effects is set the effect type. In this example, that is done with `e1:SetType(EFFECT_TYPE_FIELD)`. With passive effects, you will probably be using this, however here is a list of effect types and what they do:

- `EFFECT_TYPE_SINGLE`: Effect that applies to only a single card on the field
- `EFFECT_TYPE_FIELD`: Effect that applies to all cards on the field
- `EFFECT_TYPE_ACTIONS`:
- `EFFECT_TYPE_ACTIVATE`: Effect which activates (Spell/Trap)
- `EFFECT_TYPE_FLIP`:
- `EFFECT_TYPE_CONTINUOUS`:
- `EFFECT_TYPE_IGNITION`: Ignition effect of a monster
- `EFFECT_TYPE_TRIGGER_O`: Optional trigger effect
- `EFFECT_TYPE_TRIGGER_F`: Forced trigger effect
- `EFFECT_TYPE_QUICK_O`: Optional quick effect
- `EFFECT_TYPE_QUICK_F`: Forced Quick Effect

## Step 2b: Setting the range and target range ##

The next thing you need to do is set the range of where your card gets its effect. That is seen in the code `e1:SetRange(LOCATION_MZONE)`.

You can select one of the following ranges:

- `LOCATION_MZONE`: Monster card zone
- `LOCATION_SZONE`: Spell/trap card zone
- `LOCATION_ONFIELD`: Monster or spell/trap card zone
- `LOCATION_GRAVE`: In the graveyard
- `LOCATION_REMOVED`: Banished
- `LOCATION_DECK`: Deck
- `LOCATION_HAND`: Hand
- `LOCATION_EXTRA`: Extra deck
- `LOCATION_OVERLAY`: Xyz material

Similarly, for field effects, you need to specify the range of the monsters that are getting the effect. `e1:SetTargetRange(LOCATION_MZONE,0)` is the example. Note that this has 2 parameters. The first parameter is your cards and the second parameter is your opponent's cards. If only one side of the field is affected, the other side should say 0.

## Step 2c: Setting the SetCode

The SetCode, as seen in `e1:SetCode(EFFECT_UPDATE_ATTACK)`, goes along with the effect type set in Step 2a. This is what says the type of effect which you are creating.

There are a long list of SetCodes. Here is that list along with what each Setcode does:

- `EFFECT_IMMUNE_EFFECT`
- `EFFECT_DISABLE`
- `EFFECT_CANNOT_DISABLE`
- `EFFECT_SET_CONTROL`
- `EFFECT_CANNOT_CHANGE_CONTROL`
- `EFFECT_CANNOT_ACTIVATE`
- `EFFECT_CANNOT_TRIGGER`
- `EFFECT_DISABLE_EFFECT`
- `EFFECT_DISABLE_CHAIN`
- `EFFECT_DISABLE_TRAPMONSTER`
- `EFFECT_CANNOT_INACTIVATE`
- `EFFECT_CANNOT_DISEFFECT`
- `EFFECT_CANNOT_CHANGE_POSITION`
- `EFFECT_TRAP_ACT_IN_HAND`
- `EFFECT_TRAP_ACT_IN_SET_TURN`
- `EFFECT_REMAIN_FIELD`
- `EFFECT_MONSTER_SSET`
- `EFFECT_CANNOT_SUMMON`
- `EFFECT_CANNOT_FLIP_SUMMON`
- `EFFECT_CANNOT_SPECIAL_SUMMON`
- `EFFECT_CANNOT_MSET`
- `EFFECT_CANNOT_SSET`
- `EFFECT_CANNOT_DRAW`
- `EFFECT_CANNOT_DISABLE_SUMMON`
- `EFFECT_CANNOT_DISABLE_SPSUMMON`
- `EFFECT_SET_SUMMON_COUNT_LIMIT`
- `EFFECT_EXTRA_SUMMON_COUNT`
- `EFFECT_SPSUMMON_CONDITION`
- `EFFECT_REVIVE_LIMIT`
- `EFFECT_SUMMON_PROC`
- `EFFECT_LIMIT_SUMMON_PROC`
- `EFFECT_SPSUMMON_PROC`
- `EFFECT_EXTRA_SET_COUNT`
- `EFFECT_SET_PROC`
- `EFFECT_LIMIT_SET_PROC`
- `EFFECT_DEVINE_LIGHT`
- `EFFECT_CANNOT_DISABLE_FLIP_SUMMON`
- `EFFECT_INDESTRUCTABLE`
- `EFFECT_INDESTRUCTABLE_EFFECT`
- `EFFECT_INDESTRUCTABLE_BATTLE`
- `EFFECT_UNRELEASABLE_SUM`
- `EFFECT_UNRELEASABLE_NONSUM`
- `EFFECT_DESTROY_SUBSTITUTE`
- `EFFECT_CANNOT_RELEASE`
- `EFFECT_INDESTRUCTIBLE_COUNT`
- `EFFECT_UNRELEASABLE_EFFECT`
- `EFFECT_DESTROY_REPLACE`
- `EFFECT_RELEASE_REPLACE`
- `EFFECT_SEND_REPLACE`
- `EFFECT_CANNOT_DISCARD_HAND`
- `EFFECT_CANNOT_USE_AS_COST`
- `EFFECT_LEAVE_FIELD_REDIRECT`
- `EFFECT_TO_HAND_REDIRECT`
- `EFFECT_TO_GRAVE_REDIRECT`
- `EFFECT_REMOVE_REDIRECT`
- `EFFECT_CANNOT_TO_HAND`
- `EFFECT_CANNOT_REMOVE`
- `EFFECT_CANNOT_TO_GRAVE`
- `EFFECT_CANNOT_TURN_SET`
- `EFFECT_CANNOT_BE_BATTLE_TARGET`
- `EFFECT_CANNOT_BE_EFFECT_TARGET`
- `EFFECT_IGNORE_BATTLE_TARGET`
- `EFFECT_CANNOT_DIRECT_ATTACK`
- `EFFECT_DUAL_STATUS`
- `EFFECT_EQUIP_LIMIT`
- `EFFECT_DUAL_SUMMONABLE`
- `EFFECT_REVERSE_DAMAGE`
- `EFFECT_REVERSE_RECOVER`
- `EFFECT_CHANGE_DAMAGE`
- `EFFECT_REFLECT_DAMAGE`
- `EFFECT_CANNOT_ATTACK`
- `EFFECT_CANNOT_ATTACK_ANNOUNCE`
- `EFFECT_CANNOT_CHANGE_POS_E`
- `EFFECT_ACTIVATE_COST`
- `EFFECT_SUMMON_COST`
- `EFFECT_SPSUMMON_COST`
- `EFFECT_FLIPSUMMON_COST`
- `EFFECT_MSET_COST`
- `EFFECT_SSET_COST`
- `EFFECT_ATTACK_COST`
- `EFFECT_UPDATE_ATTACK`
- `EFFECT_SET_ATTACK`
- `EFFECT_SET_ATTACK_FINAL`
- `EFFECT_SET_BASE_ATTACK`
- `EFFECT_UPDATE_DEFENCE`
- `EFFECT_SET_DEFENCE`
- `EFFECT_SET_DECENCE_FINAL`
- `EFFECT_SET_BASE_DEFENCE`
- `EFFECT_REVERSE_UPDATE`
- `EFFECT_SWAP_AD`
- `EFFECT_SWAP_BASE_AD`
- `EFFECT_CHANGE_CODE`
- `EFFECT_ADD_TYPE`
- `EFFECT_REMOVE_TYPE`
- `EFFECT_CHANGE_TYPE`
- `EFFECT_ADD_RACE`
- `EFFECT_REMOVE_RACE`
- `EFFECT_CHANGE_RACE`
- `EFFECT_ADD_ATTRIBUTE`
- `EFFECT_REMOVE_ATTRIBUTE`
- `EFFECT_CHANGE_ATTRIBUTE`
- `EFFECT_UPDATE_LEVEL`
- `EFFECT_UPDATE_RANK`
- `EFFECT_CHANGE_RANK`
- `EFFECT_SET_POSITION`
- `EFFECT_SELF_DESTROY`
- `EFFECT_DOUBLE_TRIBUTE`
- `EFFECT_DECREASE_TRIBUTE`
- `EFFECT_DECREASE_TRIBUTE_SET`
- `EFFECT_EXTRA_RELEASE`
- `EFFECT_TRIBUTE_LIMIT`
- `EFFECT_PUBLIC`
- `EFFECT_COUNTER_PERMIT`
- `EFFECT_COUNTER_LIMIT`
- `EFFECT_RCOUNTER_REPLACE`
- `EFFECT_LPCOST_CHANGE`
- `EFFECT_LPCOST_REPLACE`
- `EFFECT_SKIP_DP`
- `EFFECT_SKIP_SP`
- `EFFECT_SKIP_M1`
- `EFFECT_SKIP_BP`
- `EFFECT_SKIP_M2`
- `EFFECT_CANNOT_BP`
- `EFFECT_CANNOT_M2`
- `EFFECT_CANNOT_EP`
- `EFFECT_DEFENCE_ATTACK`
- `EFFECT_MUST_ATTACK`
- `EFFECT_FIRST_ATTACK`
- `EFFECT_ATTACK_ALL`
- `EFFECT_EXTRA_ATTACK`
- `EFFECT_MUST_BE_ATTACKED`
- `EFFECT_AUTO_BE_ATTACKED`
- `EFFECT_ATTACK_DISABLED`
- `EFFECT_NO_BATTLE_DAMAGE`
- `EFFECT_REFLECT_BATTLE_DAMAGE`
- `EFFECT_PIERCE`
- `EFFECT_BATTLE_DESTROY_REDIRECT`
- `EFFECT_BATTLE_DAMAGE_TO_EFFECT`
- `EFFECT_TOSS_COIN_REPLACE`
- `EFFECT_ROSS_DICE_REPLACE`
- `EFFECT_FUSION_MATERIAL`
- `EFFECT_CHAIN_MATERIAL`
- `EFFECT_SYNCHRO_MATERIAL`
- `EFFECT_XYZ_MATERIAL`
- `EFFECT_FUSION-SUBSTITUTE`
- `EFFECT_CANNOT_BE_FUSION_MATERIAL`
- `EFFECT_CANNOT_BE_SYNCHRO_MATERIAL`
- `EFFECT_SYNCHRO_MATERIAL_CUSTOM`
- `EFFECT_CANNOT_BE_XYZ_MATERIAL`
- `EFFECT_SYNCHRO_LEVEL`
- `EFFECT_RITUAL_LEVEL`
- `EFFECT_XYZ_LEVEL`
- `EFFECT_EXTRA_RITUAL_MATERIAL`
- `EFFECT_NONTUNER`
- `EFFECT_OVERLAY_REMOVE_REPLACE`
- `EFFECT_SCRAP_CHIMERA`
- `EFFECT_SPSUM_EFFECT_ACTIVATED`
- `EFFECT_MATERIAL_CHECK`
- `EFFECT_DISABLE_FIELD`
- `EFFECT_USE_EXTRA_MZONE`
- `EFFECT_USE_EXTRA_SZONE`
- `EFFECT_MAX_MZONE`
- `EFFECT_MAX_SZONE`
- `EFFECT_HAND_LIMIT`
- `EFFECT_DRAW_COUNT`
- `EFFECT_SPIRIT_DONOT_RETURN`
- `EFFECT_SPIRIT_MAYNOT_RETURN`
- `EFFECT_CHANGE_ENVIRONMENT`
- `EFFECT_NECRO_VALLEY`
- `EFFECT_FORBIDDEN`
- `EFFECT_NECRO_VALLEY_IM`
- `EFFECT_REVERSE_DECK`
- `EFFECT_REMOVE_BRAINWASHING`
- `EFFECT_BP_TWICE`
- `EFFECT_UNIQUE_CHECK`
- `EFFECT_MATCH_KILL`
- `EFFECT_SYNCHRO_CHECK`