# EAS_nanoowl（精簡版）

## 0. 專案介紹

### 0.1 介紹
EAS_nanoowl 為以文字提示（Prompt）驅動的邊緣視覺推論專案，支援即時目標偵測與 Tree Prompt 推論流程，可用於多場域的通用 AI 視覺應用。

- **Category**：通用領域（General-purpose Edge Vision AI）

<p align="center">
  <img src="https://raw.githubusercontent.com/NVIDIA-AI-IOT/nanoowl/main/assets/jetson_person_2x.gif" width="70%" />
</p>

### 0.2 模型介紹
- **OWL-ViT（Open-Vocabulary Detection）**：可用自然語言描述目標，不需重新訓練即可執行開放詞彙偵測。
- **Tree Prompt 推論**：可在同一推論流程中進行巢狀目標與語意判斷。
- **TensorRT 加速**：透過 TensorRT engine 提升邊緣裝置推論效率。

### 0.3 支援平台

| Platform | Hardware Spec | OS / SDK | 連結 |
|---|---|---|---|
| AIR-075 | RAM: 128/64 GB, Storage: 512 GB | JetPack 7.1 Install | Hardware: <https://docs.edge-ai-sdk.advantech.com/docs/Hardware/AI_System/Nvidia/Jetson%20Thor/AIR-075><br>Install: <https://docs.edge-ai-sdk.advantech.com/docs/Turtorial/Document/Linux/User_Manual-3.0> |

---

## 1. 環境建置

### 1.1 AI-Stack Check Info
請先確認目標裝置的 AI Stack 狀態：

```bash
cat /etc/nv_tegra_release
nvcc --version
python3 --version
docker --version
```

### 1.2 需要可存取相機（例如 `/dev/video0`）

```bash
ls -l /dev/video*
```

### 1.3 其他必要條件
- 已完成 NVIDIA Container Runtime 安裝
- 可正常執行 `docker compose`
- 可連外下載模型（首次部署）

---

## 2. 開發與佈署

### 2.1 使用 Docker YAML 啟動（含 MODEL_DIR）

```bash
MODEL_DIR=/opt/Advantech/EdgeAI/System/Nvidia_Jetson/VisionAI/app/nanoowl
mkdir -p "$MODEL_DIR"

cat > docker-compose.yml << 'YAML'
services:
  eas_nanoowl:
    image: nanoowl:jp7-thor-persist
    container_name: eas_nanoowl
    runtime: nvidia
    ipc: host
    shm_size: "2g"
    ports:
      - "7860:7860"
    devices:
      - "/dev/video0:/dev/video0"
    environment:
      MODEL_DIR: /models
      ENGINE_PATH: /models/engines/owl_image_encoder_patch32.engine
      LD_LIBRARY_PATH: /usr/local/lib/python3.12/dist-packages/tensorrt_libs:/opt/hostlibs:/usr/src/tensorrt/lib
    volumes:
      - "${MODEL_DIR}:/models"
      - "/usr/src/tensorrt:/usr/src/tensorrt:ro"
      - "/usr/lib/aarch64-linux-gnu/libnvinfer_plugin.so.10:/opt/hostlibs/libnvinfer_plugin.so.10:ro"
      - "/usr/lib/aarch64-linux-gnu/libnvinfer_plugin.so.10.13.3:/opt/hostlibs/libnvinfer_plugin.so.10.13.3:ro"
    command: ["/opt/nanoowl/docker/jetpack7-thor/run_tree_demo_persistent.sh"]
YAML

docker compose up -d
docker compose logs -f eas_nanoowl
```

### 2.2 服務驗證

```bash
curl -I http://127.0.0.1:7860/
```

UI 入口：`http://<device-ip>:7860`
