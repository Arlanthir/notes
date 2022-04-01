# Netrunner

- Android: Netrunner / Project NISEI
- Asymmetric card game
- 2 players

## Intro

A hacker (**Runner**) tries to hack a corporation (**Corp**) and gain access to valuable information (**Agendas**).
The Corp tries to protect itself with **ICE** (Intrusion Countermeasures Electronics) until the Runner runs out of resources.

The Corp tries to install Agendas in its servers and **advance** them to score them.

The Runner only has **access** (see) the Agenda cards in any place (board, deck, hand, discard pile) to score them.

The Corp always has 3 central servers:
- HQ (hand, maximum 5 hand size)
- R&D (deck)
- Archives (discard pile, cards are trashed facedown or faceup depending on how they were in the board)

The Corp can also create any number of other (**remote**) servers by placing cards (Agendas, Assets or ICE) to the side of the central servers.

## Setup

### Corp

- Place your **Identity** card in the middle of the board. It may give you a special ability and represents your **HQ**.
- Place your deck (**R&D**) to the left of the Identity.
- The (empty) discard pile (**Archives**) is to the left of the R&D.
- Draw 5 cards.
  - Optionally mulligan once (shuffle and draw another 5 cards).
  - Recommended if you have more Agenda than ICE in hand.
- Gain 5 credits.

### Runner

- Place your **Identity** card in the middle of the board. It may give you a special ability and represents your **Grip** (hand). It also gives you a **memory** limit.
- Place your deck (**Stack**) to the left of the Identity.
- The (empty) discard pile (**Heap**) is to the left of the Stack.
- Gain 5 credits.

## Turn

The Corp takes the first turn (and draws the usual card). In its first turn, the Corp will probably want to gain credits and protect HQ and R&D.

### Corp Turn

Each turn, the Corp **draws a card** and has 3 **clicks** to spend on actions (see reference card).

1. Gain 3 **clicks** to spend on actions.
2. Resolve "When your turn begins" effects.
3. Draw 1 card.
4. Spend clicks on actions.
5. Discard down to maximum hand size (typically 5).

Available actions (cost 1 click unless stated otherwise):

- Gain 1 credit.
- Draw 1 card.
- Play an **Operation**
  - Pay the top left cost in credits.
  - Apply the effects of the card and trash it.
- Install an **Asset** or **Agenda**
  - (Optionally trash any cards in the root of the server)
  - Play it facedown in the root of an empty remote server (to the right of HQ).
- Install an **Upgrade**
  - (Optionally trash any cards in the root of the server)
  - Play it facedown in the root of any server.
- Install **ICE**
  - (Optionally trash any ICE already in the server)
  - Pay 1 credit for each ICE already on the server.
  - Play it sideways facedown (**unrezzed**) on top of a server.
- 1 credit: Advance 1 installed card.
  - If you fully advance an Agenda, you can reveal it and score it (during your turn).
  - You can continue to advance a fully advanced Agenda if you want (for bluffing).
- 2 credits: Trash 1 installed resource from a **tagged** Runner.
- Spend 3 clicks total: Purge virus counters.

At any time during the game, without spending clicks:
- **Rez** an Asset or Upgrade:
  - Spend credits equal to the Rez cost in the upper left
  - Turn the card faceup.

When the Runner approaches a piece of ICE, you can **Rez** it in the same way.

### Runner Turn

Each turn, the Runner has 4 **clicks** to spend on actions (see reference card).

1. Gain 4 **clicks** to spend on actions.
2. Resolve "When your turn begins" effects.
3. Spend clicks on actions.
4. Discard down to maximum hand size (typically 5).

Available actions (cost 1 click unless stated otherwise):

- Gain 1 credit.
- Draw 1 card.
- Play an **Event**:
  - Pay the top left cost in credits.
  - Apply the effects of the card and trash it.
- Install a **Program**, **Resource** or **Hardware**:
  - Pay the top left cost in credits.
  - Place the card in the appropriate section (Resources on the bottom, then Harware and Programs on top)
  - If it's a program, check if you have available memory or if you have to trash existing program(s). 
- Run any server
  - The Corp can **Rez** any **ICE** you approach.
  - After you pass any ICE you can **Jack Out** before approaching the next.
  - If you encounter Rezzed ICE, you suffer each subroutine unless you use **Icebreakers** to break them.
    - Your Icebreaker needs to break the appropriate type (barrier, sentry, codegate). Some Icebreakers break everything (e.g. Mayfly).
    - Your Icebreaker needs to have strengh >= than the ICE it's breaking.
    - You don't need to break every subroutine to continue the run, just the ones that say "End the run".
    - If you increase the strength of your Icebreaker, it resets after you pass that ICE (unless otherwise stated).
  - If the run is successfull, you **Breach** the server and **Access** it:
    - You can see every card installed in the root of that server.
    - HQ: You can see 1 cards at random from the Corp's hand.
    - R&D: You can see 1 card from the top.
    - Archives: You can see all the cards in the Corp's discard pile.
  - Some cards let you access multiple cards from HQ or R&D:
    - You see N different cards but one at a time (you have to decide whether to trash one before seeing another).
  - For each card you access:
    - If it's an Agenda, score it.
    - If it has a trash ability, you may pay to trash it.
  - See reference cards for details.
- Spend 2 credits: remove 1 **Tag**.

## Damage

Some cards deal damage to the Runner.

- Net/Meat Damage: The Runner trashes that many cards at random from their hand.
  - If they can't, they lose the game.

## End

The game ends in the following cases:
- One of the players scores 7 Agenda points (and wins).
- The Corp tries to draw from R&D and has no cards (and loses).
- The Runner **flatlines** (is dealt more damage than cards in hand, and loses).

# Deckbuilding

Each Identity has a faction (color), a minimum deck size (number in the left) and an influence limit (number in the right):

Your deck:
- Can include any number of cards from the faction of your identity.
- Can include cards from other factions as long as the total influence of those cards (bottom right vertical dots) is <= than your influence limit.
- Must have at least as many cards as the the minimum deck size of the identity.
- May have at most 3 copies of the same card.
- The Corp deck must include a number of Agenda points depending on the deck size:

| Deck size | Agenda points |
| --------- | ------------- |
| 30-34     | 14-15         |
| 35-39     | 16-17         |
| 40-44     | 18-19         |
| 45-49     | 20-21         |
| 50-54     | 22-23         |

Starter decks cards have solid dots near the portal set symbol in the bottom, stating how many copies of them you should include in the starter deck. Extended starter decks cards have plus symbols instead of dots.
