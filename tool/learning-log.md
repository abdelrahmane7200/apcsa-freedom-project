# Tool Learning Log

## Tool: **C++ & SDL2**

## Project: **Platformer about time-manipulation**

---

### 10/24/2026:
* Issue: Getting input would work for checking if key is held but not if it is pressed
* Reason figured out: Because updating the keyboard state was not running in sync with our framerate:
```cpp
while (m_window.isOpen() && m_is_running) {
    m_window.handleWindowEvents();
    m_input.update();
...
}
```
* Fix: Move it so that it's at the top of our fixed timeste:
```cpp
while (m_window.isOpen() && m_is_running) {
    ...
    while (m_window.isOpen() && m_is_running) {
        m_window.handleWindowEvents();
        m_input.update();
    }
}
```

### X/X/XX:
* Text


<!-- 
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->
