# 🍓 Raspberry Pi Pico W 開發板介紹

## 💻 核心硬體規格
- **微控制器**：RP2040（雙核心 ARM Cortex-M0+ 處理器，運作頻率高達 133 MHz）
- **靜態隨機存取記憶體 (SRAM)**：264 KB 晶片內建 SRAM
- **快閃記憶體 (Flash)**：2 MB 外部 QSPI Flash
- **無線晶片**：Infineon CYW43439（支援 2.4 GHz 802.11 b/g/n Wi-Fi 與藍牙 5.2）
- **工作電壓**：3.3V（I/O 邏輯準位），5V（透過 USB 供電）
- **引腳介面**：
  - 26 個多功能 GPIO 引腳
  - 3 個類比輸入引腳（12-bit ADC）
  - 16 個 PWM 通道

## 📡 支援的通訊方式
- **無線網路**：Wi-Fi 4 (802.11 b/g/n)
- **藍牙**：Bluetooth 5.2 / BLE（透過 CYW43439 晶片支援）
- **串列通訊**：2 × UART
- **總線介面**：2 × I2C、2 × SPI

## 🌟 架構的先進特色
- **雙核心靈活性**：具備強大的雙核心 Cortex-M0+ 架構，可透過雙核分工處理即時控制與通訊任務。
- **內建無線連線**：相較於標準版 Pico，Pico W 原生整合 Wi-Fi 與藍牙無線模組，極適合 IoT 物聯網與智慧家庭應用。
- **可程式化 I/O (PIO)**：擁有 8 個 PIO 狀態機器，可自訂硬體周邊介面（如 WS2812 燈條、VGA 等），擴充彈性極高。

## 📏 外型尺寸規格
- **尺寸大小**：21 mm × 51 mm
- **連接埠**：Micro-USB 2.0（用於供電與韌體燒錄）
- **封裝形式**：DIP 雙排引腳與邊緣半孔焊盤（Castellated Pads）

## 🗺️ 文字化接腳與結構示意圖

```text
       +-----------------------------------+
       |           [ Micro-USB ]           |
       +-----------------+-----------------+
                         |
      +------------------+------------------+
      |  ( ) GP0 (TX0)       GP21 ( )      |
      |  ( ) GP1 (RX0)       GND  ( )      |
      |  ( ) GND             GP22 ( )      |
      |  ( ) GP2 (SDA0)      GP25 ( )      |
      |  ( ) GP3 (SCL0)      3V3  ( )      |
      |  [ BOOTSEL Button ]  VSYS ( )      |
      +-------------------------------------+
                 Raspberry Pi Pico W
```

## ⚙️ 在 VS Code + PlatformIO 開發環境中需要怎麼設定？

### 專案設定檔 (`platformio.ini`)
```ini
[env:picoW]
platform = raspberrypi
board = pico
framework = arduino
monitor_speed = 115200
```

### 測試範例程式碼 (`src/main.cpp`)
```cpp
#include <Arduino.h>

void setup() {
  // 初始化序列埠通訊
  Serial.begin(115200);
  // 初始化內建 LED 腳位為輸出模式
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  // 輸出訊息至序列埠
  Serial.println("Raspberry Pi Pico W Running...");
  // 點亮 LED 燈
  digitalWrite(LED_BUILTIN, HIGH);
  delay(1000);
  // 熄滅 LED 燈
  digitalWrite(LED_BUILTIN, LOW);
  delay(1000);
}
```