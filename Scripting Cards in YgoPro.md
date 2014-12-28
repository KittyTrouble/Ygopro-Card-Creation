# Step 2: Creating Passive effects #

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