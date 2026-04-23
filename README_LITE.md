# NanoOWL

## 介紹
EAS_nanoowl 提供以文字驅動（prompt-driven）的視覺推論能力，可在邊緣裝置上執行即時目標偵測與樹狀語意推論（Tree Prediction），適合智慧製造、零售場域、公共空間與工業監控等情境的快速 PoC 與量產導入。
原始專案：https://github.com/NVIDIA-AI-IOT/nanoowl
- **Category**：通用領域（General-purpose Edge Vision AI）

<p align="center">
  <img src="https://raw.githubusercontent.com/NVIDIA-AI-IOT/nanoowl/main/assets/jetson_person_2x.gif" width="70%" />
</p>

## 模型
EAS_nanoowl 核心採用 NanoOWL 技術路線，整合以下模型能力：

- **OWL-ViT（Open-Vocabulary Detection）**  
  以自然語言描述目標類別，支援不需重訓即可進行開放詞彙偵測。
- **Tree Prompt 推論流程**  
  支援巢狀提示（nested prompts），可在同一流程中完成「物件偵測 + 細部屬性/子物件判定」。
- **TensorRT 加速路徑**  
  使用 TensorRT engine 提升推論效能，兼顧即時性與部署可行性。

## 支援平台

| Platform | Hardware Spec | OS | Edge AI SDK |
|---|---|---|---|
| AIR-075 | NVIDIA Jetson Thor - RAM: 128/64 GB, Storage: 512 GB | JetPack 7.1 | [Install](https://docs.edge-ai-sdk.advantech.com/docs/Hardware/AI_System/Nvidia/Jetson%20Thor/AIR-075)) |

---

# Prerequisites

## Check AI Stack
請先確認目標裝置的 AI Stack 狀態：

```bash
cat /etc/nv_tegra_release
nvcc --version
python3 --version
docker --version
```

## 需要可存取相機（例如 `/dev/video0`）

```bash
ls -l /dev/video*
```

---

# Develop and Development

## Setup 1 :Launch the demo

```bash
MODEL_DIR=/opt/Advantech/EdgeAI/System/Nvidia_Jetson/VisionAI/app/nanoowl
mkdir -p "$MODEL_DIR"
MODEL_DIR="$MODEL_DIR" docker compose up -d
MODEL_DIR="$MODEL_DIR" docker compose logs -f eas_nanoowl
```

## Setup 2 : Open your browser

Open your browser to `http://<device-ip>:7860`


### Setup 3 :使用 Docker YAML 關閉

```bash
MODEL_DIR=/opt/Advantech/EdgeAI/System/Nvidia_Jetson/VisionAI/app/nanoowl
MODEL_DIR="$MODEL_DIR" docker compose down
```

# 常見問題

- camera 無法開啟：
  1) 先確認 camera 是否正常連接到主機。  
  2) 確認 `docker-compose.yml` 內有正確映射裝置：

  ```yaml
  devices:
    - "/dev/video0:/dev/video0"
  ```

  若缺少此設定，容器將無法存取 camera。

- 7860 端口衝突：
  停止舊容器或其他佔用 7860 端口的容器後，再重新啟動。
