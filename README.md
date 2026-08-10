# VEILINK 六轴机械臂入门指南

VEILINK 是一款以 3D 打印结构件和飞特 STS3215 总线舵机为基础的六轴机械臂。本仓库整理了制作和调试这款机械臂所需的 3D 模型、装配说明、运动学参数及快速入门程序。

## 关于这个项目

我正在学习机械臂相关知识，VEILINK 是我设计的第一款机械臂，因此在设计和制作过程中，可能仍有一些不够完善的地方。希望这个项目能为同样对机械臂、机器人控制和 3D 打印感兴趣的朋友提供一些灵感，也欢迎大家一起学习、交流和探讨。

项目展示及打印配置可参考：[MakerWorld：VEILINK 六轴机械臂](https://makerworld.com.cn/zh/models/2817152-veilink-liu-zhou-ji-jie-bi#profileId-3285755)。

## 仓库内容

| 目录 | 内容 |
| --- | --- |
| [3D Models](<3D Models/>) | 3MF 打印文件、STEP 三维模型和装配说明书 |
| [Code](<Code/>) | 舵机编号、整机校准、单舵机控制和 Xbox 手柄控制 Notebook |
| [运动学参数 Kinematic Parameters](<运动学参数 Kinematic Parameters/>) | 运动学参数说明、DH 参数表、运动学模型和零位示意图 |

主要资料：

- [3D 打印文件](<3D Models/VEILINK Arm 3D Print.3mf>)
- [STEP 三维模型](<3D Models/VEILINK Arm.STEP>)
- [装配说明书](<3D Models/装配说明书 Assembly Instruction.pdf>)
- [DH 参数表](<运动学参数 Kinematic Parameters/DH参数表 DH Parameters.png>)
- [运动学模型](<运动学参数 Kinematic Parameters/运动学模型 Kinematic Model.png>)
- [机械臂零位示意图](<运动学参数 Kinematic Parameters/零位 Zero Pose.PNG>)
- [运动学参数说明](<运动学参数 Kinematic Parameters/运动学参数说明.md>)

## 软件环境

快速入门 Notebook 按 LeRobot 0.5.x API 编写，当前校准记录使用 LeRobot 0.5.1。推荐使用 Anaconda 创建独立的 Python 环境，便于安装依赖、切换环境及避免不同项目之间的版本冲突。

### 1. 安装 Anaconda

前往 [Anaconda 官网](https://www.anaconda.com/download)下载并安装适合当前操作系统的版本。Windows 用户安装完成后，可从开始菜单打开 **Anaconda Prompt** 执行下面的命令。

先确认 Conda 已正确安装：

```powershell
conda --version
```

如果普通 PowerShell 提示找不到 `conda`，可改用 Anaconda Prompt，或在安装完成后重新打开终端。

### 2. 创建并激活项目环境

创建名为 `lerobot`、使用 Python 3.10 的独立环境：

```powershell
conda create -n lerobot python=3.10 -y
conda activate lerobot
```

激活成功后，终端提示符前通常会显示 `(lerobot)`。可以运行以下命令确认当前 Python 来自新环境：

```powershell
python --version
where.exe python
```

### 3. 安装项目依赖

在 `lerobot` 环境中执行：

```powershell
python -m pip install --upgrade pip
python -m pip install "lerobot==0.5.1" pyserial ipykernel ipywidgets jupyter numpy pygame
```

随后将该环境注册为 Jupyter 内核：

```powershell
python -m ipykernel install --user --name lerobot --display-name "Python (lerobot)"
```

### 4. 打开 Notebook

使用 VS Code 或 Jupyter 打开仓库中的 Notebook，并在右上角选择 `Python (lerobot)` 内核。串口号、夹爪选项及其他需要修改的参数，均在对应 Notebook 中标有说明。

以后再次使用时，只需打开 Anaconda Prompt 并激活环境：

```powershell
conda activate lerobot
```

常用的环境管理命令：

```powershell
conda env list                  # 查看已有环境
conda deactivate                # 退出当前环境
conda remove -n lerobot --all   # 删除 lerobot 环境
```

更多用法可参考 [Conda 环境管理文档](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)。

## 推荐使用顺序

### 1. 准备打印件和硬件

使用仓库中的 [3MF 文件](<3D Models/VEILINK Arm 3D Print.3mf>)打印结构件，并根据[装配说明书](<3D Models/装配说明书 Assembly Instruction.pdf>)准备舵机、控制板、电源、紧固件及工具。

### 2. 设置舵机编号

在机械装配前，打开 [1-舵机编号标记.ipynb](<Code/1-舵机编号标记.ipynb>)，按照其中的说明逐台设置舵机 ID、进行通信验证并粘贴实体标签。

### 3. 完成机械装配

按照[装配说明书](<3D Models/装配说明书 Assembly Instruction.pdf>)给出的方向和顺序安装舵机、舵盘、打印件及控制板。装配前后都应核对舵机标签与关节位置。

### 4. 校准零位和安全限位

机械装配完成后，参考[零位示意图](<运动学参数 Kinematic Parameters/零位 Zero Pose.PNG>)，运行 [2-舵机校准.ipynb](<Code/2-舵机校准.ipynb>)，记录各关节零位并采集安全限位。

校准结果会保存到 [机械臂校准参数.json](<Code/机械臂校准参数.json>)。仓库中的参数来自一台实际机械臂，仅用于展示数据结构和提供参考；不同设备的安装误差和安全范围并不相同，请务必为自己的机械臂重新校准。

### 5. 进行单关节调试

校准完成后，运行 [3-单舵机控制.ipynb](<Code/3-单舵机控制.ipynb>)，按照 Notebook 中的安全步骤逐个检查关节的位置反馈、运动方向、零位和限位，再逐步开展后续控制实验。

### 6. 使用 Xbox 手柄控制

完成校准和逐关节验证后，可以运行 [手柄操控机械臂.ipynb](<Code/手柄操控机械臂.ipynb>)，使用 Xbox 手柄控制腕部中心移动、腕部关节、末端旋转和夹爪开合。该 Notebook 使用 Pygame 的 SDL 标准手柄映射，并从同一份校准 JSON 读取 DH 参数、零位和软件限位。

首次使用时，请先完成 Notebook 中的手柄输入预览和离线回归测试，再以最低速度档进行短脉冲实机验证。控制过程中，短按 A 可在控制与保持状态之间切换，长按 A 退出循环并保持位置，B 键用于锁存紧急卸力；软件卸力不能替代实体断电或急停措施。

### 7. 使用运动学资料

需要进行正逆运动学、轨迹规划或上层控制开发时，建议先阅读[运动学参数说明](<运动学参数 Kinematic Parameters/运动学参数说明.md>)，再结合 [DH 参数表](<运动学参数 Kinematic Parameters/DH参数表 DH Parameters.png>)、[运动学模型](<运动学参数 Kinematic Parameters/运动学模型 Kinematic Model.png>)和零位示意图进行验证。

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
验证 Xbox 手柄输入与安全停止逻辑
        ↓
开展运动学与上层控制开发
```
