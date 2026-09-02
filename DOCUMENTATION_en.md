# RimLogic — guide

A full account of how the mod works: what networks it has, how blocks talk to each other, what links are and how they differ from simply standing next to one another. If you are just starting out, read straight through — the sections go from simple to involved.

For a short overview and the block list, see the [README](README.md).

---

## Contents

1. [Two networks: signals and power](#1-two-networks-signals-and-power)
2. [The logic network](#2-the-logic-network)
3. [Links between blocks](#3-links-between-blocks)
4. [Placing blocks side by side](#4-placing-blocks-side-by-side)
5. [The power network](#5-the-power-network)
6. [Blocks and their types](#6-blocks-and-their-types)
7. [Example circuits](#7-example-circuits)

---

## 1. Two networks: signals and power

Every block in RimLogic leads **two separate lives**, and it is worth keeping them apart.

**The logic network** carries signals. This is how blocks talk to one another: the sensor reports that it has gone dark, the logic decides the lights should come on, the speaker announces it. Signals have nothing to do with energy and power nothing.

**The power network** is RimWorld's ordinary wiring. Blocks can tie into it: some simply draw power, others pass current through themselves and act as switches you can control.

One block can take part in both at once. A light sensor, for instance, puts out a signal **and** passes current from its electrical input to its output at the same time — it is both a source of information and a switch.

Every block has four sides, and the role of each side is set separately for each network, with two buttons: **"Power sides: input/output"** and **"Signal sides: input/output"**. Any side can be an input, an output, or switched off.

---

## 2. The logic network

### Plain signals

The foundation of everything. A plain signal is a yes or a no: it is either there or it is not. Present means one, absent means zero. That is how most blocks work — the motion sensor spots someone and a signal appears; they walk away and it goes.

### Complex signals

Some blocks send more than a yes: they send a **number**. That is a complex signal. Memory, the math block, the selector, the crafter and the logic converter can produce one; memory, the math block and the screen can read one.

There is an important difference from plain signals: a complex signal has no default zero. Either it carries information, or it is not there at all.

### The status table

Blocks that report on their own work use **codes shared across the whole mod**. Learn them once and every such block becomes readable:

| Code | Meaning |
|---|---|
| 0 | no power |
| 1 | on, nothing to do |
| 2 | switched off by signal |
| 3 | working |
| 4 | started work (brief pulse) |
| 5 | finished work (brief pulse) |
| 6 | queue empty |
| 7 | not enough materials |
| 8 | nowhere to put finished goods |

The numbers are only the format they travel in. You never have to memorise them: the state is always written out in words in the block's inspect pane, and the **logic converter** can catch a given code and turn it into a plain signal.

Codes 4 and 5 are not states but events: they last only a moment and mark when work began or ended.

### How signals travel

Signals do not update instantly — they move in short ticks. Each hop along a chain takes one tick, so a signal passing through three blocks arrives three ticks later. The delay is small, and it is what keeps circuits with loops and feedback from tying themselves in knots.

---

## 3. Links between blocks

A link is a signal wire. It joins the **logic output** of one block to the **logic input** of another, and is drawn on the map as a line.

### Running one

1. Select the block and press **"Add link"**.
2. Click this block's output (if it only has one, it is picked for you).
3. Click the block you are connecting to, then its input.

While you are drawing the line, clicking empty tiles drops **route points**. The line then goes through them instead of straight across — handy for routing around buildings, or simply for running it neatly along a wall.

Right-click steps back: first it drops the chosen target, then it removes the last route point, and only then leaves the mode. Esc cancels everything at once.

### Removing one

Hover over the link line and right-click.

### Seeing every link

Normally lines show only for the selected block. The **"Show all RimLogic links"** button in the bottom-right corner puts every link on the map at once — invaluable once a circuit has grown.

### Worth knowing

- A link always runs from an output to an input. Two outputs or two inputs cannot be joined.
- One output can feed several blocks: just run several links from it.
- The line on the map is only a drawing. It routes itself around mod blocks and redraws when you build or remove something. How it looks has no bearing on how the circuit works.
- Deconstruct a block and all its links vanish with it.

---

## 4. Placing blocks side by side

This is the nicest thing about the mod: **neighbouring blocks need no links at all**.

Put blocks flush against each other so that **one block's output faces the other's input**, and the signal simply passes across. No line to draw.

Here is how it works: a block looks at the tile beyond its input side. If another mod block stands there and **the side facing us is set as an output**, the signal is taken.

That has a few practical consequences:

- **Circuits can be built as a chain**, just by placing blocks in a row. Sensor, then logic, then speaker — all working without a single link.
- **Side direction is everything.** If a signal is not getting through, the first thing to check with "Signal sides: input/output" is that one block has that side as an output and its neighbour has it as an input.
- **A neighbour outranks a link.** If a source block stands flush against an input, that input counts as taken and a link can no longer be run to it. Move the neighbour or change the sides first.

The same goes for power: blocks standing flush with active electrical sides pass current to each other without any wire.

---

## 5. The power network

Here RimLogic works with RimWorld's ordinary power system and invents nothing of its own. Blocks come in two kinds.

### Consumers

They simply draw power and connect to the nearest line like any ordinary building. Current does not pass through them.

| Block | Draw |
|---|---|
| Register, timer, memory | 5 W |
| Signal generator, tick generator | 10 W |
| Clock, speaker | 15 W |
| Screen | 25 W |
| Crafter block | 500 W |

A consumer short of power stops working — and the ones that report their state will say so with code 0.

The **"Reconnect power"** button lets you choose which line feeds a block. That matters when several branches run nearby and you want the block sitting on, say, the backup rather than the main one.

### Conductors

Sensors, logic gates, the switch, the selector, the splitter and the diode use almost no power themselves, but **pass current through** their active electrical sides. That turns them into switches you can control: the light sensor does not merely report darkness, it physically feeds the lamps.

The **diode** deserves a mention of its own: it passes current one way only, and only while the input side has power to spare. With it a grid can be split into sections — the surplus flows into a backup branch without that branch draining anything back.

---

## 6. Blocks and their types

The mod has three **general-purpose blocks**: you build one thing, and choose what it becomes after placing it, with the **"Type"** button. Only researched types are listed.

Note: changing the type resets the block — side roles, every link and every output. Settle on the type before you run wires to it.

---

### Logic block

Conditions are built out of these. Every type in this group has several inputs and one output.

#### AND

Outputs a signal **only while every connected input has one**. Let one go quiet and the output goes dark.

It works with power too: current passes only when all inputs are live.

This is the "both of these" block. The turret comes on if the motion sensor fired **and** it is night. The door opens if someone approached **and** there is no alert.

#### OR

Outputs a signal **if at least one input has one**. Same with power: current flows while any input is live.

The "either of these" block. The siren sounds from the fire sensor **or** the motion sensor — whichever gets there first.

#### XOR

Outputs a signal **when the inputs differ**: one has a signal, the other does not. If both are on, or both are off, the output goes dark.

It catches disagreement. Say two sensors watch the same thing and you want to know the moment their readings part ways.

#### NAND, NOR, XNOR

The same as AND, OR and XOR, but **inverted**. Signals only — no wires attach to these.

- **NAND** — a signal at all times **except** when every input has one. Good for things that should run by default and stop when conditions meet.
- **NOR** — a signal **only while nothing has one**. This is your "all quiet" condition.
- **XNOR** — a signal while the inputs **agree**. The "readings match" condition.

#### Splitter

Takes one signal and **sends it out through every one of its outputs**, identical and simultaneous. Works with power as well.

You want it whenever one sensor should drive several blocks. Without it you would need a separate link from the sensor to each — and a sensor usually has just the one output.

#### Logic converter

Translates one kind of signal into the other. It has two switches — **what to expect on the input** and **what to send on the output** — and each offers the same two choices: a plain signal or a number.

Which gives four combinations:

| Input | Output | What you get |
|---|---|---|
| plain | number | tags an event with a number |
| number | plain | catches a given number and turns it into a signal |
| plain | plain | simply repeats the signal |
| number | number | swaps one number for another |

The second is the main use. Blocks like the crafter report their state as a number (see the [status table](#the-status-table)), while the speaker, the switch and the gates only understand plain signals. The converter bridges the two: set the input to number **7** and it fires exactly when the crafter has run out of materials.

When the condition is not met the block stays silent: a plain output goes dark, and a numeric one sends nothing at all.

---

### Sensor

Every circuit starts at a sensor: first you notice something, then you decide what to do about it.

Every sensor except the power sensor works in **two roles at once**: it gives a signal and passes current from its electrical input to its output while the condition holds. So a light sensor is both a source of information and the switch for your lamps.

#### Light sensor

Watches the **light level on its own tile**. The settings hold a threshold as a percentage and a condition (above or below).

Place it where the light needs measuring, not where the lamps are — it looks at the tile beneath itself.

#### Motion sensor

Reacts to presence in the **area around it**. Two things are set in its settings: **who to notice** (colonists, tame animals, wild animals, slaves, prisoners, neutral visitors, enemies — several at once if you like) and **the area to check**.

The area comes in three shapes, and this is the same for every block that checks an area:

- **circle** — a radius in tiles, set with a slider;
- **square** — the same, but square;
- **room** — the room the block stands in; outdoors it falls back to a small area around the block.

The room shape is the most convenient indoors: no radius to fiddle with, and the area matches the walls exactly.

#### Temperature sensor

Watches the **temperature on its own tile**. The condition is set in the settings: above or below so many degrees.

A pair of these with a heater and a cooler keeps a room in range without you lifting a finger.

#### Power sensor

The one sensor that **does not pass current** — it only watches. Its electrical input is there so it can tie into the grid and see its figures.

The settings choose what it watches:

- **spare power in the grid** — what the grid is putting out right now, in watts;
- **battery charge** — how much is stored, in watt-days.

And how to check it: an exact figure with a condition (above, below, equal), or a range from one value to another.

#### Pollution sensor

Checks for **pollution in an area**. The area is set the same way as for the motion sensor.

#### Fire sensor

Fires while **something is burning in the area**. The area is set the same way.

The foundation of a fire-response circuit: that signal can drive the speaker, cut power to a section, or open doors.

---

### Functional block

The working hands of a circuit: they steer power, crunch numbers, and connect logic to ordinary buildings.

#### Switch

While the control input **has a signal**, current passes from the electrical input to the output. The signal stops and the circuit opens.

This is how you hand power over to logic. Any sensor or condition gains the ability to switch a whole section of the grid.

#### Selector

Sends current to **one of two outputs** — left or right, never both at once. Each new signal on the control input flips it to the other side.

Between flips it stays closed on the chosen side. At the moment it switches it also sends a brief pulse to its logic output, so the circuit can tell that the side has changed.

#### Math block

Takes the number from the **left input** (the first) and the **right** one (the second), performs the operation you chose, and sends the result to the output.

The operations fall into two groups.

**Arithmetic:**

| Operation | What it does |
|---|---|
| add, subtract, multiply, divide | ordinary arithmetic |
| remainder | what is left after dividing whole |
| power, square root | raising and extracting |
| absolute value | the number without its sign |
| difference | how far apart the numbers are, unsigned |
| percent | what percentage one is of the other |
| negate | plus becomes minus |

**Comparisons** — and these matter most, because a comparison turns a number back into a plain signal.

| Operation | Signal appears when |
|---|---|
| equal, not equal | the numbers match, or do not |
| greater, greater or equal | the first is above the second |
| less, less or equal | the first is below the second |
| approximately equal | the numbers are close |

That is how a number becomes a condition: "steel below 200", and the circuit raises the alert itself. The second number can come from memory or be typed in by hand.

#### Multitool

The one block that connects a circuit to the ordinary life of the colony: to items, crops, doors and storage. The mode is chosen in its settings, along with the area to check — circle, square or room.

The modes fall into two groups, and the difference matters: some **send a number out**, others **take a signal in**.

**Counting modes.** Their output is a complex signal, so it needs somewhere to go: a screen, memory, or the math block.

| Mode | What it counts |
|---|---|
| **Count items** | how much of a chosen item lies in the area — on the ground and in storage alike |
| **Crop maturity** | how many plants in the area are ready to harvest |
| **Count pawns** | how many pawns in the area match the filter: by group (colonists, animals, enemies, prisoners and others) or one specific pawn |
| **Storage fullness** | how full the area's storage is, as a percentage |

**Control modes.** These wait for a signal instead.

| Mode | What it does |
|---|---|
| **Toggle device** | switches a neighbouring device on and off by signal |
| **Open/close doors** | opens and closes doors in the area by signal |

The counting modes pair naturally with the math block: it compares the number against a threshold and turns it into a plain signal. "Steel below 200", or "storage over 90% full" — and from there, the speaker or a halt to production.

---

### Standalone blocks

These are built on their own and their type never changes.

#### Diode

Passes current **one way only** — from input to output — and only while the input side has **power to spare**. With no surplus, the circuit opens.

This is how a grid gets split into sections: the surplus flows into a backup branch, and nothing drains back out of it even if its batteries run flat.

A control signal switches the diode on and off. Its logic output carries a signal whenever current is actually passing, so you can see whether the transfer is really happening.

#### Signal generator

It simply **holds a signal on its output** while it is running. A source of constant "on".

You want it when something needs a signal that never lapses: holding a door open, keeping a block from switching itself off, feeding a condition that is always true. A logic input can interrupt it.

#### Tick generator

Sends out signals as **even pulses**, over and over, with the same gap between them.

This is the circuit's clock. Counters count by its pulses, sequences step through them, repeating actions fire on them. A logic input can interrupt the beat.

#### Register

A block that remembers one thing: **on or off**.

Each new signal on its input **flips it to the other state**: on becomes off, off becomes on. Between signals it holds that state by itself, however long the wait.

It works like a light switch on the wall: press it and the light is on, press again and it is off. One and the same signal both switches on and switches off. Without a register you would need two different sources to do that.

It reacts to a signal **arriving**, not to a signal being present: a signal that stays on flips the state once, rather than making it flicker.

#### Timer

Repeats the incoming signal on its output **after a delay**. The length of the delay is set with a slider.

When a signal arrives the timer waits out the delay and only then passes it on. From there, two things can happen:

- if the signal is **still there** by then, the output holds too;
- if it has **already stopped**, the output gives a brief pulse instead.

Use it so lights do not snap off the same second, doors close a moment later, and steps in a circuit follow one another with pauses.

#### Memory

It **stores a number**. What it remembers depends on the source:

- from a **plain signal** — it remembers 1 or 0;
- from a block that carries a **number** (the selector, the math block, the multitool, another memory) — it remembers that number.

The value stays until a new one arrives. The settings let you set the current value by hand and reset it with a button.

**Every time the value changes** memory sends a brief pulse to its output. That is the "data has been updated" signal, handy for kicking off a recalculation or an alert.

Memory is the only block that lets a circuit remember something between events: it can hold a sensor reading taken at the moment of an alarm, to compare against later.

#### Screen

Shows **a single digit** of the number arriving at its input. One digit, not the whole number — that is the thing to know.

To display a longer number you place **several screens and connect them all to one source**. They then sort out the places between themselves: **the first one connected shows the units, the second the tens, the third the hundreds**, and so on. The order comes from the order in which you ran the links, not from where the screens stand on the map — so run them left to right in the order you want.

If the number is negative, a minus sign appears on the highest screen.

A screen reads its source's value continuously, even while the source is silent. That is why a memory holding a number shows on the display the whole time instead of flickering on updates.

#### Clock

Gives a signal at a **chosen time of day**. In the settings you pick an hour or a span of hours and a condition: before, after, within a range and so on. While local time matches, the output carries a signal.

The backbone of any schedule: lights in the evening, machines by day only, non-essentials off overnight.

#### Speaker

On a signal it **puts up an in-game notification** — an ordinary blue one or a red alert, with your own heading and text. All of it configurable.

This is the circuit's voice. A sensor or a calculation tells you about a fire, a power shortage or an empty store by itself, instead of you watching the readings. The signal also carries on to the output, so the speaker can sit in the middle of a chain.

#### Crafter block

The most involved block in the mod, so in detail.

**Where it goes.** On a bench's **work tile** — the one a colonist would stand on. The game normally refuses to build there, but an exception is made for the crafter. Put it anywhere else and it will say plainly in its inspect pane that it is not on a work spot.

**What it does.** It works through the **bench's own queue** — the one you set up in the bench. Nothing to configure on the block itself: it makes what is listed there and honours every setting on those bills, including "do X times" and suspension.

**It takes the bench.** While the block is in place, colonists will not use that bench — just as they would not step up to a machine someone is already working at.

**How it does it.** The block produces nothing by itself. Everything that happens at a bench in RimWorld goes through a worker: bench speed, fuel use, the quality of the piece, butchering and smelting. A building cannot take part in that conversation. So the block puts out a **manipulator** — a service unit standing on its tile, which the game hands an ordinary work job. From there the game itself does the counting, which is why the block works correctly at benches from other mods too.

The manipulator is not drawn — it is part of the block, not a creature of its own. It does not leave its post either: it may step across to the neighbouring storage for materials and comes straight back. It will not carry finished goods around the base — the block does that itself.

**Materials.** It takes them from storage **when work begins**, like a colonist carrying supplies to the bench: they sit at the bench and are spent at the moment the item is finished. If the job is interrupted — power lost, the bench removed, the bill deleted — the supplies stay by the bench, and colonists will put them away as they would anything else.

**Storage.** Its settings choose where materials come from and where finished goods go. Only storage **touching the block** counts: the crafter is meant to stand in its own supplies. You can leave it at "all nearby" or tick specific ones.

Before starting, the block checks that the result **has somewhere to go**. If there is no suitable space, it will not begin — no sense spending materials on something it cannot put down.

**Speed and quality** follow its level. It works about a third more slowly than a living crafter of the same skill: a machine trades finesse for never tiring and never being called away.

**Levels** are bought with materials, taken from colony storage. Each step costs more than the last, so a fully trained crafter is a serious investment.

**Status** goes out as a number, on the [shared table](#the-status-table). The same thing is written in words in the inspect pane, and the work itself shows on the ordinary progress bar above the bench — the same one a colonist gets.

**What it can do.** Any recipe the bench offers, butchering and smelting included: the game works out the result exactly as it would for a colonist. It only passes over work that belongs to a particular person — bills with a chosen worker, "slaves only", "mechs only" — and recipes its level is not high enough for.

---

## 7. Example circuits

### Lights at dusk

Light sensor → lamps.

Set the sensor to a low light level, feed power into its electrical input, and run a wire from its output to the lamps. That is all: the lights come on as it gets dark. No links needed at all.

### Fire alarm

Fire sensor → speaker.

Place the speaker flush against the sensor so the sensor's output faces the speaker's input. Write your own text in the speaker's settings and pick the red notification. Now a fire announces itself.

### Lights only at night, and only when someone is there

Clock → AND ← motion sensor, then AND → switch → lamps.

The clock signals during night hours, the sensor while someone is in the room. AND passes the signal only when both hold.

### Shedding load when the batteries run low

Power sensor → switch.

Set the sensor to a low battery charge and run everything you can live without through the switch. When power gets tight, the circuit sheds the load by itself.

### The crafter says it is out of materials

Crafter → logic converter → speaker.

In the converter set the input to "complex" with the number **7** (out of materials), and the output to "plain". Now the speaker tells you the moment the block runs dry.

### A warning when steel runs low

Multitool → math block → speaker.

Put the multitool in "count items" mode, choose steel and give it a generous area — it will report the amount as a number. Set the math block to "less than" with 200 as the second number. Once the stock drops below, the comparison becomes a plain signal and the speaker warns you.

Storage fullness works the same way: the same multitool in another mode, "greater than 90", and you know the store is about to seize up.

### Production on a schedule

Clock → crafter.

Run the clock's signal into the crafter's logic input: while the signal is there it works, and when it stops it halts. That keeps the machines running by day only.
