# Dungeons & Dragons

## Stats

### Abilities

- Strength
- Dexterity
- Constitution
- Intelligence
- Wisdom
- Charisma

Random initial scores:
1. Roll 4d6 and add up the 3 highest values.
2. Repeat five more times.
3. Associate each result with an ability.

- Expected single ability score value: 12.24
- Expected total: 73.47
- In Critical Role: reroll everything if total is below 70

Fixed initial scores:
1. Use the above method as if you had rolled the following totals: 15, 14, 13, 12, 10, 8.

- Total: 72

Custom initial scores:
1. Every ability starts at 8.
2. You can spend 27 points.
3. Each ability increase costs 1 point, up to 13.
4. Ability increases to 14 and 15 cost 2 points.

- Minimum total: 15 + 15 + 15 + 8 + 8 = 61
- Maximum total: 13 + 13 + 13 + 13 + 13 + 10 = 75

Modifiers:
- Each ability modifier is calculated in the following way: `(Ability score - 10) / 2`, rounded down (-1.5 = -2).

### HP and Hit dice

- Hit die type is defined by class (e.g. d6-d12)
- Staring HP = Maximum Hit die roll + Constitution modifier
- When leveling up, roll your Hit Die + Constitution modifier and add it to your max HP
  - House rule (Critical Role): Reroll any 1s in your Hit Die
- Instead of rolling a die, you may use the average roll of the die, rounded up.
- When your Constitution modifier goes up, your max HP goes up by 1 for each level you have.

### AC (Armor Class)

Starting at `10 + Dexterity modifier`.

Using Armor you're not proficient in has drawbacks:
- Disadvantage on ability checks, saving throws and attack rolls that involve Strength or Dexterity.
- Can't cast spells.

Different types of armor:
- Light Armor
  - AC: (11-12) + Dex modifier.
  - May give disadvantage on Stealth checks.
- Medium Armor
  - AC: (12-15) + Dex modifier (max 2).
  - May give disadvantage on Stealth checks.
- Heavy Armor
  - AC: 14-18.
  - Always gives disadvantage on Stealth checks.
  - May require minimum Str score to not reduce movement speed by 10 feet.
- Shield
  - Increases AC by 2.

See the Armor table (p. 145) for details.

### Weapons

Types of weapons:
- Melee
  - Attack targets within 5 feet
    - Throw attack of a weapon without the *Thrown* property is considered Improvised Weapon, 1d4, normal range 20 feet, long range 60 feet.
    - Melee attack of a ranged weapon is also an Improvised Weapon.
- Ranged
  - Attack targets at a distance.
- Unarmed strike
  - Has proficiency bonus. *1 + Str modifier* bludgeoning damage.

Weapon properties:
- **Ammunition**: At the end of a battle, you can spend 1 minute to recover half the expended ammunition. Melee attack is considered Improved Weapon, 1d4 damage.
- **Finesse**: Can use Dexterity modifier instead of Strength.
- **Heavy**: Small or tiny creatures have disadvantage with the weapon.
- **Light**: Easy to dual-wield. (Why?)
- **Loading**: Only one attack per action, even if you can attack twice otherwise.
- **Martial**: Requires explicit proficiency (contrary to Simple weapons).
- **Range**: Normal range and long range (attack with disadvantage).
- **Reach**: Adds 5 feet to melee range.
- **Simple**: Doesn't require explicit proficiency (contrary to Martial weapons).
- **Special**: Has special rules detailed in the handbook.
- **Thrown**: You can throw the weapon to make a ranged attack. Melee thrown weapons (like a Handaxe) use the same modifier for ranged attacks as melee attacks (typically Str, or choice between Str and Dex for Finesse weapons).
- **Two-Handed**: Requires two hands to attack with.
- **Versatile**: Can be used with one or two hands (dealing more damage).

