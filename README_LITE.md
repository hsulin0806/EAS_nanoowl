# NanoOWL

EAS_nanoowl 提供以文字驅動（prompt-driven）的視覺推論能力，可在邊緣裝置上執行即時目標偵測與樹狀語意推論（Tree Prediction），適合智慧製造、零售場域、公共空間與工業監控等情境的快速 PoC 與量產導入。


- **Category**：通用領域（General-purpose Edge Vision AI）

<p align="center">
  <img src="https://raw.githubusercontent.com/NVIDIA-AI-IOT/nanoowl/main/assets/jetson_person_2x.gif" width="70%" />
</p>

## OWL-ViT

- **OWL-ViT（Open-Vocabulary Detection）**  
  Uses natural language descriptions for target classification, enabling open-vocabulary object detection without retraining.
- **Tree Prompt **  
  Supports nested prompts, allowing object detection and fine-grained attribute/sub-object recognition within a single pipeline.
- **TensorRT Optimization**  
  Leverages a TensorRT engine to improve inference efficiency while maintaining real-time performance and deployability

## 支援平台

| Platform | Hardware Spec | OS | Edge AI SDK |
|---|---|---|---|
| AIR-075 | NVIDIA Jetson Thor - RAM: 128/64 GB, Storage: 512 GB | JetPack 7.1 | [Install](https://docs.edge-ai-sdk.advantech.com/docs/Hardware/AI_System/Nvidia/Jetson%20Thor/AIR-075) |

---

# Setup

## Step 1: Download this project
```bash
cd /opt/Advantech/EdgeAI/EdgeAIHub \
git clone https://github.com/hsulin0806/EAS_nanoowl
```

## Step 2: Check AI Environment
```bash
/opt/Advantech/EdgeAI/System/System_Check/ai_stack_check.sh
```
Pass

## Step 3: Ensure you have a camera device connected

```bash
ls -l /dev/video*
```
If no video device is found, exit from the container and check if you can see a video device on the host side.

---

# Develop and Development

## Setup 1 : Build Docker Image
```bash
cd /opt/Advantech/EdgeAI/EdgeAIHub/NanoOWL/
docker build -t nanoowl:jp7-thor-persist -f docker/jetpack7-thor/Dockerfile .
```

## Setup 1 :Launch the demo

```bash
docker compose up -d
```

## Setup 2 : Open your browser

Open your browser to `http://<device-ip>:7860`


## Setup 3 : Stop the demo

# Result
<p align="center"> <img src="https://www.jetson-ai-lab.com/images/tutorials/nanoowl_chrome_window.png" width="70%" /> </p>
