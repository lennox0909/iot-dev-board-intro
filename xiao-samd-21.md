# ⚡ Seeed Studio XIAO SAMD21 開發板介紹

## 💻 核心硬體規格
- **微控制器**：Microchip ATSAMD21G18（ARM Cortex-M0+ 32位元處理器）
- **運作頻率**：最高 48 MHz
- **快閃記憶體 (Flash)**：256 KB
- **靜態隨機存取記憶體 (SRAM)**：32 KB
- **工作電壓**：3.3V（邏輯準位），5V（透過 USB 供電）
- **引腳介面**：
  - 11 個數位引腳（可作 PWM 輸出）
  - 11 個類比引腳（支援 12 位元 ADC 輸入）
  - 1 個 DAC 類比輸出引腳

## 📡 支援的通訊方式
- **UART**：1 組（Tx / Rx）
- **I2C**：1 組（SDA / SCL）
- **SPI**：1 組（MISO / MOSI / SCK）
- **USB**：內建原生 USB 2.0 Type-C 介面（支援序列埠通訊與韌體燒錄）

## 🌟 架構的先進特色
- **極致微型設計**：尺寸僅有 21 mm x 17.8 mm，極適合穿戴式裝置及空間受限的微型聯網專案。
- **低功耗運作**：基於 ARM Cortex-M0+ 架構，具備優異的能源效率，適合電池供電的長效型物聯網節點。
- **親民的擴充性**：兩側採用標準半孔焊盤與排針設計，相容於麵包板與外掛擴充板。

## 📏 外型尺寸規格
- **尺寸大小**：21 mm × 17.8 mm
- **連接埠**：USB Type-C
- **封裝形式**：DIP / SMT 兩用雙排半孔焊盤

## 🗺️ 文字化接腳與結構示意圖

```text
       +-------------------+
       |     [Type-C]      |
       +---------+---------+
                 |
      +----------+----------+
      |  ( ) 5V     GND ( ) |
      |  ( ) 3.3V  RX/D6 ( ) |
      |  ( ) A0/D0 TX/D7 ( ) |
      |  ( ) A1/D1  SDA/D8 ( ) |
      |  ( ) A2/D2  SCL/D9 ( ) |
      |  ( ) A3/D3  SCK/D10|( )
      |  ( ) A4/D4  MISO/D ( ) | -> MISO/D11
      |  ( ) A5/D5  MOSI/D ( ) | -> MOSI/D12
      +---------------------+
           XIAO SAMD21
```

## ⚙️ 在 VS Code + PlatformIO 開發環境中需要怎麼設定？

### 專案設定檔 (`platformio.ini`)
```ini
[env:seeed_xiao]
platform = atmelsam
board = seeed_xiao
framework = arduino
```

### 測試範例程式碼 (`src/main.cpp`)
```cpp
#include <Arduino.h>

void setup() {
  // 初始化內建 LED 腳位為輸出模式
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  // 點亮 LED 燈
  digitalWrite(LED_BUILTIN, HIGH);
  delay(1000);
  // 熄滅 LED 燈
  digitalWrite(LED_BUILTIN, LOW);
  delay(1000);
}
```