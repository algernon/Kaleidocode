<!-- -*- mode: markdown; fill-column: 8192 -*- -->

## The layout of the EEPROM

Deep down the `EEPROM` is just a sequence of bytes. However, we have a number of semi-independent plugins, which all want to store some data in there. Therefore, we need a way to coordinate this, preferably in a way that makes it possible to detect when the firmware and the `EEPROM` are out of sync. This is where the [EEPROM-Settings][plugin:eeprom-settings] plugin comes in. This plugin is the coordinator, that other plugins can request slices of memory from, and it will hand out the next slice, as long as there is enough room left. As long as plugins request the same amount, in the same order, the contents of `EEPROM` will remain valid.

 [plugin:eeprom-settings]: https://github.com/keyboardio/Kaleidoscope-EEPROM-Settings

> If we add new plugins to our sketch, ones that use `EEPROM`, we should request their slice after all the others we already have.

The most simple sketch that uses the [EEPROM-Settings][plugin:eeprom-settings] plugin is shown below. It is recommended to start from there.

```c++
void setup () {
  Kaleidoscope.setup ();
  USE_PLUGINS (&EEPROMSettings);
  
  EEPROMSettings.seal ();
}
```

This does not do a whole lot, just initializes the framework, and makes sure that the small header the plugin needs to store the checksum - among other things - is safe and sound. When adding new plugins, requesting slices of storage, add those before the call to `EEPROMSettings.seal()`! Sealing the plugin means that no new slices will be given out, and we can verify the integrity of the layout.

To have a better understanding of how things work under the hood, lets imagine a scenario, where we want to have a few settings on the keyboard, which we want to persist. One such thing would be the speed of mouse keys, and the speed of acceleration. We'd store two settings: the delay between cursor movements, and the amount of acceleration. The first is a 16-bit value, the second an 8-bit one. Lets store them in a struct:

```c++
struct {
  struct {
    uint16_t movementDelay;
    uint8_t accelSpeed;
  } mouse;
} KeyboardSettings;
```

We will want to store this in EEPROM, be able to read it at keyboard startup, and update them from macros. Lets start with the easy parts: requesting a slice, and reading the settings at boot:

```c++
static uint16_t settingsBase;

void setup () {
  Kaleidoscope.setup ();
  USE_PLUGINS (&EEPROMSettings, &MouseKeys);
  
  settingsBase = EEPROMSettings.requestSlice (sizeof (KeyboardSettings));
  
  EEPROMSettings.seal ();
  
  EEPROM.get (settingsBase, KeyboardSettings);
  
  if (KeyboardSettings.mouse.movementDelay == 0xffff) {
    KeyboardSettings.mouse.movementDelay = 5;
  }
  if (KeyboardSettings.mouse.accelSpeed = 0xff) {
    KeyboardSettings.mouse.accelSpeed = 1;
  }
  
  MouseKeys.speedDelay = KeyboardSettings.mouse.movementDelay;
  MouseKeys.accelSpeed = KeyboardSettings.mouse.accelSpeed;
}
```

Now, there is one interesting part of this snippet which we have not talked about yet: why do we check the values of `KeyboardSettings.mouse.movementDelay` and `KeyboardSettings.mouse.accelSpeed`? And what are those magic numbers? You see, the EEPROM, when not initialized yet, is filled with values of `0xff`. Since we want to prepare our sketch to work with an uninitialized `EEPROM` too, we do these checks. Once the initialization has been done, and the values written back, we can safely remove those lines. Once we made sure that the values are correct, we apply those settings to the `MouseKeys` plugin too.

This pretty much covers how data is laid out in `EEPROM`: we start with a small header, then we - or plugins we use - can request slices, and they will be allocated in a continuous manner.

> **Being out of sync**: If we request a slice before our `KeyboardSettings` request, then the layout our firmware works with will be different than we have stored in `EEPROM`. This is what we call being out of sync. To avoid this, we either need to add further slice requests after the ones we already have, or we must move the contents of `EEPROM`, one way or the other.

> **Note**: The layout of the EEPROM is not stored in the EEPROM, there is no table of contents, anywhere. The firmware is responsible for handling the layout itself, and this must be done with care. If we change the EEPROM layout, plugins will suddenly find themselves working with data that makes no sense.

In the next section, we will look at how storage and retrieval works, in a little more detail.
