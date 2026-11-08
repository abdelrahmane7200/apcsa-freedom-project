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

### 11/07/2026:
* Issue: Creating a consistent way of making maps.
* What was done:
1) Installed and played around with [LDtk](https://ldtk.io/)
2) Used a [JSON Viewer](https://jsonformatter.org/json-viewer) to understand the output of the code 
3) Create a custom Parser class to interpert the JSON:
```cpp
    class LDtkParser {
public:
  void loadLevel(const std::string& filepath, const std::string& level_name);

  std::vector<Rect> loadSolids();

  SDL_Point getSpawnPoint();

  std::vector<RenderTile> loadTiles(const std::string& layer_name);
```
4) Go through the JSON
```cpp
  for (auto& layer : m_level["layerInstances"]) {
    if (layer["__identifier"] == name) {
      returned_layer = layer;
      found = true;
      break;
    }
  }
```
5) Do what I need with the information within it, for example, getting all the tiles to be VISUALLY rendered:
```cpp
  std::vector<RenderTile> tiles; // What will be returned
  for (auto& tile : tile_list) {
    int px_x = tile["px"][0]; // Where in the level
    int px_y = tile["px"][1];
    int src_x = tile["src"][0];
    int src_y = tile["src"][1];
    int flip = tile["f"];

    RenderTile t;
    t.x = px_x;
    t.y = px_y;
    t.src_rect = {src_x, src_y, cell_size, cell_size}; // x, y, width, height
    t.flip = flip;
    tiles.push_back(t);
  }
  return tiles;
```

<!-- 
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->
