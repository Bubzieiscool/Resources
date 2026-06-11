# Noclip

```cpp
while (true)
{
    try
    {
        if (!game::datamodel.address)
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            continue;
        }

        cache::entity_t local;
        {
            std::lock_guard<std::mutex> lock(cache::mtx);
            local = cache::cached_local_player;
        }

        if (!local.instance.address)
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(10));
            continue;
        }

        if (settings::rage::noclip)
        {
            for (const auto& part_pair : local.parts)
            {
                if (!part_pair.second.address)
                    continue;

                rbx::part_t part = part_pair.second;
                std::uint64_t primitive_address = memory->read<std::uint64_t>(part.address + Offsets::BasePart::Primitive);
                if (!primitive_address)
                    continue;

                try
                {
                    std::uint8_t primitive_flags = memory->read<std::uint8_t>(primitive_address + Offsets::Primitive::Flags);
                    primitive_flags &= ~0x08;
                    memory->write<std::uint8_t>(primitive_address + Offsets::Primitive::Flags, primitive_flags);
                }
                catch (const std::exception& e)
                {
                    (void)e;
                    continue;
                }
            }
        }

        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
    catch (const std::exception& e)
    {
        (void)e;
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}
```
