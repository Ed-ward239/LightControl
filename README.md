# LightControl


````markdown
This project automatically turns a **USB-controlled LED altar light** **ON or OFF** based on the time of day,  
creating a daily lighting ritual for a Buddhist home altar.

Built with **C++17** and **libusb**, it continuously monitors system time and controls the light via USB.

---

## 🌅 Features

- ⏰ Automatically turns lights **ON** during morning & evening hours (6–10 AM, 6–10 PM)
- 🌙 Turns lights **OFF** during daytime or late-night hours
- 🔌 Communicates directly with the USB device using **libusb-1.0**
- 🧘 Minimal and efficient — designed for quiet continuous operation
- 💡 Can run on **macOS**, **Linux**, or **Raspberry Pi**

---

## ⚙️ Schedule Logic

| Time Range | Action |
|-------------|---------|
| 06:00 – 10:00 | Light ON (Morning Offering) |
| 10:00 – 18:00 | Light OFF |
| 18:00 – 22:00 | Light ON (Evening Offering) |
| 22:00 – 06:00 | Light OFF (Night) |

The schedule is defined in the function:

```cpp
bool isDaytime(int hour)
{
    return ((hour >= 6 && hour < 10) || (hour >= 18 && hour < 22));
}
````

You can easily modify these hours to fit your own daily altar routine.

---

## 🧰 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Ed-ward239/LightControl.git
cd buddhist-home-altar-light
```

### 2. Install Dependencies

#### macOS (Homebrew)

```bash
brew install libusb
```

#### Ubuntu / Raspberry Pi OS

```bash
sudo apt update
sudo apt install build-essential libusb-1.0-0-dev
```

---

## 🧱 Build Instructions

Compile using `g++` with libusb linked:

```bash
g++ altar_light.cpp -lusb-1.0 -std=c++17 -o altar_light
```

---

## ▶️ Run the Program

```bash
sudo ./altar_light
```

> **Note:** USB access usually requires root privileges.
> You can create a udev rule (on Linux) to allow non-root execution.

The program will:

* Continuously check the current time once per minute
* Print logs such as:

  ```
  Turning lights ON: 2025-11-06 06:05:00
  Turning lights OFF: 2025-11-06 10:00:00
  ```
* Control the LED through your USB device.

---

## 🔌 Hardware Setup

* A **USB LED light** connected to your Raspberry Pi, Linux, or macOS device.
* The code expects a **Vendor ID (VID)** and **Product ID (PID)** of:

  ```cpp
  0x1234, 0x5678
  ```

  Replace these with your actual USB device’s IDs.
  You can find them with:

  ```bash
  lsusb
  ```

---

## 🧠 How It Works

1. Initializes USB communication via `libusb_init`
2. Locates the device using `libusb_open_device_with_vid_pid`
3. Sends a simple control byte:

   ```cpp
   unsigned char data[] = {turnOn ? 0x01 : 0x00};
   ```
4. Transfers the command using a **bulk transfer** to endpoint `0x01`
5. Repeats every 1 minute

---

## 🧘 Recommended Setup (Raspberry Pi)

1. Connect your LED via USB
2. Copy `altar_light` binary to `/home/pi/altar/`
3. Add it to system startup:

   ```bash
   sudo crontab -e
   ```

   Then add:

   ```
   @reboot /home/pi/altar/altar_light &
   ```
4. Your altar light will now automatically turn ON/OFF at the correct times every day 🌅🌙

---

## 🧾 Example Output

```
Turning lights ON: 2025-11-06 06:00:00
Turning lights OFF: 2025-11-06 10:00:00
Turning lights ON: 2025-11-06 18:00:00
Turning lights OFF: 2025-11-06 22:00:00
```

---

## 🛠️ Customization

| Setting               | Location                        | Description                               |
| --------------------- | ------------------------------- | ----------------------------------------- |
| On/Off hours          | `isDaytime()`                   | Adjust lighting schedule                  |
| USB Vendor/Product ID | `controlLed()`                  | Match your specific device                |
| Polling frequency     | `std::this_thread::sleep_for()` | Change check interval (default: 1 minute) |

---

## 🧑‍💻 Author

**Edward Lee**
📧 `edward.lee2309@gmail.com`
💻 [GitHub: Ed-ward239](https://github.com/Ed-ward239)

---

### ☸️ “May this light represent clarity, mindfulness, and compassion.”