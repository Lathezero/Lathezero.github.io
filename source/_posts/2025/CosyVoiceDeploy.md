---
title: CosyVoiceDeploy
date: 2025-01-05 21:30:13
categories: 
  - [模型部署, 语音合成]
tags: [模型部署, 语音合成]
description: CosyVoice 本地部署教程
keywords: CosyVoice
---

```
pip install cython -i https://pypi.tuna.tsinghua.edu.cn/simple
```

```
conda install -y -c conda-forge pynini==2.1.5
```

```
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host=mirrors.aliyun.com
```

```
python -c"import torch;print(torch.__version_)
```



CosyVoice 是阿里通义实验室开源的多语言语音合成模型，支持音色克隆、情感控制和跨语种语音合成等功能。以下是一个详细的本地部署教程，帮助你在本地部署并使用 CosyVoice 模型。

---

## **本地部署 CosyVoice 的步骤**

### **1. 环境准备**(无需理会)

- **硬件要求**：
  - NVIDIA 显卡，建议显存 6GB 以上。
  - 确保系统路径和文件名不含中文或空格。
- **软件依赖**：
  - **CUDA 和 cuDNN**：用于 GPU 加速。检查并安装与显卡兼容的 CUDA 版本（建议 CUDA 12.x）和对应的 cuDNN 版本。
  - **Git**：用于克隆代码仓库。
  - **Miniconda**：用于创建 Python 虚拟环境。

---

### **2. 克隆代码仓库**

1. 打开命令行窗口，执行以下命令克隆 CosyVoice 仓库：

   ```bash
   git clone --recursive https://github.com/FunAudioLLM/CosyVoice.git
   cd CosyVoice
   git submodule update --init --recursive
   ```

   - 如果克隆失败，可以尝试多次或使用代理。

---

### **3. 创建虚拟环境**

1. 使用 Miniconda 创建虚拟环境：

   ```bash
   conda create -n cosyvoice python=3.8
   conda activate cosyvoice
   ```

2. 安装前置条件：

   ```
   pip install cython -i https://pypi.tuna.tsinghua.edu.cn/simple
   ```

   ```
   conda install -y -c conda-forge pynini==2.1.5
   ```

3. 安装依赖库：

   - 修改 `requirements.txt` 文件，删除`onnxruntime`这一行，并将 `onnxruntime-gpu` 后面的条件判断`;`后面一整段删除

   - 使用国内镜像加速安装：

     ```bash
     pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host=mirrors.aliyun.com
     ```

   - 如果 `torch` 安装过慢，可以手动下载并安装：

     ```bash
     pip install torch-2.0.1+cu118-cp38-cp38-win_amd64.whl -i https://pypi.tuna.tsinghua.edu.cn/simple
     ```

   - 查看`torch`是否正确安装：

     ```
     python -c"import torch;print(torch.__version_)
     ```

---

### **4. 下载模型**

1. 使用 ModelScope 下载预训练模型：

   - 创建一个 Python 脚本 `download_models.py`，内容如下：

     ```python
     from modelscope import snapshot_download
     snapshot_download('iic/CosyVoice-300M', local_dir='pretrained_models/CosyVoice-300M')
     snapshot_download('iic/CosyVoice-300M-SFT', local_dir='pretrained_models/CosyVoice-300M-SFT')
     snapshot_download('iic/CosyVoice-300M-Instruct', local_dir='pretrained_models/CosyVoice-300M-Instruct')
     snapshot_download('iic/CosyVoice-ttsfrd', local_dir='pretrained_models/CosyVoice-ttsfrd')
     ```

   - 运行脚本：

     ```bash
     python download_models.py
     ```

   - 如果下载速度慢，可以从百度网盘或夸克网盘下载模型并解压到 `pretrained_models` 目录。

---

### **5. 启动模型**

对于零样本/跨语言推理，请使用 `CosyVoice-300M` 模型。对于 sft 推理，请使用 `CosyVoice-300M-SFT` 模型。对于指令推理，请使用 `CosyVoice-300M-Instruct` 模型。

1. **内置音色生成**：

   - 在命令行中运行：

     ```bash
     python webui.py --port 50000 --model_dir pretrained_models/CosyVoice-300M-SFT
     ```
     
   - 打开浏览器访问 `http://127.0.0.1:50000` 使用 Web 界面。
   
2. **音色克隆**：

   - 准备一段 3~10 秒的音频作为音色样本。

   - 运行以下命令：

     ```bash
     python webui.py --port 50001 --model_dir pretrained_models/CosyVoice-300M
     ```

