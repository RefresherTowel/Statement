# Statement

#### Turn your states into a Statement.

Statement is a lightweight but feature rich state machine framework for GameMaker, designed to be your new go to for state logic. Instead of juggling state variables and giant switch blocks, each object gets a clean Statement machine with named states and clear Enter / Update / Exit / Draw handlers. You can keep things simple, or turn on more advanced features when you need them.

This repository is for the Statement framework landing page and issue tracker.

## Features at a glance

- Clean, fluid API that drops into any GameMaker project with only a few lines of code.
- Named states with built in Enter / Update / Exit (and optional Draw) handlers.
- Queued transitions so you can request a state change now and apply it safely later.
- Declarative transitions that automatically switch when your condition is true.
- State stacks (PushState / PopState) for pause menus, overlays, and temporary states.
- Transition payloads to pass data into state changes without polluting globals.
- Non interruptible (exit locked) states for things like stagger, attacks, and cutscenes.
- Per machine pause flag so you can freeze logic without tearing anything down.
- Optional per state timers backed by time sources.
- Comes with Echo, a small debug logging framework, included for free.

## Quick start

A minimal example: one object, one machine, two states.
```js
// Create Event
sm = new Statement(self);

var _idle = new StatementState(self, "Idle")
    .AddEnter(function() {
        image_speed = 0;
        image_index = 0;
    })
    .AddUpdate(function() {
        if (keyboard_check(vk_anykey)) {
            sm.ChangeState("Move");
        }
    });

var _move = new StatementState(self, "Move")
    .AddEnter(function() {
        image_speed = 1;
    })
    .AddUpdate(function() {
        var _right = keyboard_check(vk_right) || keyboard_check(ord("D"));
        var _left  = keyboard_check(vk_left)  || keyboard_check(ord("A"));
        var _hor   = (_right - _left);

        if (_hor == 0) {
            sm.ChangeState("Idle");
        } else {
            x += _hor * 2;
            image_xscale = sign(_hor);
        }
    });

sm.AddState(_idle).AddState(_move);
```

Then, in the Step event:

```js
sm.Update();
```

For more advanced examples (stacks, queues, payloads, exit locks, etc) see the full documentation.

## Documentation

Full online docs are available here:

### Statement docs:
https://refreshertowel.github.io/docs/statement/

### Where to buy

Statement is available on itch.io:

Itch page:
itch url here

### Bug reports and feature requests

The best place to report bugs or request features is the GitHub Issues page:

Issues:
https://github.com/RefresherTowel/Statement/issues

If you are not comfortable using GitHub, you can also post in the [**Statement channel on the RefresherTowel Games Discord**](https://discord.gg/8spFZdyvkb) and I can file an issue for you.

### License

Statement is a paid framework. The license terms for using it in your projects are in:

[LICENSE_Statement.txt]()

In short: it is licensed per developer seat, you can use it in unlimited games (free or commercial), but you cannot redistribute the framework source itself.

### Credits and third party assets

Example projects for Statement may use third party art assets (for example, Kenney assets from kenney.nl). These are provided under their own licenses and are not covered by the Statement framework license. See the example project CREDITS.txt for details.
