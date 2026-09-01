# 🚀 ESP32-S3-WROOM-1 開發板介紹

## 核心硬體規格
*   **處理器**：Xtensa® 雙核心 32 位元 LX7 微處理器，時脈高達 240 MHz。
*   **記憶體**：
    *   內部 SRAM：512 KB
    *   內部 ROM：384 KB
    *   整合 SPI Flash：支援最高 16 MB
    *   整合 PSRAM：支援最高 8 MB（依具體型號而定）
*   **工作電壓**：3.0 V ~ 3.6 V
*   **工作溫度**：-40 °C ~ 105 °C（高溫版本）

## 支援的通訊方式
*   **Wi-Fi**：802.11 b/g/n（2.4 GHz 頻段），支援高達 150 Mbps 的資料傳輸率。
*   **藍牙**：Bluetooth 5 (LE) 包含 Bluetooth Mesh，支援長距離模式（Long Range）與 2 Mbps 高速傳輸。

## 架構的先進特色
*   **AI 加速支援**：MCU 內部增加了用於加速神經網路計算與訊號處理的向量指令（Vector Instructions），非常適合語音辨識與圖像處理等 Edge AI 應用。
*   **豐富的 I/O 介面**：提供多達 45 個可程式化的 GPIO，支援 SPI、I2S、I2C、PWM、RMT、ADC、UART、SD/MMC 主機控制器及 TWAI® 控制器。
*   **硬體安全機制**：內建完善的安全架構，包含 AES-XTS 的 Flash 加密、Secure Boot V2、RSA 演算法、HMAC 以及數位簽章（Digital Signature）模組。

## 外型尺寸規格
*   **WROOM-1 模組尺寸**：18.0 mm × 25.5 mm × 3.1 mm
*   **標準 DevKitC-1 開發板尺寸**：約為 50.8 mm × 25.4 mm（長 × 寬）

## 文字化開發板接腳與結構示意圖
```text
               +---------------------------------------+
               |        [  PCB 天線 Antenna  ]         |
               |                                       |
          3V3 -| [ 1]                             [44] |- GND
     EN / CHIP-| [ 2]                             [43] |- IO0 
          IO4 -| [ 3]        ESP32-S3-WROOM-1     [42] |- IO48
          IO5 -| [ 4]            Module           [41] |- IO47
          IO6 -| [ 5]                             [40] |- IO21
          IO7 -| [ 6]                             [39] |- IO20
          IO15-| [ 7]                             [38] |- IO19
          IO16-| [ 8]                             [37] |- IO18
          IO17-| [ 9]                             [36] |- IO17
          IO18-| [10]                             [35] |- IO16
          IO8 -| [11]                             [34] |- IO15
          IO3 -| [12]                             [33] |- IO14
          IO46-| [13]                             [32] |- IO13
          IO9 -| [14]                             [31] |- IO12
          IO10-| [15]                             [30] |- IO11
          IO11-| [16]                             [29] |- IO10
          IO12-| [17]     +-------+  +-------+    [28] |- IO9
          IO13-| [18]     | Reset |  | Boot  |    [27] |- IO46
          IO14-| [19]     |  BTN  |  |  BTN  |    [26] |- IO3
           5V -| [20]     +-------+  +-------+    [25] |- 5V
          GND -| [21]                             [24] |- GND
               |         [ USB-C / Micro USB ]         |
               +---------------------------------------+
```

## 在 VS Code + PlatformIO 開發環境中需要怎麼設定？

### 專案設定檔 (platformio.ini)
```ini
[env:esp32-s3-devkitc-1]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
monitor_speed = 115200
; 針對特定需要開啟 USB CDC 的情況，可加上以下建置參數 (非必備)
; build_flags = 
;     -D ARDUINO_USB_MODE=1
;     -D ARDUINO_USB_CDC_ON_BOOT=1
```

### 測試範例程式碼 (src/main.cpp)
```cpp
#include <Arduino.h>

// ESP32-S3-DevKitC-1 的內建 LED 通常連接在 GPIO 38 或 48 (依板子版本有所不同)
#define LED_PIN 38 

void setup() {
  // 初始化序列埠通訊
  Serial.begin(115200);
  
  // 設定 LED 腳位為輸出模式
  pinMode(LED_PIN, OUTPUT);
  
  Serial.println("ESP32-S3 啟動完成，開始測試！");
}

void loop() {
  // 點亮 LED
  digitalWrite(LED_PIN, HIGH);
  Serial.println("LED 狀態: ON");
  delay(1000);
  
  // 關閉 LED
  digitalWrite(LED_PIN, LOW);
  Serial.println("LED 狀態: OFF");
  delay(1000);
}
```