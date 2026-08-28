# 🔌 物聯網 IoT 開發板介紹與 VS Code + PlatformIO 選型指南

## 📡 1. ESP32-WROVER 核心模組介紹

ESP32-WROVER 是樂鑫科技（Espressif Systems）推出的一款高效能、通用型 Wi-Fi + 藍牙 / 藍牙低功耗（BLE）MCU 模組，其最核心的特色在於**內建或外接了 PSRAM（外部偽靜態隨機存取記憶體）**，非常適合需要處理大量數據、網頁伺服器或圖形介面的物聯網專案。

### ⚙️ 主要硬體規格
- **處理器**：Tensilica 雙核心 32 位元 Xtensa LX6 微控制器，運作頻率可達 160 或 240 MHz。
- **無線連接**：Wi-Fi (802.11 b/g/n) 與藍牙 (v4.2 BR/EDR 與 BLE)。
- **記憶體配置**：
  - 520 KB 內部 SRAM。
  - 4MB / 8MB SPI Flash。
  - **4MB / 8MB PSRAM**（提供額外的大容量記憶體，解決標準 ESP32 記憶體不足的痛點）。
- **周邊介面**：ADC、DAC、Touch Sensor、UART、SPI、I2C、PWM、I2S 等。

### 💡 典型應用場景
- 聯網相機與影像串流應用。
- 內建複雜網頁伺服器（Web Server）的高階 IoT 設備。
- 運行 MicroPython 或需要較大動態記憶體配置的系統。
- 智慧家居控制中心與圖形化使用者介面（GUI）。

---

## 💻 2. VS Code + PlatformIO 開發設定範例

在 VS Code 中使用 PlatformIO 開發 ESP32-WROVER 時，必須在設定檔中正確宣告 PSRAM 參數，否則系統無法正常呼叫擴充記憶體。

### 📄 `platformio.ini` 設定範例
```ini
[env:esp-wrover-kit]
platform = espressif32
board = esp-wrover-kit
framework = arduino
monitor_speed = 115200
build_flags = 
    -D BOARD_HAS_PSRAM
    -mfix-esp32-psram-cache-issue
```

### 📄 程式碼範例 (`src/main.cpp`)
```cpp
#include <Arduino.h>

void setup() {
    Serial.begin(115200);
    delay(1000);
    
    Serial.println("--- ESP32-WROVER 系統啟動 ---");
    
    // 檢查 PSRAM 是否成功載入
    if(psramFound()) {
        Serial.printf("PSRAM 總容量: %d bytes\n", ESP.getPsramSize());
        Serial.printf("PSRAM 可用空間: %d bytes\n", ESP.getFreePsram());
    } else {
        Serial.println("警告：未偵測到 PSRAM！");
    }
}

void loop() {
    Serial.printf("內部堆疊剩餘記憶體 (Heap): %d bytes\n", ESP.getFreeHeap());
    delay(5000);
}
```

---

## 🧭 3. VS Code + PlatformIO 開發板選型指南

在進行 IoT 專案開發時，選對開發板能讓開發事半功倍。以下為常見物聯網開發板選型對照表：

| 開發板系列 | 核心晶片 | 優勢與特色 | 推薦應用場景 | PlatformIO 板型代號 (Board ID) |
| :--- | :--- | :--- | :--- | :--- |
| **ESP32-WROVER** | ESP32 | 具備外接 PSRAM、雙核心、Wi-Fi/BT | 影像處理、大型網頁伺服器、複雜 IoT 設備 | `esp-wrover-kit` |
| **ESP32-WROOM** | ESP32 | 性價比高、社群資源極豐富、雙核心 | 一般智慧感測器、居家自動化、基礎聯網 | `esp32dev` |
| **ESP32-S3** | ESP32-S3 | 具備 AI 運算加速、原生 USB、豐富 GPIO | 機器學習邊緣運算、USB 裝置、彩色螢幕互動 | `esp32-s3-devkitc-1` |
| **ESP32-C3** | ESP32-C3 | RISC-V 架構、超低功耗、成本低廉 | 智慧照明、小型藍牙/Wi-Fi 終端節點 | `esp32-c3-devkitm-1` |
| **Raspberry Pi Pico W** | RP2040 | 雙核心 ARM、獨特的 PIO 狀態機 | 精準時序控制、多通道訊號採集、輕量連網 | `rpipicow` |

### 🔍 選型建議總結
1. **若專案需要處理畫面、快取大數據或運行較重負載**：優先選擇帶有 PSRAM 的 **ESP32-WROVER**。
2. **若為一般感測器上報、MQTT 傳輸等標準 IoT 專案**：性價比最高的 **ESP32-WROOM** 即可勝任。
3. **若需結合機器學習（AI）或豐富的人機介面**：建議轉向更新一代的 **ESP32-S3** 系列。