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

Two types of weapons:
- Melee
  - Attack targets within 5 feet (throw attack is considered Improved Weapon, 1d4, normal range 20 feet, long range 60 feet).
- Ranged
  - Attack targets at a distance.

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

Two axis:
1. Lawful, Neutral or Chaotic
2. Good, Neutral or Evil

### Leveling up

Starting level is 1. Max level is 20.
Leveling up can be through gaining XP (Experience Points) or by campaign milestones.

When a class provides an Ability Score Improvement while gaining a level, this means you can improve an Ability Score by 2 or two different Ability Scores by 1 each (up to 20).

## Races

Define, among others:
1. Ability Score Increases at the start (e.g. +2 Constitution, +1 Wisdom)
2. Speed (e.g. 25-35 feet)
3. Special features (e.g. Darkvision)
4. Resistances / weaknesses
5. Proficiencies
6. Languages

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

Typical DCs:
- Very easy: 5
- Easy: 10
- Medium: 15
- Hard: 20
- Very hard: 25
- Nearly impossible: 30

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

## Combat

When combat begins:
1. Roll initiative: Dexterity check to determine order of creatures' turns.

Attack action:
1. Attack roll
  - Melee attack roll: d20 + proficiency bonus (if proficient with the weapon) + strength modifier
  - Ranged attack roll: d20 + proficiency bonus (if proficient with the weapon) + dexterity modifier
2. Damage roll
  - Die indicated by the weapon + ability modifier (but not proficiency bonus).

Movement, Actions, Bonus Actions?

## Magic

Different abilities can be used for spellcasting (e.g. Intelligence, Charisma)

- Spell Save DC = 8 + Proficiency Bonus + Ability modifier
- Spell Attack modifier = Proficiency Bonus + Ability modifier

Cantrips

Rituals

Spells

1. Spend a spell slot of the spell's level or higher (spell slots recover at long rests).

Spellcasting focus (e.g. Bard's instrument)

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
- **Comfortable**: 2 gp per day
- **Wealthy**: 4 gp per day
- **Aristocratic**: minimum 10 gp per day

Inn stay (per night):
- **Comfortable**: 8 sp
- **Wealthy**: 2 gp
- **Aristocratic**: 4 gp

Meals (per day):
- **Comfortable**: 5 sp
- **Wealthy**: 8 gp
- **Aristocratic**: 2 gp

**Hireling**: 2 gp per day (minimum)

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