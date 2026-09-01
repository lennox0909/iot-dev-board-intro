# 📷 HUB 8735 SMART AI CAM 開發板介紹

## ⚙️ 核心硬體規格
* **核心晶片**：採用 Realtek Ameba RTL8735 晶片。
* **神經網路處理器 (NPU)**：內置 NPU AI 運算引擎，可加速邊緣端 AI 模型處理。
* **影像處理**：具備多功能影像處理的高度集成模組，支援 Full HD 1080P 高解析智慧鏡頭。
* **音訊功能**：內建 MIC 語音輸入功能，支援語音及聲學應用開發。
* **傳輸介面**：採用 USB Type-C 介面，簡化供電與程式燒錄流程。

## 📡 支援的通訊方式
* **Wi-Fi 網路**：提供 802.11 a/b/g/n 標準的 2.4G/5G 雙頻 Wi-Fi 無線傳輸。
* **藍牙 (Bluetooth)**：內建 BLE (Bluetooth Low Energy) 低耗電藍牙傳輸功能。

## 🚀 架構的先進特色
* **邊緣端 AI 運算**：可直接運行多款 Pre-trained AI Model (如人臉辨識、物件偵測等)，不需將資料上傳雲端即可完成運算，大幅降低延遲並提升隱私安全。
* **豐富的視覺擴充**：支援與原相科技合作的多款鏡頭選用，包含標準、近焦、廣角等鏡頭模組，賦予開發者極大的設計彈性。
* **低門檻快速上手**：全面兼容 Arduino 開發環境，可輕易取代傳統一般 IoT Camera，快速實現 AIoT 場域落地應用。

## 📐 外型尺寸規格
採用高度集成的精簡模組化設計，體積小巧，便於隱藏與嵌入至各式物聯網感測器及邊緣設備外殼中。兩側具備標準的 GPIO 排針孔位，底端配備 Type-C 介面與功能按鍵，兼顧了開發便利性與工業整合需求。

## 📍 文字化開發板接腳與結構示意圖

```text
       +------------------------------------+
       |          [ Camera Connector ]      |
       |                                    |
   3V3 | [ ]                            [ ] | GND
   GND | [ ]                            [ ] | 5V
 TX/D1 | [ ]        RTL8735             [ ] | D13
 RX/D0 | [ ]    (NPU AI Engine)         [ ] | D12
    D2 | [ ]                            [ ] | D11
    D3 | [ ]                            [ ] | D10
    D4 | [ ]         [ MIC ]            [ ] | D9
    D5 | [ ]                            [ ] | D8
    D6 | [ ]                            [ ] | D7
       |                                    |
       |       [ BOOT ]      [ RESET ]      |
       |             [Type-C]               |
       +------------------------------------+
```

## 💻 在 VS Code + PlatformIO 開發環境中需要怎麼設定？

若要在 VS Code 搭配 PlatformIO 環境中進行開發，需透過 `realtek-ameba` 平台進行設定。

### 專案設定檔 (`platformio.ini`)

```ini
[env:ameba_pro2]
platform = realtek-ameba
board = ameba_pro2
framework = arduino
monitor_speed = 115200
```

### 測試範例程式碼 (`src/main.cpp`)

```cpp
#include <Arduino.h>

void setup() {
    // 初始化序列埠通訊
    Serial.begin(115200);
    
    // 設定內建 LED 為輸出模式 (依實際開發板腳位定義調整)
    pinMode(LED_BUILTIN, OUTPUT);
    
    Serial.println("HUB 8735 SMART AI CAM 初始化完成！");
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    Serial.println("System Running - LED ON");
    delay(1000);
    
    digitalWrite(LED_BUILTIN, LOW);
    Serial.println("System Running - LED OFF");
    delay(1000);
}
```