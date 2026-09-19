# Port status – 2026-09-19

## Completed in source

- Forge 1.12.2 coremod bootstrap.
- Global + per-item stack-size rule engine.
- NBT `BigCount` persistence.
- PacketBuffer large-count serialization using an int.
- Forge ByteBufUtils large-count serialization using an int where applicable.
- Slot/inventory limit scaling.
- Creative guard expansion.
- Item-drop chunk scaling.
- Entity-item merge fallback expansion.
- Server/client rule synchronization.
- Basic client tooltip support.
- `/biggerstacks` command.

## Not verified in a real Minecraft 1.12.2 runtime yet

- Exact production-obfuscated method descriptors on every target class.
- A full Forge client startup with this coremod.
- A dedicated server connection and large-stack round trip.
- Interactions with third-party mods that ship custom inventory/network serializers.

## Validation performed here

All Java sources were compiled with `javac -source 8 -target 8` against a local stub set representing the external Minecraft/Forge/ASM APIs. This verifies Java syntax and the project's internal signatures, but it is **not equivalent to a Forge runtime test**.

Compiler result: no Java compilation errors against the stubs (only expected JDK 21 warnings about compiling legacy Java 8 source/target).


## Test client result — 2026-09-19
The first single-JAR test reached Forge and loaded `BiggerStacks12Core`, but crashed during mod identification because `BiggerStacksCore#getModContainerClass()` incorrectly returned the normal `@Mod` class. Forge attempted to cast `BiggerStacksMod` to `ModContainer`, producing a `ClassCastException`. Fixed by returning `null` from `getModContainerClass()` and adding `@IFMLLoadingPlugin.MCVersion("1.12.2")`. The fixed test JAR is `BiggerStacks-1.12.2-Port-0.1.0-alpha-test-fixed.jar`.


### v2 test build
The `PlayerLoggedInEvent` reference was corrected for Forge 1.12.2 (`fml.common.gameevent.PlayerEvent`).