3. **语气微调**：

   - 使用 `CosyVoice-300M-Instruct` 模型，支持富文本输入（如 `<strong>` 强调语气）：

     ```bash
     python webui.py --port 50002 --model_dir pretrained_models/CosyVoice-300M-Instruct
     ```

---

### **6. 常见问题**

- **SSLError 或 HTTPSConnectionError**：设置代理端口：

  ```bash
  set http_proxy=http://127.0.0.1:你的代理端口地址 & set https_proxy=http://127.0.0.1:你的代理端口地址
  ```

- **模型下载失败**：更新 `modelscope` 到最新版本：

  ```bash
  pip install modelscope==1.17.0 -i https://pypi.tuna.tsinghua.edu.cn/simple
  ```

---

### **7. 进阶功能**

- **跨语种语音合成**：输入中文语音，输出其他语言的语音。
- **情感控制**：通过富文本标签（如 `<laughter>`）控制语音情感。

---

## **总结**

通过以上步骤，你可以在本地成功部署并使用 CosyVoice 模型。如果需要更详细的教程或遇到问题，可以参考相关文档或视频教程。

根据搜索结果，以下是 **CosyVoice** 系列模型的详细解释和适用场景：

---



## **1. CosyVoice-300M**

- **功能**：这是 CosyVoice 的基座模型，支持 **零样本音色克隆** 和 **跨语言语音合成**。
- **特点**：
  - 仅需 3~10 秒的音频样本，即可生成与目标音色高度相似的语音，包括韵律、情感等细节。
  - 支持多语言（如中文、英文、日语、粤语、韩语）的语音合成。
- **适用场景**：
  - 音色克隆：适用于需要快速复刻特定音色的场景，如虚拟助手、有声读物等。
  - 跨语言语音合成：输入一种语言的语音，输出另一种语言的语音，同时保留原始音色。

---

## **2. CosyVoice-300M-SFT**

- **功能**：这是经过 **SFT（Supervised Fine-Tuning）微调** 的模型，内置了多个预训练音色。
- **特点**：
  - 支持多种预定义音色（如中文女声、中文男声、日语男声、粤语女声等）。
  - 在特定任务或领域的语音生成质量上表现更优。
- **适用场景**：
  - 内置音色生成：适用于需要快速生成高质量语音的场景，如语音客服、语音播报等。
  - 多语言支持：支持多种语言的语音合成，适合国际化应用。

---

## **3. CosyVoice-300M-Instruct**

- **功能**：这是支持 **细粒度控制** 的模型，允许通过自然语言指令或富文本标签控制语音生成。
- **特点**：
  - 支持情感、口音、角色风格等细粒度控制。
  - 内置丰富的指令集，如 `<laughter>`（笑声）、`<strong>`（强调）、`[breath]`（呼吸声）等。
  - 支持多语言和方言控制（如四川话）。
- **适用场景**：
  - 情感语音生成：适用于需要表达特定情感的语音合成，如影视配音、游戏角色语音等。
  - 指令控制：适合需要动态调整语音风格和情感的场景，如虚拟助手、互动播客等。

---

## **4. CosyVoice-ttsfrd**

- **功能**：这是 CosyVoice 的 **文本规范化工具**，用于优化文本到语音的转换效果。
- **特点**：
  - 提供更好的文本归一化性能，支持多语言文本处理。
  - 可选的安装包，默认使用 WeTextProcessing 作为替代。
- **适用场景**：
  - 文本预处理：适用于需要高质量文本到语音转换的场景，如语音合成系统的前端处理。

---

## **5. CosyVoice2-0.5B**

- **功能**：这是 CosyVoice 的升级版本，支持 **流式语音合成** 和 **更高质量的语音生成**。
- **特点**：
  - 超低延迟：首包合成延迟可低至 150ms，适合实时应用。
  - 高准确度：在发音错误率上比 CosyVoice 1.0 下降 30%~50%。
  - 支持双向流式语音合成，适用于实时语音交互场景。
- **适用场景**：
  - 实时语音合成：适用于需要低延迟语音生成的场景，如语音聊天、实时翻译等。
  - 高质量语音生成：适合对语音质量要求较高的应用，如影视配音、广告语音等。

---

## **总结**

- **CosyVoice-300M**：适合音色克隆和跨语言语音合成。
- **CosyVoice-300M-SFT**：适合内置音色生成和多语言语音合成。
- **CosyVoice-300M-Instruct**：适合细粒度控制和情感语音生成。
- **CosyVoice-ttsfrd**：适合文本规范化处理。
- **CosyVoice2-0.5B**：适合实时语音合成和高质量语音生成。