---
title: Embodied AI 赛道 · 赞助商资源启用指南
description: Embodied AI 具身智能赛道赞助商提供的硬件设备与平台资源启用方式
date: 2026-02-18
---

# Embodied AI 赛道 · 赞助商资源启用指南

> 具身智能赛道的资源以**现场硬件设备**为主，请在 Kick Off 当天与组委会确认设备分配方案。部分软件平台资源可提前申请。

### ⚠️ 会前重要准备工作

由于本赛道涉及较多硬件控制与ROS开发，请所有参赛选手**务必在赛前完成以下准备**：

1. **开发环境准备：安装 Ubuntu 系统**
   - 强烈建议选手提前在个人电脑上安装 **Ubuntu 20.04 或 22.04** 操作系统（双系统或高配置虚拟机均可）。
   - 绝大多数开源机器人项目（如 ROS/ROS2, LeRobot 等）和硬件驱动在 Linux 环境下拥有最好的兼容性。**Windows/macOS 可能会在编译底盘驱动、调用摄像头、配置网络等环节遇到难以预料的阻碍，极大影响比赛效率。**

2. **3D打印切片软件：安装 Bambu Studio**
   - 如您的项目计划在现场使用拓竹 3D 打印机打印结构件，务必提前下载并安装官方切片软件：**[Bambu Studio](https://bambulab.cn/download)**。
   - 现场打印机时间宝贵，请在个人电脑上完成切片并生成可打印文件后再前往设备端连接。

<br>

---
<br>

## 🧠 软件与模型资源

### 北京智源研究院 · RoboBrain-X

**提供内容**：最新发布的通用 VLA（Vision-Language-Action）模型，实现从感知到执行的一体化能力

#### 资源说明

RoboBrain-X 通过统一建模视觉、语言与动作，解决跨机器人本体的泛化适配问题，支持：
- 自然语言指令 → 机器人动作序列
- 多模态感知（RGB + 深度 + 触觉）
- 跨机器人平台适配（机械臂、移动机器人等）

> 📖 **官方 GitHub 与完整文档**：[BAAI-BAAI/RoboBrain-X](https://github.com/BAAI-BAAI/RoboBrain-X)
> 💡 **提示**：RoboBrain-X 支持零样本泛化，无需针对特定任务重新训练，直接用自然语言描述任务即可。

<br>

---
<br>

### 🤖 机器人仿真环境

**提供内容**：极客节官方提供的一套用于机器人学习和强化学习的仿真环境一键部署脚本，包含 **MuJoCo** 和 **Isaac Lab** 两个物理仿真平台。

#### 资源说明

如果在开发中实体机台不足，利用仿真环境可以帮助您在纯软件层面上提前开展算法训练和策略验证，避免真机调试时发生意外损耗。

- **MuJoCo**（轻量级）：适合快速算法验证和强化学习研究。容器化封装，起步速度快，资源消耗低。
- **Isaac Lab**（重量级）：NVIDIA 官方机器人高保真物理和传感器仿真平台。适合需要复杂视觉和力控计算的场景，强烈推荐配合配备 NVIDIA GPU 算力的云服务器（如丹摩智算平台）使用。

> 📖 **一键部署包与开源代码仓库**：[GeekDay2026 / robotics-simulation](https://github.com/23-5-N-GeekDay/GeekDay2026/tree/main/robotics-simulation)

<br>

---
<br>

## ⚡ 丹摩智算平台 · GPU 云实例

**提供内容**：提供高性能 GPU 云实例及存储服务，满足机器人模型训练、视觉与强化学习需求。
**现场配置**：组委会将直接为队伍分配已启动的高端算力实例（预配 NVIDIA A800 等高性能显卡及基础运行环境）。**您无需自行注册发配。**

### 连接与使用实例（推荐三种方式）

- **内置 JupyterLab（新手推荐）**：
  现场组委会将下发每个实例独有的网页访问链接。您可直接在浏览器打开进入网页版 JupyterLab，进行代码编写与文件管理。
  - `/` 系统盘：还原系统时会被清空
  - `/root/workspace` 数据盘：支持扩容，持久化保存
  - `/root/shared-storage` 共享存储盘：平台提供 20GB 免费存储空间，挂载于此处并跨实例共享。
- **本地 SSH 连接**：
  组委会在分配实例时，将一并下发 SSH 登录指令（如 `ssh -p 端口号 root@主机地址`）和对应的私钥文件（`.pem`）。在终端执行：
  ```bash
  ssh -i <下发的密钥文件.pem> -p <端口号> root@<主机地址>
  ```
- **MobaXterm 连接（Windows 推荐）**：
  新建 SSH Session，填入主机地址与端口，在高级设置中勾选「Use private key」导入下发的 `.pem` 私钥即可连接。

> 📖 **完整官方教程请参考**： [DAMODEL 新手指引](https://doc.damodel.com/profile/biginner-guide.html)

<br>

---
<br>

## 🤖 硬件资源

> 以下硬件设备将在**现场统一提供**，请在 Kick Off 当天与组委会确认分配。

### 地瓜机器人 · RDK X5

**提供内容**：机器人开发者套件，搭载旭日5智能计算芯片，10 TOPs 算力

#### 设备规格

| 参数 | 规格 |
|------|------|
| 处理器 | 旭日5（BPU 10 TOPs） |
| 内存 | 4GB LPDDR4 |
| 存储 | 32GB eMMC |
| 接口 | USB 3.0 × 4, MIPI CSI × 2, 40-pin GPIO |
| 操作系统 | Ubuntu 22.04（现场已预装，可即插即用） |

> 📖 **官方开发者文档**：[地瓜机器人开发者中心](https://developer.d-robotics.cc/)

<br>

---
<br>

### SeeedStudio（矽递科技）

**提供内容**：开源机械臂（Lerobot Arm）+ 边缘计算模块 + 传感器套件

#### 可用设备清单

- **Lerobot Arm**：6自由度开源机械臂，支持 ROS2
- **传感器套件**：深度相机（RealSense D435）、力传感器、IMU

涵盖了搭建机械臂抓取任务的基础套件全集。

> 📖 **LeRobot 框架文档**：[huggingface/lerobot](https://github.com/huggingface/lerobot)
> 📖 **RealSense 开发者指南**：[Intel RealSense SDK](https://github.com/IntelRealSense/librealsense)

<br>

---
<br>

### 拓竹 · CyberBrick

**提供内容**：模块化编程机器人（支持低代码拖拽编程）+ 3D 打印机（PLA 材质）

#### 资源与使用说明

**1. CyberBrick 模块化机器人**
- 支持通过 CyberBrick Studio 低代码拖拽编程与 Python API。
- 适合快速搭建功能组件、控制电机与读取传感器状态。
- [点击下载 CyberBrick Studio](https://bambulab.cn/cyberbrick)

**2. 3D 打印机**
- **设备**：拓竹 X1 Carbon（现场提供，并备有 PLA 材质）
- **切片软件**：请参赛者在自己的笔记本上提前下载安装 **[Bambu Studio](https://bambulab.cn/download)**，完成模型切片后再到现场设备上操作打印。
- **说明**：现场预约资源，随到随用，请提前备好需要切片的 `.stl` 或 `.obj` 3D模型文件。

<br>

---
<br>

## 🛠️ 全赛道通用资源

### TRAE · TraePro 会员

**提供内容**：每位选手免费获得 **4天 TraePro 会员**

#### 启用步骤

1. **下载 TRAE**
   - 访问 [trae.ai](https://trae.ai) 下载客户端
   - 支持 macOS / Windows / Linux

2. **激活极客节专属会员**
   - 打开 TRAE → 点击右上角头像 → 「会员中心」
   - 输入组委会提供的**激活码**
1.  **下载 TRAE**
    -   访问 [trae.ai](https://trae.ai) 下载客户端
    -   支持 macOS / Windows / Linux

2.  **激活极客节专属会员**
    -   打开 TRAE → 点击右上角头像 → 「会员中心」
    -   输入组委会提供的**激活码**

3.  **具身 AI 开发场景推荐用法**
    -   用 AI 补全生成机器人控制代码
    -   接入 ROS2 文档作为知识库，快速查询 API
    -   用 Agent 模式自动调试传感器数据处理流程

<br>

---

<br>

**遇到问题？** 联系组委会：cysybeijing@163.com
