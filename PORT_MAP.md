# Modern Bigger Stacks -> Forge 1.12.2 mapping

| Modern Bigger Stacks area | Forge 1.12.2 port implementation |
|---|---|
| `ItemStackMixin` max size | `BiggerStacksTransformer` patches `ItemStack#getMaxStackSize` |
| `ItemStackMixin` NBT | `ItemStack#writeToNBT` + `ItemStack(NBTTagCompound)`/static NBT loader hooks |
| `FriendlyByteBufMixin` | `PacketBuffer` ItemStack serializer; `ByteBufUtils` serializer where applicable |
| `ItemMixin` | `Item#getMaxStackSize` and `Item#getItemStackLimit` |
| `ContainersMixin` | common inventory/slot limits + `InventoryHelper` drop chunk scaling |
| `ItemEntityMixin` | `EntityItem#combineItems` 64 fallback |
| creative packet handling | `NetHandlerPlayServer` creative 64 guard |
| modern renderer mixin | vanilla 1.12 renderer retained + exact-count tooltip fallback |
| optional AE2/Mekanism/RS integrations | not included in alpha; separate compatibility modules can be added later |
