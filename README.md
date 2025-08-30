# BNO085 RVC component for ESPHome

This is a custom [ESPHome](https://esphome.io/) component for the **BNO085 IMU (Inertial Measurement Unit)** in **RVC (Robotics Vehicle Control) mode** over UART.  
It reads **fused orientation (yaw, pitch, roll)** and **linear acceleration (X, Y, Z)** directly from the sensor, making it ideal for robotics, drones, gimbals, or any project where precise orientation and motion data are required.

---

## 📖 What is the BNO085?

The **BNO085** (by Hillcrest Laboratories / CEVA, often sold under Adafruit or SparkFun boards) is a high-performance 9-axis absolute orientation sensor.  
It fuses data from its accelerometer, gyroscope, and magnetometer using onboard algorithms to provide stable orientation and motion outputs.

---

## 🔧 What is RVC Mode?

RVC (**Robotics Vehicle Control**) is a special binary output protocol of the BNO085.  
Instead of using I²C or SPI with complex command sets, the sensor continuously streams compact orientation and acceleration data via **UART**.  

This has several advantages:
- Simple integration (just read UART packets).
- Low overhead compared to full SHTP protocol.
- Ideal for microcontrollers with limited resources.
- Provides yaw, pitch, roll, and linear acceleration directly.

---

## 🚀 Features

- Reads **Yaw, Pitch, Roll** in degrees (°).
- Reads **X, Y, Z Acceleration** in m/s².
- Configurable update interval (default: 50 ms).
- Exposes each value as an ESPHome `sensor`.
- Works with any ESP8266, ESP32, or similar board supported by ESPHome.

---

## 📦 Installation

1. Copy this repository into your ESPHome `custom_components` folder:

   ```
   esphome/
   └── custom_components/
       └── bno085_rvc/
           ├── __init__.py
           ├── sensor.py
           ├── bno085_rvc.cpp
           ├── bno085_rvc.h
   ```

2. Make sure the `uart` component is configured in your ESPHome YAML (the BNO085 requires 115200 baud in RVC mode).

3. Reference the component in your ESPHome configuration (see example below).

---

## 📝 Example Configuration

```yaml
esphome:
  name: imu_node

# Enable UART (adjust TX/RX pins to your board wiring)
uart:
  rx_pin: GPIO16
  baud_rate: 115200

# Include the custom BNO085 RVC component
sensor:
  - platform: bno085_rvc
    update_interval: 50ms
    yaw:
      name: "BNO085 Yaw"
    pitch:
      name: "BNO085 Pitch"
    roll:
      name: "BNO085 Roll"
    x_accel:
      name: "BNO085 Accel X"
    y_accel:
      name: "BNO085 Accel Y"
    z_accel:
      name: "BNO085 Accel Z"
```

---

## ⚠️ Notes

- Make sure the **BNO085 firmware** is set to **RVC mode**. (This usually requires flashing the RVC firmware variant once or setting a jumper/GPIO of the sensor).
- RVC mode is **UART only** – I²C and SPI are not used with this integration.
- The ESPHome node will warn in the logs if corrupted or incomplete packets are received.

---

## ✅ To-Do / Ideas

- Provide automatic detection of communication errors.
- Extend documentation with wiring examples for popular breakout boards.
- Improve incomplete data frames handling

---

## 📚 References

- [ESPHome Custom Components](https://esphome.io/custom/index.html)  
- [BNO085 Datasheet](https://cdn.sparkfun.com/assets/learn_tutorials/1/2/7/9/SH-2-Reference-Manual.pdf)  
- [Adafruit BNO085 Guide](https://learn.adafruit.com/adafruit-bno08x-absolute-orientation-sensor)  

---

## 🖊 License

GNU General Public License v3.0
