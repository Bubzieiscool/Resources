# Teleport

```cpp
bool cache::teleport(model target)
{
    auto character = mem->simple_read<model>(globals::g_local_player.address + offsets::Player::Character);
    if (!utilities::is_valid_address(character.address))
        return false;

    auto hrp = character.find_first_child_of_value<base_part>("HumanoidRootPart");
    if (!utilities::is_valid_address(hrp.address))
        return false;

    auto target_hrp = target.find_first_child_of_value<base_part>("HumanoidRootPart");
    if (!utilities::is_valid_address(target_hrp.address))
        return false;

    auto prim = hrp.get_primitive();
    if (!utilities::is_valid_address(prim.address))
        return false;

    auto target_prim = target_hrp.get_primitive();
    if (!utilities::is_valid_address(target_prim.address))
        return false;

    prim.set_property(math::vector3(0, 0, 0), offsets::Primitive::AssemblyLinearVelocity);
    prim.set_property(target_prim.get_data().position, offsets::Primitive::Position);
    prim.set_property(target_prim.get_data().rotation, offsets::Primitive::Rotation);

    return true;
}
```

> Credits: 32xdd_2 / 1369295091704795151
