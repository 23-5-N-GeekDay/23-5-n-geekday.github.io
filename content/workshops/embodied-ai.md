---
title: 具身智能入门工作坊
description: 学习如何使用RDK X5和RoboBrain-X构建具身智能机器人
date: 2026-02-15
---

# 具身智能入门工作坊

本工作坊将带你快速入门具身智能开发，了解如何构建感知—决策—执行闭环的机器人系统。

## 🤖 什么是具身智能？

具身智能（Embodied AI）是一种具备物理交互能力的AI系统，能够：

- 👁️ **感知**：通过传感器获取环境信息
- 🧠 **决策**：分析信息并做出判断
- 🦾 **执行**：控制机械装置完成任务

### 与传统机器人的区别

| 特性 | 传统机器人 | 具身智能 |
|------|------------|----------|
| 编程方式 | 预设程序 | 自主学习 |
| 适应能力 | 固定场景 | 泛化迁移 |
| 交互方式 | 精确指令 | 自然语言 |
| 任务能力 | 重复作业 | 复杂推理 |

## 🔧 硬件环境

### 地瓜机器人 RDK X5

RDK X5 是千元内最佳机器人开发平台：

- **旭日5智能计算芯片**
- **10 TOPs 算力**
- **100+ 机器人配/套件**
- 只需一根Type-C线即可开始

```bash
# 连接RDK X5
ssh root@192.168.1.xxx

# 查看系统信息
cat /etc/sunrise_release
```

### 智源 RoboBrain-X

通用VLA（Vision-Language-Action）模型：

- 视觉理解能力
- 语言指令解析
- 动作轨迹生成
- 跨机器人泛化

## 🎯 实战案例：物品抓取机器人

### 1. 环境感知

```python
import cv2
from rdk import Camera, ObjectDetector

# 初始化摄像头
camera = Camera(device_id=0)

# 加载目标检测模型
detector = ObjectDetector(model="yolov8n")

def detect_objects():
    """检测场景中的物品"""
    frame = camera.capture()
    results = detector.detect(frame)
    
    objects = []
    for r in results:
        objects.append({
            "class": r.class_name,
            "confidence": r.confidence,
            "bbox": r.bbox,
            "center": r.center
        })
    
    return objects
```

### 2. 语言指令解析

```python
from robobrain import RoboBrainX

# 初始化RoboBrain-X
brain = RoboBrainX(model_path="/models/robobrain-x")

def parse_instruction(text, objects):
    """理解自然语言指令"""
    
    # 构建场景描述
    scene = brain.describe_scene(objects)
    
    # 解析指令意图
    intent = brain.parse_instruction(text, scene)
    
    return {
        "action": intent.action,  # 如 "pick", "place"
        "target": intent.target,  # 如 "red cup"
        "destination": intent.destination
    }
```

### 3. 动作规划与执行

```python
from rdk import RobotArm
from robobrain import ActionPlanner

# 初始化机械臂
arm = RobotArm(serial_port="/dev/ttyUSB0")

# 初始化动作规划器
planner = ActionPlanner(arm_config=arm.config)

def execute_action(intent, objects):
    """规划并执行动作"""
    
    # 找到目标物品
    target_obj = find_object(objects, intent["target"])
    
    if target_obj is None:
        return {"success": False, "error": "目标物品未找到"}
    
    # 规划抓取轨迹
    trajectory = planner.plan_grasp(
        current_pos=arm.get_position(),
        target_pos=target_obj["center"],
        obstacle_map=objects
    )
    
    # 执行动作序列
    for waypoint in trajectory:
        arm.move_to(waypoint)
        
    # 抓取
    arm.gripper_close()
    
    return {"success": True}
```

### 4. 完整闭环

```python
class PickAndPlaceRobot:
    def __init__(self):
        self.camera = Camera()
        self.detector = ObjectDetector()
        self.brain = RoboBrainX()
        self.arm = RobotArm()
        
    def run(self, instruction):
        """执行一次完整的抓取任务"""
        
        # 1. 感知
        objects = self.detect_objects()
        
        # 2. 理解指令
        intent = self.brain.parse_instruction(
            instruction, 
            objects
        )
        
        # 3. 规划动作
        trajectory = self.plan_action(intent, objects)
        
        # 4. 执行
        result = self.execute(trajectory)
        
        # 5. 反馈
        return self.brain.generate_response(result)

# 使用示例
robot = PickAndPlaceRobot()
response = robot.run("请把红色的杯子放到桌子左边")
print(response)  # "好的，我已经把红色杯子放到桌子左边了"
```

## 🛠️ 快速原型搭建

### 使用 CyberBrick 搭建结构

```
推荐配置：
├── 底座模块 x1
├── 旋转关节 x2
├── 直线滑轨 x1
└── 夹爪模块 x1
```

### 3D 打印定制零件

```bash
# 推荐打印参数
- 材质: PLA
- 层高: 0.2mm
- 填充: 20%
- 打印时间: 30分钟-2小时（小尺寸零件）
```

## 📊 调试技巧

### 可视化调试

```python
import matplotlib.pyplot as plt

def visualize_trajectory(trajectory):
    """可视化机械臂轨迹"""
    xs = [p.x for p in trajectory]
    ys = [p.y for p in trajectory]
    zs = [p.z for p in trajectory]
    
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    ax.plot(xs, ys, zs, marker='o')
    plt.show()
```

### 安全约束

```python
# 设置工作空间边界
arm.set_workspace_limits(
    x_range=(-0.5, 0.5),
    y_range=(-0.5, 0.5),
    z_range=(0.1, 0.6)
)

# 设置最大速度
arm.set_max_velocity(0.3)  # m/s

# 启用碰撞检测
arm.enable_collision_detection(True)
```

## 🎓 总结

通过本次工作坊，你学习了：

- ✅ 具身智能的核心概念
- ✅ RDK X5 开发环境配置
- ✅ RoboBrain-X 模型使用
- ✅ 感知—决策—执行闭环实现
- ✅ 快速原型搭建技巧

### 下一步学习

- 深入了解力控与柔顺控制
- 学习多模态传感器融合
- 探索 Sim2Real 迁移技术

<br>

---

<br>

**用48小时，让你的机器人从概念走向现实！**
