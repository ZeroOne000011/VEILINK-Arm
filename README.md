# VEILINK 六轴机械臂入门指南

VEILINK 是一款以 3D 打印结构件和飞特 STS3215 总线舵机为基础的六轴机械臂。本仓库整理了制作和调试这款机械臂所需的 3D 模型、装配说明、运动学参数及快速入门程序。

## 关于这个项目

我正在学习机械臂相关知识，VEILINK 是我设计的第一款机械臂，因此在设计和制作过程中，可能仍有一些不够完善的地方。

希望这个项目能为同样对机械臂、机器人控制和 3D 打印感兴趣的朋友提供一些灵感，也欢迎大家一起学习、交流和探讨。

项目展示及打印配置可参考：[MakerWorld：VEILINK 六轴机械臂](https://makerworld.com.cn/zh/models/2817152-veilink-liu-zhou-ji-jie-bi#profileId-3285755)。

## 仓库内容

| 目录 | 内容 |
| --- | --- |
| [3D Models](<3D Models/>) | 3MF 打印文件、STEP 三维模型和装配说明书 |
| [快速入门指南](<快速入门指南/>) | 舵机编号、整机校准和单舵机控制程序 |
| [运动学参数 Kinematic Parameters](<运动学参数 Kinematic Parameters/>) | DH 参数表、运动学模型和零位示意图 |

主要资料：

- [3D 打印文件](<3D Models/VEILINK Arm 3D Print.3mf>)
- [STEP 三维模型](<3D Models/VEILINK Arm.STEP>)
- [装配说明书](<3D Models/装配说明书 Assembly Instruction.pdf>)
- [DH 参数表](<运动学参数 Kinematic Parameters/DH参数表 DH Parameters.xlsx>)
- [运动学模型](<运动学参数 Kinematic Parameters/运动学模型 Kinematic Model.pdf>)
- [机械臂零位示意图](<运动学参数 Kinematic Parameters/零位 Zero Pose.PNG>)

## 软件环境

快速入门 Notebook 按 LeRobot 0.5.x API 编写，当前校准记录使用 LeRobot 0.5.1。建议为项目创建独立的 Python 环境，避免不同版本之间产生兼容问题。

以下示例适用于 Windows PowerShell，并以 Python 3.10 为例：

```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install "lerobot==0.5.1" pyserial ipykernel ipywidgets jupyter
python -m ipykernel install --user --name lerobot --display-name "Python (lerobot)"
```

安装完成后，使用 VS Code 或 Jupyter 打开 Notebook，并选择 `Python (lerobot)` 内核。串口号、夹爪选项及其他需要修改的参数，均在对应 Notebook 中标有说明。

## 推荐使用顺序

### 1. 准备打印件和硬件

使用仓库中的 [3MF 文件](<3D Models/VEILINK Arm 3D Print.3mf>)打印结构件，并根据[装配说明书](<3D Models/装配说明书 Assembly Instruction.pdf>)准备舵机、控制板、电源、紧固件及工具。

### 2. 设置舵机编号

在机械装配前，打开 [1-舵机编号标记.ipynb](<快速入门指南/1-舵机编号标记.ipynb>)，按照其中的说明逐台设置舵机 ID、进行通信验证并粘贴实体标签。

### 3. 完成机械装配

按照[装配说明书](<3D Models/装配说明书 Assembly Instruction.pdf>)给出的方向和顺序安装舵机、舵盘、打印件及控制板。装配前后都应核对舵机标签与关节位置。

### 4. 校准零位和安全限位

机械装配完成后，参考[零位示意图](<运动学参数 Kinematic Parameters/零位 Zero Pose.PNG>)，运行 [2-舵机校准.ipynb](<快速入门指南/2-舵机校准.ipynb>)，记录各关节零位并采集安全限位。

校准结果会保存到 [机械臂校准参数.json](<快速入门指南/机械臂校准参数.json>)。仓库中的参数来自一台实际机械臂，仅用于展示数据结构和提供参考；不同设备的安装误差和安全范围并不相同，请务必为自己的机械臂重新校准。

### 5. 进行单关节调试

校准完成后，运行 [3-单舵机控制.ipynb](<快速入门指南/3-单舵机控制.ipynb>)，按照 Notebook 中的安全步骤逐个检查关节的位置反馈、运动方向、零位和限位，再逐步开展后续控制实验。

### 6. 使用运动学资料

需要进行正逆运动学、轨迹规划或上层控制开发时，可参考 [DH 参数表](<运动学参数 Kinematic Parameters/DH参数表 DH Parameters.xlsx>)和[运动学模型](<运动学参数 Kinematic Parameters/运动学模型 Kinematic Model.pdf>)。

## 安全提示

- 开始装配或调试前，请完整阅读装配说明书和对应 Notebook 中的安全要求。
- 通电前核对舵机额定电压、接线极性、外部电源容量及控制板与电源共地情况。
- 调试承重关节时应可靠固定机械臂并托住连杆，避免卸力后突然下坠。
- 首次运动测试应采用低速、小幅度、逐关节的方式进行。
- 出现异常运动、发热、异味、异响、通信错误或结构干涉时，应立即卸力并切断舵机电源。

```text
准备打印件和硬件
        ↓
设置舵机 ID 并粘贴标签
        ↓
完成机械装配与接线
        ↓
校准零位和安全限位
        ↓
逐关节进行低速、小幅度测试
        ↓
开展运动学与上层控制开发
```
