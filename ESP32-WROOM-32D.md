# 🔌 智慧聯網時代：物聯網 IoT 開發板介紹

---

## 🌐 什麼是物聯網 IoT 開發板？

物聯網（IoT）開發板是集成了微控制器（MCU）、無線通訊模組（如 Wi-Fi、藍牙等）、周邊介面（GPIO、ADC、I2C、SPI 等）以及電源管理電路的硬體平台。它們讓開發者能夠快速從「概念驗證（PoC）」走向「產品原型」，廣泛應用於智慧家居、工業監控、環境感測與穿戴式裝置。

---

## 🛠️ VS Code + PlatformIO 開發板選型指南

在眾多開發環境中，**VS Code 結合 PlatformIO** 是目前最受推崇的現代化 IoT 開發方案。相比傳統的 Arduino IDE，它具備以下優勢：

1. **強大的專案與依賴管理 (`platformio.ini`)**：自動下載並管理第三方庫，避免版本衝突。
2. **專業級程式碼輔助**：透過 IntelliSense 提供精準的自動完成與重構。
3. **內建強大除錯與序列埠監控**：支援進階偵錯與多環境切換。
4. **跨平台支援**：完美支援 Windows、macOS 與 Linux。

### 選型核心維度對照表

| 選型維度 | 考量重點 | 推薦方向 |
| :--- | :--- | :--- |
| **通訊協定** | 需不需要 Wi-Fi、藍牙、LoRa 還是僅需 BLE？ | 雲端連線首選 ESP32 系列；低功耗感測首選 nRF52 系列。 |
| **運算效能** | 僅收集資料，還是需要邊緣運算（Edge AI）？ | 簡單邏輯用 ESP32-C3；複雜任務用雙核心 ESP32。 |
| **功耗需求** | 插電使用，還是電池供電（需超長待機）？ | 電池供電應選擇 ESP32 的 Deep Sleep 模式或 Nordic nRF 系列。 |
| **開發生態** | 社群資源是否豐富、教學文件是否好找？ | 選擇在 PlatformIO 支援度極高的 Espressif (ESP32) 或 STMicroelectronics。 |

---

## 🔥 明星級開發板深度解析：ESP32-WROOM-32D

在眾多 IoT 開發板中，**ESP32-WROOM-32D** 是目前性價比最高、社群最龐大的明星級開發板之一。

### 核心規格數據
* **微控制器**：Tensilica Xtensa 雙核心 32 位元 LX6 微控制器（運作頻率高達 240 MHz）
* **無線通訊**：Wi-Fi (802.11 b/g/n) + 藍牙 (Bluetooth v4.2 BR/EDR 與 BLE)
* **記憶體**：4 MB SPI Flash，520 KB SRAM
* **周邊介面**：34 個可規劃 GPIO、12-bit ADC、觸控感測接腳、I2C、SPI、UART、PWM

### 文字化接腳與結構示意圖
```text
       [ ESP32-WROOM-32D 開發板示意 ]
       
             +--------------+
       3V3 --| 1        38  |-- GND
       EN  --| 2        37  |-- D23 (VSPI MOSI)
      IO36 --| 3 (VP)   36  |-- D22 (I2C SCL)
      IO39 --| 4 (VN)   35  |-- TXD0
      IO34 --| 5        34  |-- RXD0
      IO35 --| 6        33  |-- D21 (I2C SDA)
      IO32 --| 7        32  |-- GND
      IO33 --| 8        31  |-- D19 (VSPI MISO)
      IO25 --| 9        30  |-- D18 (VSPI SCK)
      IO26 --| 10       29  |-- D5  (VSPI SS)
      IO27 --| 11       28  |-- D17
      IO14 --| 12       27  |-- D16
      IO12 --| 13       26  |-- D4
     GND   --| 14       25  |-- D0
      IO13 --| 15       24  |-- D2
      SD2  --| 16       23  |-- D15
      SD3  --| 17       22  |-- CMD
      CMD  --| 18       21  |-- CLK
             +--------------+
```

---

## 💻 PlatformIO 專案配置與程式碼範例

### 1. 專案設定檔 (`platformio.ini`)
```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
upload_speed = 921600
```

### 2. 測試範例程式碼 (`src/main.cpp`)
```cpp
#include <Arduino.h>
#include <WiFi.h>

const char* ssid = "Your_WiFi_SSID";
const char* password = "Your_WiFi_Password";

void setup() {
    Serial.begin(115200);
    delay(1000);

    Serial.println("\n--- ESP32-WROOM-32D 啟動中 ---");

    WiFi.begin(ssid, password);
    Serial.print("正在連線至 Wi-Fi");
    
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }

    Serial.println("\nWi-Fi 連線成功！");
    Serial.print("IP 位址: ");
    Serial.println(WiFi.localIP());
}

void loop() {
    Serial.printf("目前可用 Free Heap 記憶體: %d bytes\n", ESP.getFreeHeap());
    delay(5000);
}
```