# Set FPS Cap

```cpp
if (is_enabled)
{
    if (!value)
    {
        delay = memory->read<double>(ptr + Offsets::TaskScheduler::MaxFPS);
        value = true;
    }
    memory->write<double>(ptr + Offsets::TaskScheduler::MaxFPS, 0.0);
    used = true;
}
```
