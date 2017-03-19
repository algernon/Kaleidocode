<!-- -*- mode: markdown; fill-column: 8192 -*- -->

# Working with the EEPROM

Let us start at the very beginning: just what is this `EEPROM`, and what is it useful for? The `EEPROM` is a kind of memory on the keyboard, one that persists across reboots, and which can be easily written to while the keyboard is in operation. I like to call it the long-term memory of the keyboard. But to understand why it is useful, we will need to look at the two other kinds of storage on our devices: program memory, and `SRAM`.

Program memory is where the firmware lives, where the code runs from. It cannot be changed at run-time, and to change it, we have to upload - or flash - a new firmware. On the chips used by the keyboards Kaleidoscope supports, we usually have about 28 kilobytes of this. Not much, but still a plenty!

Unlike program memory, `SRAM` can be changed at run-time, and this is where all dynamic data is stored. Counters, timers, reports, and everything else the keyboard firmware needs to operate, and modify during its normal operation. We have two and a half kilobytes of this, more than the amount of `EEPROM`, but changes to `SRAM` are not peristent: if we plug our keyboards out, every update made to it, will be lost.

And this is where `EEPROM` comes in, as a middle ground between the two others: smaller than both, but writeable, and persists across reboots. This makes it the perfect storage for settings and other kinds of data that we may want to update, without updating the rest of the firmware too. Such things include the key layout, LED color schemes, and a number of other things.

However, using the `EEPROM` is not entirely trivial, there are many ways one can end up in incredibly confusing situations. Within this chapter, we will be looking at ways to avoid that. We will learn how to avoid the most common issues, and how to roll back, if we end up in a bad place.

* [The layout of the EEPROM](layout-of-the-eepom.md)
