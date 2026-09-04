# NuMaker-VoiceAI / TeachableMachine_Local_Studio_v3 — Git 安裝步驟

本文件說明如何建立 Python 開發環境、燒錄 Firmware，以及啟動 Local Teachable Machine Studio 的完整流程。

## Step 1. 建立 Python 環境

安裝 **Python v3.13.15**（請直接使用提供版本，避免版本差異造成異常）。

## Step 2. 建立 Download Firmware 環境

1. 安裝 [NuMicro ICP programming tool](https://www.nuvoton.com/resource-download.jsp?tp_GUID=SW1720200221181328&currentFolder=/products/microcontrollers/arm-cortex-m23-mcus/m2l31-series/&t=1788155572)。

2. 開啟 ICP programmer，選擇 **M5531** 系列：

   ![開啟 ICP programmer 選擇 M5531 系列](images/01_icp_tool_select_m5531.png)

3. 燒錄檔案設定：
   - **APROM** 內載入 `/1_Collect Firmware_bin/DMIC_UAC_Codec_Monitor.bin`
   - **LDROM** 內載入 `/1_Collect Firmware_bin/ISP_MSC 2.bin`
   - **Setting** 配置 `boot from LDROM`

4. 配置完成後，點選 **Start → batch programming mode (No)**：

   ![Batch programming mode 設定畫面](images/02_icp_batch_programming_setting.png)

## Step 3. 建立 Local Teachable Machine 環境

> ⚠️ 安裝過程中請避免防火牆造成安裝失敗。

- `01_INSTALL.bat`：會自動安裝好環境（Python 虛擬環境 `.venv`、相依套件等）。
- `02_START.bat`：啟動後可以進行模型訓練 (Train Model)。

![TeachableMachine_Local_Studio_v3 目錄下的 bat 檔案](images/03_local_studio_bat_files.png)

---

## 疑難排解 (Troubleshooting)

### `ModuleNotFoundError: No module named 'numpy'`

若執行 `02_START.bat` 出現 `Local Studio FastAPI 應用程式無法載入` 且提示缺少 `numpy`，代表 `.venv` 虛擬環境內尚未安裝相依套件。請先啟用虛擬環境並補齊套件：

```powershell
cd TeachableMachine_Local_Studio_v3
.venv\Scripts\activate
pip install -r requirements.txt
```

### `01_INSTALL.bat` 安裝失敗（pip install exit code 1）

若安裝過程中出現：

```
[ERROR] Installation failed: InstallFailure: Command failed with exit code 1:
...python.exe -m pip install --upgrade --timeout 240 --retries 5 pip setuptools wheel
```

代表在建立虛擬環境時無法連線至 PyPI，常見原因為公司網路 Proxy 阻擋或 SSL 憑證驗證失敗。可先手動測試：

```powershell
.venv\Scripts\python.exe -m pip install --upgrade pip
```

並依錯誤訊息判斷是否需要設定 Proxy（`HTTP_PROXY` / `HTTPS_PROXY`）或加入 `--trusted-host pypi.org --trusted-host files.pythonhosted.org`。詳細診斷請參考：

```
logs\LATEST_INSTALL.log
```

---

## 目錄結構建議

```
TeachableMachine_Local_Studio_v3/
├── README.md
├── images/
│   ├── 01_icp_tool_select_m5531.png
│   ├── 02_icp_batch_programming_setting.png
│   └── 03_local_studio_bat_files.png
├── 01_INSTALL.bat
├── 02_START.bat
└── ...
```

## 修訂紀錄 (Revision History)

| 日期 | 版本 | 說明 |
|------|------|------|
| 2026.09.04 | 1.0 | 由 Word 文件轉換為 GitHub README.md 格式，圖片改以相對路徑嵌入。 |