Example weapons:
| Name           | Type   | Damage          | Properties                                             |
| -------------- | ------ | --------------- | ------------------------------------------------------ |
| Dagger         | Melee  | 1d4 piercing    | Simple, finesse, light, thrown (range 20/60)           |
| Shortsword     | Melee  | 1d6 piercing    | Martial, finesse, light                                |
| Battleaxe      | Melee  | 1d8 slashing    | Martial, versatile (1d10)                              |
| Pike           | Melee  | 1d10 piercing   | Martial, heavy. reach, two-handed                      |
| Sling          | Ranged | 1d4 bludgeoning | Simple, ammunition (range 30/120)                      |
| Crossbow, hand | Ranged | 1d6 piercing    | Martial, ammunition (range 30/120), light, loading     |
| Longbow        | Ranged | 1d8 piercing    | Martial, ammunition (range 150/600), heavy, two-handed |

### Proficiency

Adds the character's Proficiency Bonus to things they are proficient at:
- Armor (e.g. Heavy, medium, light, shields)
- Weapons (e.g. Warhammer)
- Saving throws (e.g. Strength)
- Skills (e.g. Acrobatics, History)
- Tools (e.g. Thieves' tools)

Proficiency bonus starts at +2 in level 1 and increases by on every 4 levels.

### Alignment

Two axes:
1. Lawful, Neutral or Chaotic
2. Good, Neutral or Evil

E.g. *Lawful good*

### Leveling up

Starting level is 1. Max level is 20.
Leveling up can be through gaining XP (Experience Points) or by campaign milestones.

When a class provides an Ability Score Improvement while gaining a level, this means you can improve an Ability Score by 2 or two different Ability Scores by 1 each (up to 20).

## Races

Define, among others:
1. Ability Score Increases at the start (e.g. +2 Constitution, +1 Wisdom)
2. Size
3. Speed (e.g. 25-35 feet)
4. Special features (e.g. Darkvision)
5. Resistances / weaknesses
6. Proficiencies
7. Languages

- Dwarf
- Elf
- Halfling
- Human
- Dragonborn
- Gnome
- Half-Elf
- Half-Orc
- Tiefling

## Classes

Define, among others:
1. Hit Die
2. Primary Ability (e.g. Strengh or Dexterity for Fighters)
3. Saving throw proficiencies (at least 2 per class)
4. Amor and Weapon proficiencies
5. Level-up effects (spells, ability score improvements, other features)
6. Attacks/Spells

- Barbarian
- Bard
- Cleric
- Druid
- Fighter
- Monk
- Paladin
- Ranger
- Rogue
- Sorcerer
  - Natural magic users, learn spells and can spend sorcery points to make them more powerful when casting.
- Warlock
- Wizard
  - Learned magic users, copy spells into spellbook and prepare them each long rest.

## Checks

### Ability/skill checks

1. Roll a d20
2. Add the ability modifier (e.g. Dex)
3. If it's a skill (e.g. Acrobatics) you're proficient in, add the Proficiency Bonus
4. Apply other bonuses and penalties (e.g. feats, spells, etc)
5. Compare to target number (e.g. DC (Difficulty Class) for ability checks and AC (Armor Class) for attacks)
   - It succeeds if its &le; DC or AC

Typical DCs:
- Very easy: 5
- Easy: 10
- Medium: 15
- Hard: 20
- Very hard: 25
- Nearly impossible: 30

Each ability (except Constitution) has associated skills:
- Strength
  - Athletics
- Dexterity
  - Acrobatics
  - Sleight of Hand
  - Stealth
- Intelligence
  - Arcana
  - History
  - Investigation
  - Nature
  - Religion
- Wisdom
  - Animal Handling
  - Insight
  - Medicine
  - Perception
  - Survival
- Charisma
  - Deception
  - Intimidation
  - Performance
  - Persuasion

### Saving throws

Saving throws are just like ability checks, just with different proficiencies. They are usually made in reaction to a threat, such as a magical effect, poison, disease, etc.

### Advantage and disadvantage

- When a player has advantage, they roll 2 d20 and choose the highest number.
- When a player has disadvantage, they roll 2 d20 and choose the lowest number.
- Advantage and disadvantage don't stack (you always roll a maximum of two d20).
- A roll with both advantage and disadvantage rolls a single d20.
- Besides conditions that already grant advantage/disadvantage, the GM can decide that circumstances influence a given roll to have advantage/disadvantage (e.g. a player distracts an NPC so another player can have advantage on a stealth check).

### Passive checks

The average result of a task done repeatedly (e.g. searching for secret doors over and over) or to be used when the DM does not want the players to roll dice.
- 10 + all check modifiers
- If player has advantage, add 5
- If player has disadvantage, subtract 5

### Help

When two characters with the necessary requirements (e.g. proficiency) wish to work together in a skill, one can give advantage to the other's roll.

### Group check

When the entire group needs to make a check (e.g. stealth), everyone rolls and the group succeeds if at least half of the players succeed.

### Contests

Two creatures competing for the same thing (e.g. trying to pull a rope to their side) roll ability/skill checks and compare final results.

The skills may also be different, such as the player rolling a stealth skill check against an opponent's perception check. If the opponent is not searching, the DM may use the opponent's passive perception instead.


## Math

- Round down any decimal numbers

## Movement

- Character speed is the number of feet they move per round (6 seconds).
- Difficult terrain costs 1 additional speed per foot (e.g. swamp, rubble, ice).
  - Other creatures count as difficult terrain
  - You can only move through other creatures if they are friendly **or** 2 sizes smaller or larger than you
  - You can't end your turn in another creature's space
- Climbing, swimming and crawling also cost 1 additional speed per foot.
  - Climbing slippery surfaces or swimming in rough water may need a successful athletics check.
  - Stacks with difficult terrain: crawling 1 foot on difficult terrain costs 3 feet.
- Long jump:
  - Run 10 feet and cover *your Str in feet* in the air. The covered distance still counts as movement distance you have to spend.
  - Without running 10 feet prior, you can only cover half the distance in the jump.
  - If clearing obstacles, you must succeed a DC 10 *athletics* check.
  - If landing in difficult terrain, you must succeed a DC 10 *acrobatics* check, otherwise you fall **prone**.
- High jump:
  - Run 10 feet and reach *3 + Str modifier* feet up.
  - Without running 10 feet prior, you can only cover half the distance in the jump.
- A falling creature takes 1d6 bludgeoning damage for every 10 feet it fell (max 20d6). It falls **prone** as long as it took damage.
- Dropping to **prone** doesn't cost any movement
- Standing up from **prone** costs half your movement speed. You can't stand up if you have 0 speed.

If using a grid, typically each square represents 5 feet. 
- Diagonals can be counted as 5 feet (*PHB*); or
- Alternating between 5 and 10 feet each (*DMG*)

Creature sizes:
- Tiny occupies 2.5x2.5 feet
- Small occupies 5x5 feet
- Medium occupies 5x5 feet
- Large occupies 10x10 feet
- Huge occupies 15x15 feet
- Gargantuan occupies 20x20 feet

A creature can squeeze into a space one size smaller than it.
- Movement costs 1 more in each such space.
- Disadvantage or attack rolls and Dex saving throws.
- Advantage on attack rolls against it.

### Flying

If a flying creature is knocked **prone**, has its speed reduced to 0 or otherwise immobilized, it falls.
- Exception: if it has the ability to *hover* or if being held aloft by magic (e.g. **fly** spell).

## Combat

When combat begins:
1. **Determine surprise**: If one side is trying to be stealthy, compare their *stealth* checks to the other side's *passive wisdom*. Anyone who fails is *surprised* and cannot move, act or react in their first turn.
2. **Establish positions**: Describe the scene in *theater of the mind* or place characters in a battle map.
3. **Roll initiative**: Dexterity check to determine order of creatures' turns. Tie breaks can be decided by:
   - Players deciding between their ties
   - DM deciding between monsters' ties
   - DM deciding between player and monster ties
   - Rolling a d20 tiebreaker

Each round of combat lasts 6 seconds.

Each turn, in any order:
1. Move
   - Move up to your character's speed, taking the [movement rules](#movement) into account 
   - You can break up movement in between the other actions in the turn, or between different attacks in the same action
   - Leaving an hostile creature's reach allows it one opportunity attack as a reaction
2. Action (single)
   - Attack
   - Cast a spell
   - Dash (another move)
   - Disengage (allows movement without provoking attacks of opportunity)
   - Dodge
     - Until your next turn, attacks made by creatures you can see have disadvantage and you have advantage on Dexterity saving throws.
     - You lose the benefit if you are incapacitated or your speed drops to 0.
   - Help
     - Give advantage to another creature on its next ability check to perform the task you are helping with.
     - Help a friend attack a creature within 5 feet of you, by giving them advantage on their first attack roll.
   - Hide (with stealth check)
   - Improvise an action (the DM decides whether it's possible and if it needs any roll)
   - Ready
     - Holds your action until a specific trigger triggers your reaction.
     - Readying spells requires [concentration](#magic).
   - Search (*perception* or *investigation* check)
   - Use an object
     - Certain objects require an action to use (e.g. Potion of Healing).
     - Using two objects requires the action and the free action.
3. Bonus action (single)
   - Attack with a light melee weapon if you attacked with another on your action (don't add a positive ability modifier to the second damage roll)
   - Another bonus action given by an ability, spell, etc
4. Free actions
   - Communicate through brief sounds/gestures
   - Interact with a single object or feature of the environment (open a door, draw your weapon)
   - Some interactions may require a full action (e.g. opening a stuck door or turn a crank to lower a drawbridge)
5. Reaction (single)
   - Attack of opportunity
   - Another reaction given by an abilitiy, spell, etc


Attack action:
1. Determine (dis)advantage
   - If you can't see the target, you have disadvantage (even if you can hear it)
   - If the attacker isn't where you attacked, you automatically miss but the DM doesn't say whether you guessed the location correctly
   - If the target can't see you, you have advantage
   - If you are hidden (unseen and unheard) you give away your location after the attack (whether it hits or misses)
   - Long ranged attacks have disadvantage
   - Ranged attacks while a hostile creature is within 5 feet from you have disadvantage
     - As long as the creature can see you and is not incapacitated
2. Attack roll
   - Melee attack roll: d20 + proficiency bonus (if proficient with the weapon) + strength modifier
     - Unarmed strike: Has proficiency bonus. *1 + Str modifier* bludgeoning damage.
   - Ranged attack roll: d20 + proficiency bonus (if proficient with the weapon) + dexterity modifier
     - If target has half cover, it gains +2 to AC and Dextery saving throws
     - If target has 3/4 cover, it gains +5 to AC and Dextery saving throws
     - If target has full cover, it can't be targeted
   - On a 1, the attack always misses (critical miss)
   - On a 20, the attack always hits (critical hit)
   - When the result is equal to or greater than the target AC, it succeeds
3. Damage roll
   - Die indicated by the weapon + ability modifier (but not proficiency bonus).
   - If the attack was a critical hit, double the damage dice (but not the modifier)

Special melee attacks:
- Grapple
  - The target must be one size larger or less
  - Requires one free hand
  - Grapple check instead of attack roll (*athletics* check contested by target's *athletics* or *acrobatics* check)
  - Automatic success if target is incapacitated
  - On success, target is **grappled**
  - Releasing requires no action
- Shove
  - The target must be one size larger or less
  - Shove check instead of attack roll (*athletics* check contested by target's *athletics* or *acrobatics* check)
  - Automatic success if target is incapacitated
  - On success, you knock the target **prone** or push it 5 feet away from you

### Underwater combat

- Melee attacks of creatures without swimming speed have **disadvantage** (except dagger, javelin, shortswod, spear, trident).
- Ranged attacks in normal range have disadvantage (except crossbow, net, javelin, spear, trident, dart).
- Ranged attacks in long range automatically miss.
- Creatures fully immersed in water have resistance to fire damage.

## Magic

Different abilities can be used for spellcasting (e.g. Intelligence, Charisma)

- Spell Save DC = 8 + Proficiency Bonus + Ability modifier
- Spell Attack modifier = Proficiency Bonus + Ability modifier

Spells:
- You can't cast spells while wearing Armor you're not proficient in.
- Each spell has a 0-9 level that denotes its power.
- **Cantrips**: level 0 spells that characters can cast at will, without preparing or expending slots.
- level 1-9 require casters to spend a spell slot of that level or higher.
  - Some spells become more powerful when cast using a higher slot.
- Spell slots are recovered at long rests.
- **Rituals**: spells that can be cast as rituals (if class allows), taking 10 minutes longer to cast but not expending a slot.
- Some spells allow casting using the bonus action. If you do so, you can't cast another spell as an action (except a cantrip with casting time of 1 action).
- Casting times of several rounds require **concentration**. If **concentration** is broken, the spell fails but you don't expend the slot.

Casting:

1. Fulfill the spell's requirements:
   - **V**erbal (words)
   - **S**omatic (gestures of a free hand)
   - **M**aterial
     - Free materials can be replaced by a **component pouch** or a **spellcasting focus** (e.g. the Bard's instrument).
     - Some materials are consumed.
     - Requires a free hand, but may be the same as for the somatic components.
2. Expend a spell slot of the spell's level or higher.
3. If required, maintain **concentration** while casting or for the duration of the spell.
   - You can end concentration at will (no action required).
   - Casting another spell requiring concentration ends the current concentration.
   - Taking damage ends concentration unless you succeed a **Constitution saving throw**.
     - DC is 10 or half the damage, whichever is higher.
   - Being incapacitated or killed ends concentration.
   - The DM may issue a **DC 10 Con saving throw** in other situations that defy concentration.

## Damage types

For resistances and vulnerabilities:
- Acid
- Bludgeoning
- Cold
- Fire
- Force (pure magical energy)
- Lightning
- Necrotic
- Piercing
- Poison
- Psychic
- Radiant
- Slashing
- Thunder (burst of sound)

**Resistance** means a creature takes just half the damage of that type. **Vulnerability** means it takes double.

## Conditions

Lighting:
- Lightly obscured (dim light, fog): disadvantage on perception checks that rely on sight.
- Heavily obscured (darkness): creature is blinded.
- Bright light: normal.
- Dim light: shadows, boundary between a source of bright light and darkness, normal.

Suffocating:
- A creature can hold its breath for *1 + Con modifier* minutes (min 30 seconds).
  - After that, it resists *Con modifier* rounds (min 1) before dropping to 0 HP.

Grappled:
- The grappled creature can use its action to repeat the grappling contest and try to release itself.
- The grappling creature can move the grappled one.
  - At half speed as long as creature is not two or more sizes smaller.

Instant death:
- If damage reduces you to 0 HP and the remaining damage equals or exceeds your Max HP, you die instantly.
- Generally monsters die instantly as soon as their HP is 0 (important NPCs and villains may behave like PCs).
  - PCs can still choose to knock a creature out instead of killing it when they reduce its HP to 0 (it becomes stable)

Unconscious:
- If your HP is reduced to 0 (but above `- Max HP`) you fall **unconscious**.
- You regain consciousness if you regain any HP.
- Whenever you start your turn with 0 HP, you must make a **death saving throw**.
  - Roll a d20, 10 or higher you succeed. 9 or lower you fail.
    - Rolling 1 counts as two failures.
    - Rolling 20 regains you 1 HP and consciousness.
  - 3 successes make you **stable**.
  - 3 failures and the character dies.
- Any additional damage counts as a failed **death saving throw**.
  - Critical hits count as two **death saving throws**.
  - If the damage equals or exceeds your Max HP, you die instantly.
- You can attempt to stabilize an unsconscious creature with a DC 10 *medicine* check.

Stable:
- Stable creatures have 0 HP but don't need to make **death saving throws**.
- If you take damage while stable, you start making **death saving throws** again.
- A stable creature regains 1 HP after `1d4` hours.

Temporary HP:
- Bonus HP in a different pool
- Doesn't stack, each time you regain temporary HP you have to choose which amount is the new one
- Healing doesn't restore temporary HP
- A long rest clears temporary HP

## Mounts

- You can mount willing creatures that are at least a size larger than you.
- Mounting or dismounting takes half your speed.
- If the mount is moved against its will, you must succeed a **DC 10 Dexterity saving throw** or fall off the mount, **prone**.
  - Same throw if you are knocked **prone** while mounted.
- If the mount is knocked **prone**, you can use your reaction to dismount.
  - Otherwise, you fall **prone** beside it.
- If the mount provokes an opportunity attack, the attacker may attack you or the mount.
- Controlled mounts have three actions:
  - Dash
  - Disengage
  - Dodge
- Independent mounts have their own initiative and actions (including attacking).

## Rests

- Short rest
  - 1h long, eating, drinking, reading, healing
  - Spend a Hit Die and regain that much HP + Con modifier
    - Repeat until wanted (or max Hit Dice)

- Long rest
  - 8h long, 6h sleeping + 2 resting
  - Regain all HP
  - Regain half Hit Dice (min 1)
  - Must have at least 1 HP to long rest
  - At least 24h between long rests

## Inspiration

When a player roleplays in a distinguished way, the DM can reward them with Inspiration (doesn't stack).
A player with Inspiration can 
1. Spend it to gain advantage on a roll (attack, check or saving throw).
2. Give it to another player to reward them for roleplaying (also spends it).

## Coin

- 1 Copper (cp)
- 1 Silver (sp) = 10 Copper, laborer for half a day
- 1 Electrum (ep) = 5 Silver
- 1 Gold (gp) = 10 Silver, skilled artisan for a day
- 1 Platinum (pp) = 10 Gold

Advice on selling:
- Generally, used equipment in good condition can be sold for half the price.
- Magic items other than potions and scrolls should be difficult to sell.
- Jewelry and art items retain full value but may require finding a specific buyer.
- Trade goods retain full value and can be used as currency.

## Equipment

Example items:
- **Potion of Healing**: Restores 2d4 + 2 HP. Drinking or administering it takes an action. 50 gp
- **Quiver**: Holds 20 arrows. 1 gp
- **Ration**: Food for 1 day. 5 sp
- **Torch**: Provides bright light in a 20-foot radius, dim light up to 40. Burns for 1 hour. Attacks for 1 fire damage. 1 cp
- **Thieves' tools**: Proficiency in these tools lets you add proficiency bonus to disarm traps or open locks. 25 gp

Vehicles:
- **Horse**: 60 ft speed, 75 gp 
- **Rowboat**: 1.5 mph, 50 gp
- **Sailing ship**: 2 mph, 10000 gp
- **Warship**: 2.5 mph, 25000 gp

Lifestyle expenses:
- Paid between adventures for the days the PCs are not exploring.
- **Modest**: 1 gp per day
- **Comfortable**: 2 gp per day
- **Wealthy**: 4 gp per day
- **Aristocratic**: minimum 10 gp per day

Inn stay (per night):
- **Modest**: 5 sp
- **Comfortable**: 8 sp
- **Wealthy**: 2 gp
- **Aristocratic**: 4 gp

Meals (per day):
- **Modest**: 3 sp
- **Comfortable**: 5 sp
- **Wealthy**: 8 gp
- **Aristocratic**: 2 gp

**Hireling**: 2 gp per day (minimum)

## Downtime

You can spend days of downtime in various activities.
- Crafting
  - Item of up to 5 gp market value per day, spending half that gp in raw materials
  - Players can combine efforts
  - Discount of 1 gp in lifestyle
- Practicing a profession
  - Provides Modest lifestyle
  - If member of an organization (such as a guild), provides Comfortable lifestyle instead
  - If proficient in Performance, provides Wealthy lifestyle instead
- Recuperating
  - 3 days = DC 15 Con saving throw, choose:
    - End one effect on you that prevents regaining HP
    - Advantage on saving throws against one disease or poison affecting you
- Researching
  - Costs 1 gp per day
  - DM sets number of days and any investigation/persuasion checks needed
- Training
  - DM sets number of days and any checks to find instructor
  - 250 days and 1 gp per day to gain proficiency in a new tool or language

## Customization

Power players can have additional ways of customizing their characters.

### Multiclass

It's possible to multiclass, spending levels in more than one class for your character. To gain levels in a new class, you must have at least 13 in the class's primary ability (e.g. 13 Str or Dex for Fighter). Your total level becomes the sum of all class levels.

### Feats

Any time you gain a level that earns you an ability score improvement, you can instead gain a feat.

Example feats:
- Alert: +5 to initiative; can't be surprised while conscious; attacks by unseen enemies don't have advantage.
- Dual wielder: +1 AC while wielding two weapons; can dual-wield non-light weapons; you can draw or stow two weapons when you would be able to draw or stow only one.
- Durable: +1 to Con (max 20).
- Observant: +1 to Int or Wis; you can read lips; +5 to passive perception and passive investigation.
- Sharpshooter: Long range attacks don't cause disadvantage; ranged attacks ignore half cover and three-quarters cover; choice to take a -5 penalty to a proficient ranged weapon attack, to deal +10 damage.
- Skilled: Proficiency in three skills or tools of your choice.
- Tough: Your Max HP gains 2 * level.