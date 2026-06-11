# Infinite Jump

```cpp
void InfiniteJump()
{
    while (true)
    {
        bool active = false;
        SDK::Humanoid Humanoid = SDK::Cache::LocalPlayer.Humanoid;

        if (Humanoid.Address && Globals::World::Movement::InfiniteJumpNameSpace::CustomJumpPower)
        {
            Humanoid.SetUseJumpPower(true);
            Humanoid.SetJumpPower(Globals::World::Movement::InfiniteJumpNameSpace::CustomJumpPowerValue);
            active = true;
        }

        if (Globals::World::Movement::InfiniteJump)
        {
            active = true;
            if (GetAsyncKeyState(VK_SPACE) & 0x8000) {
                SDK::Cache::Bone HRP = SDK::Cache::LocalPlayer.HumanoidRootPart;
                if (HRP.Object.Address && Humanoid.Address)
                {
                    float YVelocity = Humanoid.JumpPower();
                    if (Globals::World::Movement::InfiniteJumpNameSpace::CustomJumpPower)
                    {
                        YVelocity = Globals::World::Movement::InfiniteJumpNameSpace::CustomJumpPowerValue;
                    }
                    SDK::Vector3 Velocity = HRP.Object.Primitive().Velocity();
                    SDK::Vector3 NewVelocity(Velocity.x, YVelocity, Velocity.z);
                    HRP.Object.Primitive().SetVelocity(NewVelocity);
                }
            }
        }

        if (active)
            std::this_thread::sleep_for(std::chrono::milliseconds(5));
        else
            std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
}
```
