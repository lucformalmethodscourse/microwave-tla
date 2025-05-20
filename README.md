# TLA+ Microwave Model

[![Syntax-check models](https://github.com/lucformalmethodscourse/microwave-tla/actions/workflows/main.yml/badge.svg)](https://github.com/lucformalmethodscourse/microwave-tla/actions/workflows/main.yml)

This example models a very basic microwave oven with very simple state:

- door open or closed
- radiation on or off
- time remaining in seconds between zero and some maximum

The model has various actions:

- opening/closing the door
- starting/canceling the heater (magnetron)
- incrementing/counting down time

These typically correspond to buttons and other affordances in the appliance's user interface or internal events.

## Typing

TLA+ does not support static typing, but we can define a (dynamic) invariant `TypeOK` that constrains which variable can take on which values.

## Safety

Checking the intial model succeeds, but...

*What does it actually mean for the microwave to be safe?*

Typically, we wouldn't want it to be heating when the door is open. 
So let's define a `DoorSafety` invariant accordingly.

Now the initial model fails, and we need to fix it so it satisfies the invariant!

- stage 1: turn off when opening door
- stage 2: don't allow turning on when door is open

## Liveness

Is this notion of safety enough? 

*How do we know it won't accidentally keep running, overheat, and catch on fire?*

We need some kind of `HeatLiveness` property.
This detects whether the model could go into stuttering, i.e., staying in the same state without getting closer to finishing.

We need to enable weak fairness (WF) to break this stutter-invariance! 
(See also [www.hillelwayne.com/post/fairness](https://www.hillelwayne.com/post/fairness).)

We can explore other temporal properties, e.g., `RunsUntilDoneOrInterrupted`.

## Checking the model in various configurations

We have created separate configuration files reflecting the various possible model configuration settings.

| Configuration | Result | Description |
| --- | --- | --- |
| Microwave.cfg | success | unsafe, unchecked |
| MicrowaveChecked.cfg | safety failure | unsafe, checked |
| MicrowaveChecked2.cfg | safety failure | still unsafe, checked |
| MicrowaveSafe.cfg | success | safe and checked |
| MicrowaveStuttering.cfg | liveness failure | stuttering |
| MicrowaveLive.cfg | success | weak fairness |

To check the model in any configuration other than the default (`Microwave.cfg`), you need to specify the corresponding `.cfg` file as an option either interactively or on the command-line.

If you have Visual Studio Code installed, you can usually run the TLA+ model checker, TLC, as follows:

```
java -jar ~/.vscode/extensions/tlaplus.vscode-ide-*/tools/tla2tools.jar -config MicrowaveChecked2.cfg Microwave.tla
```

## Related publications

We have included this example in various formal methods education papers and presentations:

- [2025 TLA+ Community Event at ETAPS](https://conf.tlapl.us/2025-etaps/)
- [Dec 2024 IEEE Computer magazine](https://ieeexplore.ieee.org/document/10754605)
- [2024 IEEE Frontiers in Education conference (WIP)](https://ieeexplore.ieee.org/document/10893422)
