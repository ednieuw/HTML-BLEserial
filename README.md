# HTML-BLEserial

[Webpage terminal / serial monitor to connect to a BLE UART device.](https://ednieuw.home.xs4all.nl/BLESerial/BLE_UART_Terminal.html)

To control microprocessors via a serial port or Bluetooth a serial terminal is used — a program that sends and receives ASCII characters.<br> 
When a BLE device uses the TI CC2541 (HM-10, HM-16, JDY modules) or a Nordic nRF52 chipset (all ESP32s), [an app on a phone](https://ednieuw.home.xs4all.nl/BLESerial/IOSappMain.html) can be installed and used. <br>
Since 2026, browsers like Chrome and Edge can simulate a serial terminal via BLE directly, without installing any app.

<img width="900" alt="image" src="https://github.com/user-attachments/assets/76d0f0b0-9bed-4342-84b6-54fc1d50e94d" />

[Open Serial monitor](https://ednieuw.home.xs4all.nl/BLESerial/BLE_UART_Terminal.html)

---

## Requirements

- **Browser:** Chrome or Edge (desktop or Android). Safari and Firefox do not support Web Bluetooth.
- **OS:** Windows 10/11, macOS, Android. iOS is not supported by Chrome.
- **Device:** Any BLE UART device using the Nordic UART Service (NUS) UUIDs or TI CC2541-compatible UUIDs.

---

## Browser flags

If no devices appear or the connection fails, check that Web Bluetooth is enabled:

- In Chrome, type `chrome://flags` in the address bar
- In Edge, type `edge://flags`
- Search for `bluetooth`
- Make sure nothing is disabled. On some versions `#enable-web-bluetooth` needs to be explicitly enabled.
- Also enable: **Use the new permissions backend for Web Bluetooth** and **Web Bluetooth confirm pairing support**

---

## Usage

1. Open `BLE_UART_Terminal.html` from [ednieuw.home.xs4all.nl](https://ednieuw.home.xs4all.nl/BLESerial/BLE_UART_Terminal.html) in Chrome or Edge
2. Click **⬡ CONNECT** — a browser dialog appears listing nearby BLE devices
3. Select your device and click **Pair** (despite the label, no PIN or bond is required)
4. The status indicator turns green and the device name appears in the header
5. Type a command in the input field and press **Enter** or **SEND**
6. Received data appears in green (◀), sent data in amber (▶)
7. Click **✕ DISCONNECT** to disconnect

---

## Options

| Option | Description |
|---|---|
| TIMESTAMP | Prefix each line with HH:MM:SS.mmm (off by default) |
| AUTOSCROLL | Automatically scroll to the latest received line |
| CR+LF / LF / CR / NONE | Line ending appended to each sent command |
| ASCII / HEX / BOTH | Display format for received data |
| Light/Dark theme switch | Choose between light or dark theme |
---

## Device compatibility

| Chipset | Modules | Notes |
|---|---|---|
| Nordic nRF52 | ESP32, Arduino Nano ESP32 | Full support, all message lengths |
| TI CC2541 | HM-10, HM-16, JDY-08 | Supported — sends one character at a time due to small MTU |
| TI CC2511 | Various | Compatible |

### UUIDs used

This terminal uses the following service and characteristic UUIDs:

```
Service:  6E400001-B5A3-F393-E0A9-E50E24DCCA9E
TX (write): 6E400002-B5A3-F393-E0A9-E50E24DCCA9E
RX (notify): 6E400003-B5A3-F393-E0A9-E50E24DCCA9E
```

> **Note:** These differ slightly from the standard Nordic NUS UUIDs (`B5BA`). If your firmware uses standard NUS UUIDs, edit the three UUID constants at the top of the script.

---

## Windows notes

- The browser picker shows all nearby BLE devices. Devices your firmware does not advertise the UART service for will appear greyed out and cannot be selected.
- On Windows 11, the first GATT connection attempt sometimes fails. The terminal automatically retries up to 3 times with a 1-second delay between attempts.
- Previously paired devices show **"- Paired"** in the picker. This is a Chrome/Windows label and cannot be removed from the webpage.

---

## Quick-send buttons

The row of preset buttons sends a single command with one click. To customize them, edit the `data-cmd` attribute and label text of each `.preset-btn` button in the HTML source.

---

## Troubleshooting

**No devices appear in the picker**
Your device may not be advertising the correct service UUID. Try moving closer to the device and ensure it is in advertising mode.

**Connect failed: GATT Server is disconnected**
Common on Windows with older BLE chipsets. The terminal retries automatically. If it keeps failing, remove the device from Windows Bluetooth settings and let the browser re-pair it.

<img width="896" alt="BLE UART Terminal screenshot" src="https://github.com/user-attachments/assets/48aa3ccb-8bb8-40f5-8cb4-dfa19b6b3f6f" />
**Device only responds to the first character of a command**
Check the line ending setting in the toolbar. Many devices require a specific line ending (**CR+LF**, **LF**, or **CR**) to recognise a complete command. If the wrong ending is selected the device may only process the first character. Try switching the dropdown until the device responds correctly.

**Works on Mac but not on Windows**
Check the browser flags above. Also try unpairing the device in Windows Bluetooth settings and reconnecting via the browser.
