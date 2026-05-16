# Zombin vs Plant

一款基于 C++ 和 EasyX 图形库开发的 **双人对战平台格斗游戏**，灵感源自《植物大战僵尸》。玩家可以选择豌豆射手或向日葵进行本地 1v1 对战。

## 游戏特色

- **双人对战** — 本地同屏对战，支持键位分离
- **角色差异** — 豌豆射手（远程）与向日葵（近战）拥有截然不同的战斗风格
- **平台跳跃** — 多层平台地形，包含重力、跳跃、碰撞等物理系统
- **技能系统** — 普通攻击 + 能量满后释放大招，附带攻击冷却和蓝量管理
- **视觉效果** — 粒子特效、摄像机震动、受伤闪烁、素描帧等动画表现
- **场景流转** — 主菜单 → 角色选择 → 战斗，完整的场景状态机

## 操作指南

| 操作 | 玩家 1 (1P) | 玩家 2 (2P) |
| :--- | :--- | :--- |
| 移动 | `A` / `D` | `←` / `→` |
| 跳跃 | `W` | `↑` |
| 普通攻击 | `F` | `.` |
| 大招 (需满蓝) | `G` | `/` |

## 技术栈

- **语言**：C++ (Modern C++，使用智能指针与 Lambda)
- **图形库**：EasyX (Windows 2D 图形库)
- **音频**：Windows MCI (MP3/WAV)
- **构建工具**：Visual Studio 2022 (v143 工具集)
- **目标平台**：Windows 10+

## 项目结构

```
zombin vs plant/
├── zombin vs plant.sln            # Visual Studio 解决方案
├── zombin vs plant/
│   ├── main.cpp                   # 程序入口，游戏主循环 (60 FPS)
│   ├── scene.h                    # 场景抽象基类
│   ├── scene_manager.h            # 场景管理器 (状态机)
│   ├── menu_scene.h               # 主菜单场景
│   ├── selector_scene.h           # 角色选择场景
│   ├── game_scene.h               # 战斗主场景
│   ├── player.h                   # 玩家基类 (物理/动画/碰撞/输入)
│   ├── peashooter_player.h        # 豌豆射手
│   ├── sunflower_player.h         # 向日葵
│   ├── bullet.h                   # 子弹基类
│   ├── pea_bullet.h               # 豌豆子弹
│   ├── sun_bullet.h               # 向日葵子弹
│   ├── sun_bullet_ex.h            # 向日葵大招子弹
│   ├── animation.h                # 帧动画系统
│   ├── atlas.h                    # 图集管理 (序列帧加载)
│   ├── camera.h                   # 摄像机 (含震动效果)
│   ├── particle.h                 # 粒子特效系统
│   ├── platform.h                 # 平台碰撞体
│   ├── status_bar.h               # 血条/蓝条 UI
│   ├── timer.h                    # 计时器 (回调驱动)
│   ├── vector2.h                  # 2D 向量数学
│   ├── player_id.h                # 玩家标识枚举
│   ├── util.h                     # 工具函数 (Alpha 混合/翻转/素描)
│   └── resources/                 # 资源文件 (精灵图/音效/字体)
└── 游戏架构设计教学文档.md          # 架构设计文档
```

## 构建与运行

### 前置要求

1. **Visual Studio 2022** — 需安装"使用 C++ 的桌面开发"工作负载
2. **EasyX** — 从 [easyx.cn](https://easyx.cn) 下载并安装对应 VS2022 版本

### 编译步骤

1. 克隆仓库并打开 `zombin vs plant.sln`
2. 选择配置 (Debug/Release) 和平台 (x64)
3. 生成 → 生成解决方案
4. 运行生成的 `zombin vs plant.exe`

## 架构概览

```
main.cpp (游戏循环: 输入 → 更新 → 渲染)
  └─ SceneManager (场景状态机)
       ├─ MenuScene     (主菜单)
       ├─ SelectorScene (角色选择)
       └─ GameScene     (战斗)
            ├─ Player × 2      (角色实体)
            ├─ Bullet List     (子弹系统)
            ├─ Platform List   (地形碰撞)
            └─ StatusBar × 2   (UI 状态栏)
```

核心设计模式：**场景模式**管理游戏状态流转，**状态模式**驱动角色动画切换，**观察者模式** (Timer 回调) 处理延时事件，**工厂模式**根据选择创建角色实例。

## 致谢

- 灵感来源：PopCap Games《植物大战僵尸》
- 图形引擎：[EasyX](https://easyx.cn)
