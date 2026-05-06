# Swift build support

GNUstep Make can build Swift sources when the active environment provides a
GNUstep Swift interop profile.  This support is experimental and is intended to
track the GNUstep Objective-C interop work while that compiler interface is
being upstreamed.

Swift sources are listed with the same per-instance pattern used for other
languages:

```make
include $(GNUSTEP_MAKEFILES)/common.make

TOOL_NAME = MyTool
MyTool_SWIFT_FILES = main.swift Support.swift

include $(GNUSTEP_MAKEFILES)/tool.make
```

Applications use the same variable:

```make
include $(GNUSTEP_MAKEFILES)/common.make

APP_NAME = MyApp
MyApp_SWIFT_FILES = Main.swift

include $(GNUSTEP_MAKEFILES)/application.make
```

The instance may override the Swift module name with
`<instance>_SWIFT_MODULE_NAME`.  The default module name is the instance name
with `-` and `.` replaced by `_` so common GNUstep target names are valid Swift
module identifiers.

Objective-C files in the same instance can include a generated Swift-to-ObjC
header by setting:

```make
MyTool_GENERATE_SWIFT_OBJC_HEADER = yes
```

The generated header is emitted into the object directory as
`<module>-Swift.h`, and that directory is added to the Objective-C include
path for the instance.

The Swift interop profile must provide these variables:

* `GNUSTEP_SWIFT_SWIFT_FRONTEND`
* `GNUSTEP_SWIFT_REAL_SWIFTC`
* `GNUSTEP_SWIFT_RESOURCE_DIR`
* `GNUSTEP_SWIFT_GNUSTEP_HEADERS`
* `GNUSTEP_SWIFT_GNUSTEP_MODULEMAP`
* `GNUSTEP_SWIFT_LIBOBJC2_SRC`
* `GNUSTEP_SWIFT_LIBOBJC2_CONFIG_INCLUDE`
* `GNUSTEP_SWIFT_LIBOBJC2_LIBDIR`

`GNUSTEP_SWIFT_AUTOLINK_EXTRACT` may be set to override the default
`swift-autolink-extract` path derived from `GNUSTEP_SWIFT_REAL_SWIFTC`.

The main overridable Swift rule variables are:

* `SWIFT`
* `SWIFT_FRONTEND`
* `SWIFT_RESOURCE_DIR`
* `SWIFT_AUTOLINK_EXTRACT`
* `SWIFT_MODULE_CACHE_DIR`
* `SWIFT_OBJC_RUNTIME`
* `SWIFT_RUNTIME_PLATFORM`
* `SWIFT_RUNTIME_LIB_DIR`
* `SWIFT_RUNTIME_ARCH_LIB_DIR`
* `SWIFT_BASE_LIBS`
* `SWIFT_GUI_LIBS`

`gnustep-config --swift-flags` prints the active Swift compiler flags.
`gnustep-config --swift-libs` prints the Swift library and runtime path flags.
