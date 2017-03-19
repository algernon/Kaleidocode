<!-- -*- mode: markdown; fill-column: 8192 -*- -->

## The layout of the EEPROM

Deep down the `EEPROM` is just a sequence of bytes. However, we have a number of semi-independent plugins, which all want to store some data in there. Therefore, we need a way to coordinate this, preferably in a way that makes it possible to detect when the firmware and the `EEPROM` are out of sync. This is where the [EEPROM-Settings][plugin:eeprom-settings] plugin comes in. This plugin is the coordinator, that other plugins can request slices of memory from, and it will hand out the next slice, as long as there is enough room left. As long as plugins request the same amount, in the same order, the contents of `EEPROM` will remain valid.

 [plugin:eeprom-settings]: https://github.com/keyboardio/Kaleidoscope-EEPROM-Settings

> If we add new plugins to our sketch, ones that use `EEPROM`, we should request their slice after all the others we already have.

The most simple sketch that uses the [EEPROM-Settings][plugin:eeprom-settings] plugin is shown below. It is recommended to start from there.

```c++
#include "Kaleidscope.h"
#include "Kaleidoscope-EEPROM-Settings.h"

void setup () {
  Kaleidoscope.setup ();
  USE_PLUGINS (&EEPROMSettings);
  
  EEPROMSettings.seal ();
}
```

This does not do a whole lot, just initializes the framework, and makes sure that the small header the plugin needs to store the checksum - among other things - is safe and sound. When adding new plugins, requesting slices of storage, add those before the call to `EEPROMSettings.seal()`! Sealing the plugin means that no new slices will be given out, and we can verify the integrity of the layout.
