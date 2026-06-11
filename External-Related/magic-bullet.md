# Magic Bullet (Fake / Tool Grip)

> Pretty much the fake version of magic bullet.

```cpp
static uintptr_t GetLocalTool()
{
    try
    {
        SDK::Instance charObj = SDK::Cache::LocalPlayer.CharacterObject;
        if (!charObj.Address) return 0;
        return charObj.FindFirstChildOfClass(xorstr_("Tool")).Address;
    }
    catch (...) { return 0; }
}

static SDK::Vector3 GetLockedTargetPos()
{
    uintptr_t addr = Globals::LockedTargetAddress.load();
    if (!addr) return {};
    std::shared_lock<std::shared_mutex> lk(SDK::Cache::PlayersMutex);
    for (const auto& p : SDK::Cache::Players)
        if (p.PlayerObjectAddress == addr)
            return p.HumanoidRootPart.WorldPosition;
    return {};
}

static uintptr_t GetToolHandlePrimitive()
{
    try
    {
        uintptr_t toolAddr = GetLocalTool();
        if (!toolAddr) return 0;
        SDK::Instance tool; tool.Address = toolAddr;
        SDK::Instance handle = tool.FindFirstChild(xorstr_("Handle"));
        if (!handle.Address) return 0;
        return handle.Primitive().Address;
    }
    catch (...) { return 0; }
}

static void ToolGripThread()
{
    using namespace std::chrono_literals;

    for (;;)
    {
        std::this_thread::sleep_for(1ms);

        if (!Globals::Rage::ToolGrip::Enabled)
        {
            std::this_thread::sleep_for(50ms);
            continue;
        }

        uintptr_t tool = GetLocalTool();
        if (!tool) continue;

        float px = 0.f, py = 0.f, pz = 0.f;

        if (Globals::Rage::ToolGrip::TeleportMode)
        {
            SDK::Vector3 targetPos = GetLockedTargetPos();
            if (targetPos.x != 0.f || targetPos.y != 0.f || targetPos.z != 0.f)
            {
                SDK::Vector3 myPos = SDK::Cache::LocalPlayer.HumanoidRootPart.WorldPosition;
                px = targetPos.x - myPos.x;
                py = targetPos.y - myPos.y;
                pz = targetPos.z - myPos.z;
            }
        }
        else
        {
            px = Globals::Rage::ToolGrip::OffsetX;
            py = Globals::Rage::ToolGrip::OffsetY;
            pz = Globals::Rage::ToolGrip::OffsetZ;
        }

        try
        {
            SDK::Memory->Write<float>(tool + 0x4bc + 0x0, px);
            SDK::Memory->Write<float>(tool + 0x4bc + 0x4, py);
            SDK::Memory->Write<float>(tool + 0x4bc + 0x8, pz);
        }
        catch (...) {}
    }
}
```

> Credits: <@1320879136495108179> (not my code)
