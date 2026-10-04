KRNR Ship Transfer Demo
=======================

Purpose
-------
A player-only diplomatic action for Kaiserreich + Kaiserreich Naval Rework
that transfers ships to another country by type and amount, or by name.

It does not copy or replace KR ship-name files. KR uses name themes and
fallback names, so a static list would be incomplete and would show many
ships that do not exist in the current game.

Workflow
--------
The action is player-only (is_ai = no). In the window, the round controls
add or subtract 1; hold Ctrl for 5 or Shift for 10. The amount boxes never
go below 0.

The two buttons at the bottom are persistent options, not one-shot actions,
and they are mutually exclusive:

  * neither selected  -> transfer by the selected types and amounts
  * Random ships      -> same per-type transfer, kept as an explicit mode
  * Named TANGO ship  -> transfer the single ship that was renamed to TANGO

Click a mode to activate it (the button shows a pressed state); click it
again to cancel and go back to the plain transfer.

Finally click the native Submit button at the bottom of the diplomatic
action window. Cancel performs no transfer.

Type mapping: submarine, destroyer, light cruiser, heavy cruiser,
battlecruiser, battleship, carrier, and support_ship (the support_ship
mapping covers KR support hulls).

For one named ship: rename it to the short marker TANGO, activate the
Named TANGO mode, select exactly one type with quantity 1, and confirm.

Limitations
-----------
The demo intentionally uses a marker name instead of trying to enumerate
current ships. It does not restore the original name automatically, and two
ships with the marker name may make the selected ship ambiguous. Free-form
text entry is deliberately not used; TANGO is short and easy to type in the
normal ship rename dialog.

KR repair ships use a separate type (repair_ship) and do not have their own
button yet.

Test this in a copy of the save and check logs/error.log after enabling.
