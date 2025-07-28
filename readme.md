# ESP32-P4 MIPI DSI LCD Demo

This project demonstrates how to initialize and drive a **1280×720 MIPI DSI LCD panel** using an **ESP32-P4** development kit with **PSRAM enabled** and **MIPI DPI + DSI interface**.

It includes:
- Powering the MIPI DSI PHY via LDO
- Setting up the DSI bus and DBI interface
- Initializing the LCD panel
- Drawing a software-generated color bar
- Displaying a hardware color pattern

---

## 🧰 Requirements

- **ESP32-P4-Module-DEV-KIT**
- **Waveshare MIPI DSI 7" LCD (or similar)**
- ESP-IDF **v5.2 or later**
- LCD resolution: `1280x720`
- External **PSRAM** enabled
- Sufficient **I2C and GPIO** wiring if required for initialization
- LCD backlight control (optional)

---

## 🛠️ Configuration

Ensure the following are enabled in `menuconfig`:

Component config →
ESP System Settings →
[*] Support for external, SPI-connected RAM


Also verify:
- PSRAM is large enough for framebuffer(s)
- Pixel clock isn't too high for PSRAM (≤ 40 MHz recommended)

---

## ⚙️ Pin Definitions

| Signal       | GPIO   |
|--------------|--------|
| LCD RESET    | -1 (disabled) or GPIO27 |
| Backlight    | -1 (disabled) |
| I2C SDA/SCL  | GPIO7 / GPIO8 (by default) |

Adjust as needed in your project configuration.

---

## 📁 Project Structure

- `test_init_lcd()`  
  Initializes LDO, MIPI DSI bus, DBI IO, and the LCD panel.

- `test_draw_color_bar()`  
  Draws a vertical color bar using software rendering.

- `app_main()`  
  Entry point: initializes LCD, shows hardware and software color bars, then deinitializes.

---

## 💡 Notes

- The framebuffer is allocated in **PSRAM** using `heap_caps_calloc()` with `MALLOC_CAP_DMA`.
- A **refresh semaphore** ensures synchronization between software rendering and DPI refresh.
- **Double buffering** is supported but not explicitly enabled in this example.

---

## 🖼️ Display Output

The program:
1. Initializes the panel and turns it on
2. Shows a hardware-drawn horizontal color bar pattern
3. Then shows a custom-drawn software color bar using DMA
4. Waits a few seconds before deinitializing

---

## 🚫 Known Issues

- If pixel clock (`pclk_hz`) is too high, **DMA underruns** will occur.
- Ensure `.psram_trans_align` and buffer alignment are correct.
- DSI configuration must match the panel datasheet (number of lanes, timings, etc.).

---

## ✅ Example Output (Log)

I (324) DEBUG +++++++++ log : : New panel
I (324) Waveshare DSI: version: 1.0.6
I (334) i2c_bus: I2C Bus V2 uses the externally initialized bus handle
I (345) MIPI DSI PHY Powered on
I (351) Install panel IO
I (359) Install LCD driver of dsi - OK
I (365) Show color bar pattern drawn by hardware
I (378) Show color bar drawn by software
I (398) End of test



---

## 🧪 Optional Features to Try

- Pixel-by-pixel test (uncomment in `app_main`)
- Double framebuffer (`.num_frame_buffers = 2`)
- Use `esp_cache_msync()` when drawing from PSRAM directly

---

## 📜 License

MIT — use this freely for personal or commercial projects.

---

## 🤝 Contributing

Feel free to fork, modify, and submit PRs if you improve performance, compatibility, or add new panel support.

---

## 🧾 Credits

- Based on ESP-IDF MIPI DSI and LCD component APIs
- Tested on ESP32-P4 with Waveshare 7" DSI display

---