# Bigger Stacks – Forge 1.12.2 port (alpha)

This repository is a clean-room Forge 1.12.2 implementation of the core behavior needed for very large `ItemStack` counts, based on public behavior and source-level observations of modern Bigger Stacks and on established 1.12.2 techniques used by StackUp.

## Current implementation

- Forge 1.12.2 / ForgeGradle 2.3 project.
- Java 8 source/target level.
- Global maximum configurable up to `999999999`.
- Per-item rules by registry id, metadata and `*` wildcard.
- Last matching rule wins.
- Large count persistence with a `BigCount` integer while preserving a byte-sized `Count` fallback.
- Vanilla `PacketBuffer` ItemStack count changed from byte serialization to int serialization.
- Forge `ByteBufUtils` ItemStack count changed the same way where its ItemStack serializer uses a byte count.
- Common inventory/slot limits are scaled so a 64-slot limit follows the configured global maximum.
- Vanilla creative-stack limit guard is replaced with the configured maximum.
- Vanilla item-drop chunk constants are scaled for large stacks.
- Large entity-item merge fallback is expanded.
- Server sends the effective stack-size configuration to clients after login.
- `/biggerstacks reload` and `/biggerstacks max` commands.
- Client tooltip fallback displays the exact stack count when it is greater than 64.

## Important status

This is an **alpha source port**, not a claimed feature-for-feature replacement of every modern Bigger Stacks compatibility module.

The development container used for this build does not have a ForgeGradle/Minecraft 1.12.2 development workspace and cannot reach Maven/GitHub from its build process. The Java sources were syntax-checked against a local API stub set, but the actual Forge/Minecraft runtime transformation has **not** been executed here.

That distinction matters: a coremod that compiles can still fail at class transformation time if a target method descriptor differs from the expected 1.12.2 bytecode. The first real test should therefore be a dedicated 1.12.2 client and server instance with logs enabled.

## Configuration

After first launch:

`config/biggerstacks12.cfg`

The global limit defaults to 4096.

Per-item rules:

`config/biggerstacks12/rules.cfg`

Examples:

```text
minecraft:stone=1024
minecraft:diamond=4096
minecraft:stone@1=2048
minecraft:* = 4096
```

`@metadata` applies to 1.12.2 item damage/metadata. `*` is a simple wildcard. The last matching rule wins.

Rules never exceed the global `maxStackSize` setting.

## Build

Use a JDK 8 environment. ForgeGradle 2.3 and Minecraft 1.12.2 are legacy tooling and are not a good target for modern JDK-only environments.

Typical workflow:

```text
setupDecompWorkspace

build
```

The built jar should appear under `build/libs/`.

Because this project uses the Forge 1.12.2 coremod mechanism, no MixinBooter dependency is required by the port itself.

## Recommended validation sequence

1. Start a clean 1.12.2 client with only Forge + this mod.
2. Start a clean 1.12.2 dedicated server with the same jar.
3. Set `maxStackSize=4096`.
4. Test a 5000-count stack created through commands or NBT.
5. Move the stack between player inventory and chest.
6. Log out and back in.
7. Drop a large stack and verify item entities merge as expected.
8. Test hoppers and other vanilla inventory automation.
9. Test creative inventory actions.
10. Inspect both client and server logs for transformer errors.

## Known limitations / next pass

- Modern Bigger Stacks has additional compatibility rules for individual third-party mods; this alpha does not reproduce those optional modules.
- The 1.12.2 GUI renderer is not replaced wholesale. Vanilla's count renderer remains responsible for drawing the number, while the tooltip guarantees an exact readable count.
- The generic inventory transformer intentionally matches common 1.12.2 method names; unusual coremods or non-standard bytecode may need another compatibility layer.
- The network change deliberately requires the mod on both sides because the ItemStack wire format is changed from a byte count to a four-byte integer.
