# 🔌 Ameba BW16 (Type-C / RTL8720DN) 物聯網開發板深度解析與指南

本文件針對 **Ameba BW16 (RTL8720DN)** 雙頻無線物聯網開發板進行全方位介紹，並結合 **VS Code + PlatformIO** 開發環境進行選型與實戰配置說明。

---

## 📋 目錄
1. [開發板核心規格](#1-開發板核心規格)
2. [硬體亮點與優勢](#2-硬體亮點與優勢)
3. [VS Code + PlatformIO 開發環境建置](#3-vs-code--platformio-開發環境建置)
4. [PlatformIO 專案配置範例 (platformio.ini)](#4-platformio-專案配置範例-platformioini)
5. [基礎範例程式碼 (Hello Wi-Fi & BLE)](#5-基礎範例程式碼-hello-wi-fi--ble)

---

## 1. 🌐 開發板核心規格

Ameba BW16 是一塊奠基於 Realtek RTL8720DN 晶片的高效能雙頻無線開發板，支援 2.4GHz 與 5GHz 雙頻 Wi-Fi 以及藍牙 BLE 5.0，非常適合應用於智慧家庭、IoT 閘道器與工業物聯網監控。

| 規格項目 | 內容說明 |
| :--- | :--- |
| **主晶片** | Realtek RTL8720DN (B&T BW16 模組) |
| **處理器架構** | 雙核心設計：ARM Cortex-M4 (KM4, 高達 200MHz) + ARM Cortex-M0 (KM0) |
| **無線網路 (Wi-Fi)** | 802.11 a/b/g/n (支援 **2.4GHz / 5GHz** 雙頻段) |
| **藍牙技術** | 藍牙 BLE 5.0 (支援主機與從機模式) |
| **記憶體 (Flash/RAM)** | 內建 4MB Flash / 457KB RAM |
| **連接介面** | **USB Type-C** (支援供電與序列埠通訊)、UART、SPI、I2C、GPIO、PWM |
| **開發框架支援** | Arduino IDE、Realtek Ameba SDK、**PlatformIO** |

---

## 2. ⚡ 硬體亮點與優勢

* **雙頻 Wi-Fi 支援**：有別於常見的 ESP8266 或早期 ESP32 僅支援 2.4GHz，BW16 支援 **5GHz 頻段**，能有效避開 2.4GHz 擁擠的干擾環境（如家用微波爐、藍牙設備及眾多 Wi-Fi 熱點）。
* **雙核心協同運作**：ARM Cortex-M4 負責繁重的網路協定與應用程式運算，Cortex-M0 則可處理底層通訊與低功耗狀態管理，提升系統穩定度。
* **Type-C 介面升級**：板載 Type-C 連接埠免除正反插困擾，並整合序列埠晶片，方便直接透過 VS Code 進行程式燒錄與 Log 偵錯。

---

## 3. 🛠️ VS Code + PlatformIO 開發環境建置

使用 **VS Code** 搭配 **PlatformIO IDE** 可以擺脫傳統 Arduino IDE 的陽春介面，享有強大的自動完成 (IntelliSense)、版本控制與現代化的專案管理結構。

### 安裝步驟：
1. **安裝 Visual Studio Code**：至官方網站下載並安裝主程式。
2. **安裝 PlatformIO Extension**：
   * 開啟 VS Code。
   * 點選左側的擴充功能 (Extensions) 圖示，搜尋並安裝 **PlatformIO IDE**。
   * 安裝完成後重新啟動 VS Code。
3. **建立專案**：
   * 透過 PlatformIO Home 點選 `New Project`。
   * 輸入專案名稱，於 Board 欄位搜尋 `BW16` 或 `RTL8720DN`（若無直接對應板型，可選擇通用 Realtek Ameba 架構或自定義 JSON 設定），Framework 選擇 `Arduino`。

---

## 4. ⚙️ PlatformIO 專案配置範例 (`platformio.ini`)

在 VS Code 專案根目錄下的 `platformio.ini` 設定檔內容範例：

```ini
[env:bw16]
platform = realtekameba
board = bw16
framework = arduino
monitor_speed = 115200
upload_speed = 1500000

; 若需指定序列埠，可在此手動設定 (例如 macOS 常用 /dev/cu.usbmodem...)
; upload_port = /dev/cu.usbmodem001
; monitor_port = /dev/cu.usbmodem001
```
## 5. 💻 基礎範例程式碼 (Wi-Fi 掃描器)
以下是一段透過 Arduino 框架在 Ameba BW16 上運行的基礎測試程式碼，用以掃描周遭的 2.4GHz 與 5GHz 無線網路：

```c
#include <WiFi.h>

void setup() {
  // 初始化序列埠通訊
  Serial.begin(115200);
  while (!Serial) {
    delay(100);
  }

  // 設定 Wi-Fi 為 Station 模式
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(1000);

  Serial.println("Ameba BW16 雙頻 Wi-Fi 掃描器已啟動...");
}

void loop() {
  Serial.println("開始掃描附近無線網路...");
  
  // 執行網路掃描
  int n = WiFi.scanNetworks();
  Serial.println("掃描完成！");

  if (n == 0) {
    Serial.println("未發現任何 Wi-Fi 訊號。");
  } else {
    Serial.print("共找到 ");
    Serial.print(n);
    Serial.println(" 個網路：");
    
    for (int i = 0; i < n; ++i) {
      // 輸出頻道、訊號強度、加密類型及 SSID
      Serial.print(i + 1);
      Serial.print(": [");
      Serial.print(WiFi.channel(i)); // 支援 2.4G 及 5G 頻道顯示
      Serial.print("GHz] ");
      Serial.print(WiFi.SSID(i));
      Serial.print(" (訊號強度 RSSI: ");
      Serial.print(WiFi.RSSI(i));
      Serial.println(" dBm)");
      delay(10);
    }
  }
  
  Serial.println("----------------------------------------");
  // 每隔 10 秒重新掃描一次
  delay(10000);
}
```