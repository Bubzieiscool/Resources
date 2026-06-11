# Third Person

```cpp
void ThirdPerson()
{
    SDK::Vector3 SmoothedPos(0, 0, 0);
    bool FirstFrame = true;

    while (true)
    {
        Globals::World::Movement::ThirdPersonKey.update();
        if (Globals::World::Movement::ThirdPerson && Globals::World::Movement::ThirdPersonKey.enabled)
        {
            SDK::Camera Camera = (SDK::Camera)Globals::Workspace.FindFirstChildOfClass("Camera");
            if (!Camera.Address || !SDK::Cache::LocalPlayer.Head.Object.Address)
            {
                std::this_thread::sleep_for(std::chrono::milliseconds(16));
                continue;
            }
            SDK::Memory->Write<int>(Camera.Address + SDK::Offsets::CameraType, 6);
            SDK::Matrix3 CamRot = Camera.CameraRotation();
            SDK::Vector3 LookVec = LookVector(CamRot);
            float dist = Globals::World::Movement::ThirdPersonDistance;
            SDK::Vector3 Offset = LookVec * dist;
            SDK::Vector3 Target = SDK::Cache::LocalPlayer.Head.WorldPosition + Offset + SDK::Vector3(0, 1.25f, 0);

            if (FirstFrame)
            {
                SmoothedPos = Target;
                FirstFrame = false;
            }

            SmoothedPos = SmoothedPos + (Target - SmoothedPos) * 0.15f;
            Camera.SetCameraPosition(SmoothedPos);
            std::this_thread::sleep_for(std::chrono::milliseconds(5));
        }
        else
        {
            FirstFrame = true;
            std::this_thread::sleep_for(std::chrono::milliseconds(16));
        }
    }
}
```
