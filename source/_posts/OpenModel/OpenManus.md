---
title: OpenManus本地部署
date: 2025-03-12 15:24:31
categories:
    - [Agent, manus]
tags: [Agent, manus]
description: OpenManus 本地部署
keywords: Manus, OpenManus
---

# OpenManus 安装与配置指南

## 一、前置软件下载

  1. **Miniconda 下载与安装** ：打开浏览器，访问链接<https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe> 进行下载。下载完成后，双击文件开始安装，推荐安装在除 C 盘以外的位置。安装步骤依次为：双击下载的文件，点击 “Next”->“I Agree”->“Next”->“Next”，注意勾选第二项，然后点击 “Install”，
  ![miniconda3](images/OpenManus/miniconda3.png)
  安装完成后继续点击 “Next”，若弹出网页可直接关闭。
  2. **验证 Miniconda 安装** ：回到桌面，按 “Win+R”，输入 “cmd” 回车，在弹出的界面中输入以下命令，若看到类似 “conda 25.1.1” 的输出则表示安装成功。

```bash
conda --version
```

## 二、OpenManus 文件获取与环境搭建

  1. **获取 OpenManus 文件** ：打开链接<https://github.com/mannaandpoem/OpenManus/archive/refs/heads/front-end.zip> 下载，下载完后解压，解压后打开文件，此时界面应为项目文件所在目录。
  2. **创建 Conda 环境** ：在项目文件所在目录空白处右键，选择 “Git Bash Here”（或类似方式打开命令行工具），输入以下命令，若出现提示信息，输入 “y” 加回车即可。

```bash
conda create -n open_manus python=3.12
```

  3. **激活 Conda 环境** ：继续输入以下命令。

```bash
conda activate open_manus
```

  4. **安装依赖项** ：输入以下命令，此过程可能需要较长时间，请耐心等待。

```bash
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/
```

## 三、API 配置

  1. **获取 API 密钥** ：打开链接<https://cloud.siliconflow.cn/i/y19ZZCMm> 登录，点击右上角 “新建 API 密钥”，复制生成的 API 密钥。
  2. **配置项目 config 文件** ：找到项目中的 config 文件，将其重命名为 “config.toml”，右键选择用记事本打开，将刚刚复制的 API 密钥、模型和路径粘贴到对应的位置，其中路径为 “<https://api.siliconflow.cn/v1>”，模型为 “Qwen/Qwen2.5-7B-Instruct”。
  ![config](images/OpenManus/config.png)
  3. **保存并运行项目** ：按 “Ctrl+S” 保存文件并关闭，回到命令行窗口，输入以下命令，即可开始使用 OpenManus。

```bash
python app.py
```