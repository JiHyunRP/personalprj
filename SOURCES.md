# Online references used for the port

## Bigger Stacks

- CurseForge project: https://www.curseforge.com/minecraft/mc-mods/bigger-stacks
- GitHub mirror: https://github.com/MaiKokain/biggerstacks
- Modern mixin configuration: https://raw.githubusercontent.com/MaiKokain/biggerstacks/1.20.1/src/main/resources/biggerstacks.mixins.json
- Modern ItemStack count/NBT mixin: https://raw.githubusercontent.com/MaiKokain/biggerstacks/1.20.1/src/main/java/portb/biggerstacks/mixin/vanilla/stacksize/ItemStackMixin.java
- Modern network count mixin: https://raw.githubusercontent.com/MaiKokain/biggerstacks/1.20.1/src/main/java/portb/biggerstacks/mixin/vanilla/FriendlyByteBufMixin.java
- Modern item/entity/inventory patches: https://raw.githubusercontent.com/MaiKokain/biggerstacks/1.20.1/src/main/java/portb/biggerstacks/mixin/vanilla/stacksize/ItemMixin.java

## 1.12.2 reference implementation

- StackUp repository: https://github.com/asiekierka/StackUp
- StackUp CurseForge: https://www.curseforge.com/minecraft/mc-mods/stackup
- StackUp transformer source: https://github.com/asiekierka/StackUp/blob/master/src/main/java/pl/asie/stackup/core/StackUpTransformer.java

## Forge 1.12.2 API references

- ItemStack JavaDocs: https://nekoyue.github.io/ForgeJavaDocs-NG/javadoc/1.12.2/net/minecraft/item/ItemStack.html
- Item JavaDocs: https://nekoyue.github.io/ForgeJavaDocs-NG/javadoc/1.12.2/net/minecraft/item/Item.html
- Slot JavaDocs: https://nekoyue.github.io/ForgeJavaDocs-NG/javadoc/1.12.2/net/minecraft/inventory/Slot.html
- IItemHandler JavaDocs: https://nekoyue.github.io/ForgeJavaDocs-NG/javadoc/1.12.2/net/minecraftforge/items/IItemHandler.html

## Design rationale

Modern Bigger Stacks demonstrates that large stacks require more than a higher `getMaxStackSize`: the modern implementation changes NBT persistence, item-stack network serialization, container/drop behavior and merge behavior. StackUp independently demonstrates the same broad categories on Forge 1.12.2, including `PacketBuffer`, ItemStack limits, creative handling and inventory/slot limits.

This port therefore targets those same functional pressure points using the 1.12.2 coremod/ASM mechanism instead of trying to mechanically translate modern mixin classes.
