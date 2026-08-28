# 💡 HUB 8735 ultra 開發板介紹

## 💻 核心硬體規格
- **主晶片**：Realtek RTL8735BDM (ARM v8M / Cortex-M 系列微控制器核心)
- **系統記憶體**：內建 128MB DDR2 記憶體
- **AI 運算核心**：內建 NPU (神經網路處理器) AI 運算引擎（約 0.4 TOPS）
- **影像與影音功能**：搭配 Full HD 1080P 高解析智慧鏡頭、硬體 H.264/H.265 影像編碼器、內建麥克風與混音器
- **外部儲存**：支援 MicroSD 記憶卡插槽（最高支援 512GB）
- **工作電壓**：5V（透過 USB 供電），I/O 邏輯準位為 3.3V

## 📡 支援的通訊方式
- **無線網路**：支援 802.11 a/b/g/n 雙頻 Wi-Fi（2.4GHz / 5GHz）
- **低功耗藍牙**：支援 BLE 5.1
- **串列通訊**：UART
- **總線介面**：I2C、SPI
- **控制與類比介面**：PWM、ADC、GPIO 數位/類比引腳

## 🌟 架構的先進特色
- **高度集成影像 AI**：完美結合高畫質視訊串流、硬體影音編碼與 NPU 加速，極適合用於邊緣運算（Edge AI）與智慧影像辨識。
- **國產晶片模組**：以台灣 IC 晶片模組為核心設計，具備高穩定性與在地化技術支援，並通過相關認證。
- **相容主流開發環境**：支援 Arduino 開發特性，降低開發門檻，方便快速串接各式感測器與雲端應用。

## 📏 外型尺寸規格
- **連接埠**：USB Type-C
- **模組設計**：整合相機鏡頭、麥克風、天線與擴充排針的高整合度智慧 AIoT 攝影機開發板。

## 🗺️ 文字化接腳與結構示意圖

```text
       +-----------------------------------+
       |            [ Camera ]             |
       +-----------------+-----------------+
                         |
      +------------------+------------------+
      |  ( ) 5V              GND ( )       |
      |  ( ) 3.3V          GPIO ( )       |
      |  ( ) TX / RX       I2C-SDA/SCL ( ) |
      |  ( ) SPI-SCK       SPI-MOSI ( )  |
      |  ( ) ADC           PWM ( )       |
      |  [ MicroSD Slot ]  [ USB-C ]      |
      +-------------------------------------+
                 HUB 8735 ultra
```

## ⚙️ 在 VS Code + PlatformIO 開發環境中需要怎麼設定？

### 專案設定檔 (`platformio.ini`)
```ini
[env:hub8735_ultra]
platform = realtek-ameba
board = rtl8735
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
  Serial.println("HUB 8735 ultra Running...");
  // 點亮 LED 燈
  digitalWrite(LED_BUILTIN, HIGH);
  delay(1000);
  // 熄滅 LED 燈
  digitalWrite(LED_BUILTIN, LOW);
  delay(1000);
}
```