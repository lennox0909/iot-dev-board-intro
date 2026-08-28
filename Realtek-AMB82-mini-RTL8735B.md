# 📷 Realtek AMB82-mini (RTL8735B) AI 攝影機與物聯網開發板介紹

## ⚙️ 核心硬體規格
- **主晶片 (SoC)：** Realtek RTL8735BDM (AmebaPro2 系列)
- **處理器 (MCU)：** 32-bit Arm v8M (支援 TrustZone 安全架構)，時脈高達 500 MHz
- **神經網路引擎 (NPU)：** 內建智慧硬體引擎，運算效能達 0.4 TOPS，支援 YOLOv4-Tiny、TensorFlow Lite 等輕量化 AI 模型
- **記憶體配置：** 768KB ROM、512KB SRAM、16MB 外部 SPI Flash，並在模組內嵌高達 128MB DDR2 記憶體
- **視訊與影像處理 (ISP)：** 支援 HDR、3DNR、WDR 影像優化；內建 H.264 / H.265 / JPEG 硬體視訊編碼器，最高支援 1080p @ 30fps + 720p @ 30fps 雙碼流輸出
- **相機模組：** 搭配 JXF37 1920×1080 全高清 CMOS 圖像感測器（視角 FOV 130°）
- **音訊介面：** 板載麥克風，支援 ADC、DAC 與 I2S 數位音訊介面

## 📡 支援的通訊方式
- **Wi-Fi 網路：** 802.11 a/b/g/n (1×1)，支援 2.4GHz 與 5GHz 雙頻無線網路
- **藍牙技術：** 藍牙低功耗 (BLE 5.1)
- **硬體週邊介面：** 最多 23 個 GPIO、2組 SPI、1組 I2C、3組 UART、8組 PWM、3組 ADC、2組 GDMA、USB Host / Device 以及 MicroSD 卡槽 (SD Host)

## 💡 架構的先進特色
- **邊緣 AI 運算能力：** 內建 0.4 TOPS 專用 NPU，可在本地端直接執行即時物件偵測與人臉識別，無須依賴雲端伺服器。
- **高畫質即時串流：** 支援多碼流即時 H.265 / H.264 編碼，非常適合應用於智慧安防監控與 IP Camera 串流。
- **極速啟動與低功耗：** 具備毫秒級快速開機特性與微安級超低功耗設計，完美適配電池供電的智慧物聯網設備。
- **硬體資安防護：** 支援硬體密碼引擎、安全啟動 (Secure Boot) 與信任區 (TrustZone) 保護機制。

## 📏 外型尺寸規格
- **板型大小：** 約 60 mm × 37.4 mm
- **實體配置：** 配備 2 個 Micro USB-B 連接埠（供電與序列埠除錯）、MicroSD 卡插槽、IPEX 外部天線座、Reset 按鍵與 User 輕觸開關

## 🗺️ 文字化開發板接腳與結構示意圖

```text
+---------------------------------------------------------------+
|  [MicroSD Slot]                   [IPEX Antenna Connector]    |
|                                                               |
|          +-----------------------------------------+          |
|          |         Realtek RTL8735B SoC            |          |
|          |           (Arm v8M @ 500MHz)            |          |
|          |             + 0.4 TOPS NPU              |          |
|          |                                         |          |
|          |     [H.264 / H.265 Hardware Encoder]    |          |
|          +-----------------------------------------+          |
|                                                               |
|   [USB_UART] [USB_POWER]   [RST_BTN] [USER_BTN]  [MIC]        |
+---------------------------------------------------------------+
| Pin Header Left : (3V3) (GND) (D0) (D1) (D2) (D3) (A0) (A1)   |
| Pin Header Right: (5V)  (TX)  (RX) (SDA)(SCL)(PWM0)(PWM1)(GND)|
+---------------------------------------------------------------+
```

## 🛠️ 在 VS Code + PlatformIO 開發環境中需要怎麼設定？

雖然 Realtek Ameba 官方主要透過 Arduino IDE 或原生 GCC SDK 進行開發，若要在 PlatformIO 中進行專案整合與架構管理，可透過自訂平台或相容核心進行環境配置。

### 1. 專案設定檔 (`platformio.ini`)

```ini
[env:amb82_mini]
platform = realtek-ameba
board = amb82_mini
framework = arduino
monitor_speed = 115200
build_flags = 
    -DARDUINO_AMB82_MINI
```

### 2. 測試範例程式碼 (`src/main.cpp`)

```cpp
#include "WiFi.h"

// 設定您的無線網路 SSID 與密碼
char ssid[] = "Your_WiFi_SSID";     
char pass[] = "Your_WiFi_Password"; 
int status = WL_IDLE_STATUS;

void setup() {
    Serial.begin(115200);
    while (!Serial) {
        ; // 等待序列埠連線
    }

    // 嘗試連線至 Wi-Fi 網路
    while (status != WL_CONNECTED) {
        Serial.print("Attempting to connect to WPA SSID: ");
        Serial.println(ssid);
        status = WiFi.begin(ssid, pass);
        delay(5000);
    }

    Serial.println("Connected to Wi-Fi network successfully!");
    Serial.print("IP Address: ");
    Serial.println(WiFi.localIP());
}

void loop() {
    // 系統主迴圈：可在此處加入 AI 影像辨識或感測器讀取邏輯
    delay(1000);
}
```