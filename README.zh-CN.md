# ADA：Agent Driven Avatar - 智能体驱动的虚拟形象（数字人）

<div align="center">

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](https://github.com/kuibu/ADA/releases)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/kuibu/ADA/pulls)
[![GitHub Stars](https://img.shields.io/github/stars/kuibu/ADA?style=social)](https://github.com/kuibu/ADA/stargazers)

**中文 | [English](./README.md)**

</div>

> **让智能代理拥有生动的虚拟形象（数字人），实现更自然的人机交互**

## 概述

生成式人工智能在生成文本、图像和代码方面表现出色。现在，是时候让它用于生成动态的、具有情感表达能力的虚拟形象了。今天，我们正式发布 **ADA (Agent Driven Avatar)** 项目，旨在为智能代理提供统一的虚拟形象表示和交互标准。

ADA 使智能代理能够：
- 🎭 **动态生成虚拟形象** - 根据对话上下文和用户偏好生成适合的虚拟形象
- 🎬 **表达情感和意图** - 通过面部表情、肢体动作和语音同步传达信息
- 🔄 **跨平台一致性** - 在 Web、移动端、VR/AR 等不同平台上保持一致的体验
- 🔒 **安全可控** - 声明式规范确保虚拟形象生成的安全性和可控性
- 🤝 **多代理协作** - 支持多个代理在同一场景中以虚拟形象形式交互

## 问题：代理需要更人性化的存在感

想象一下，您正在与一个 AI 助手交谈，讨论健康建议：

**纯文本交互：**
```
用户："我最近感觉很疲劳，有什么建议吗？"
代理："建议：1. 保证充足睡眠 2. 均衡饮食 3. 适量运动"
```

**使用 ADA 的交互：**

除了文本响应，代理还能：
- 以关切的面部表情出现
- 用手势强调重点建议
- 根据话题严肃程度调整语气和表情
- 在需要时展示演示动作（如运动姿势）

这种多模态的交互方式能够：
- 提升用户的参与度和信任感
- 传达更丰富的情感和意图
- 使复杂指导更易理解
- 创造更自然的对话体验

## 挑战：跨平台的虚拟形象标准化

在多代理和多平台环境中，虚拟形象面临以下挑战：

### 1. **平台碎片化**
- Web 使用 WebGL/Three.js
- 移动端使用 Unity/Unreal
- VR/AR 有各自的渲染引擎
- 每个平台都需要定制开发

### 2. **安全性问题**
- 如何防止恶意代理生成不当内容？
- 如何确保虚拟形象符合品牌规范？
- 如何在不信任的代理之间共享虚拟形象？

### 3. **性能和带宽**
- 3D 模型和动画数据量大
- 实时渲染要求高
- 移动设备性能受限

### 4. **多代理场景**
- 多个代理在同一场景中交互
- 虚拟形象需要区分和协调
- 表情和动作需要同步

## 解决方案：声明式虚拟形象规范

ADA 提供了一种**声明式、跨平台的虚拟形象描述格式**，类似于 A2UI 对用户界面的处理方式：

### 核心架构

```
┌─────────────────────────────────────────────────┐
│             智能代理 (AI Agent)                   │
│  ┌──────────────────────────────────────────┐   │
│  │  决策引擎: 确定表情、动作、语音           │   │
│  └──────────────┬───────────────────────────┘   │
│                 │                                │
│                 ▼                                │
│  ┌──────────────────────────────────────────┐   │
│  │  ADA 生成器: 输出 JSON 格式的虚拟形象描述 │   │
│  └──────────────┬───────────────────────────┘   │
└─────────────────┼───────────────────────────────┘
                  │ ADA Message (JSON)
                  │
        ┌─────────┼─────────┐
        │         │         │
        ▼         ▼         ▼
    ┌─────┐  ┌─────┐  ┌─────────┐
    │ Web │  │移动端│  │ VR/AR   │
    └─────┘  └─────┘  └─────────┘
        │         │         │
        └─────────┼─────────┘
                  ▼
        客户端 ADA 渲染器
    (使用本地组件和资源渲染)
```

### ADA 消息体系

ADA 采用**分类消息**架构，支持高效的增量更新和精确的时序控制。

#### 消息类型

```typescript
enum MessageType {
  INIT = 'avatar.init',              // 初始化虚拟形象
  UPDATE = 'avatar.update',          // 增量更新状态
  ACTION = 'avatar.action',          // 执行动作序列
  SPEECH = 'avatar.speech',          // 语音+口型同步
  INTERACTION = 'avatar.interaction', // 用户交互响应
  SCENE = 'scene.update',            // 多代理场景管理
  RESOURCE = 'resource.preload',     // 资源预加载
  EVENT = 'avatar.event'             // 事件通知
}
```

#### 示例 1: 初始化消息

完整的虚拟形象初始化，包含外观、能力和初始状态。

```json
{
  "version": "1.0.0",
  "type": "avatar.init",
  "timestamp": 1738329600000,
  "agent_id": "health-advisor-001",
  "session_id": "session-abc-123",

  "avatar": {
    "model": {
      "asset_id": "ada://models/humanoid/professional-v2",
      "fallback": ["ada://models/humanoid/basic"],
      "lod_levels": ["high", "medium", "low"]
    },
    "appearance": {
      "preset": "medical-professional",
      "overrides": {
        "clothing": {
          "upper": "white-coat",
          "accessories": ["stethoscope"]
        }
      }
    }
  },

  "initial_state": {
    "pose": {"posture": "standing-relaxed"},
    "expression": {
      "preset": "neutral-friendly",
      "blend": {"smile": 0.3}
    },
    "gaze": {"target": "camera"}
  }
}
```

#### 示例 2: 增量更新消息

仅传输变化的状态，优化带宽（类似 A2UI 的增量更新）。

```json
{
  "version": "1.0.0",
  "type": "avatar.update",
  "timestamp": 1738329605000,
  "agent_id": "health-advisor-001",
  "priority": "high",

  "updates": [
    {
      "path": "expression.preset",
      "value": "empathetic",
      "transition": {
        "duration_ms": 800,
        "easing": "ease-in-out"
      }
    },
    {
      "path": "gaze.target",
      "value": "user",
      "transition": {"duration_ms": 400}
    }
  ]
}
```

#### 示例 3: 动作执行消息

编排复杂的动画序列，与 UI 元素精确同步。

```json
{
  "version": "1.0.0",
  "type": "avatar.action",
  "timestamp": 1738329610000,
  "agent_id": "health-advisor-001",

  "action": {
    "id": "explain-sleep-tips",
    "sequence": [
      {
        "step_id": "nod",
        "action_ref": "ada://animations/gestures/nod",
        "parameters": {"intensity": 0.7},
        "start_offset_ms": 0,
        "duration_ms": 600
      },
      {
        "step_id": "point-to-ui",
        "action_ref": "ada://animations/gestures/point",
        "parameters": {
          "hand": "right",
          "target_ui": "a2ui://element/sleep-tip-card"
        },
        "start_offset_ms": 700,
        "duration_ms": 1200
      }
    ],
    "sync_markers": [
      {
        "marker_id": "speech-start",
        "align_to_step": "nod",
        "offset_ms": 0
      }
    ]
  }
}
```

#### 示例 4: 语音同步消息

与语音精确同步的表情和口型动画。

```json
{
  "version": "1.0.0",
  "type": "avatar.speech",
  "timestamp": 1738329612000,
  "agent_id": "health-advisor-001",

  "speech": {
    "utterance_id": "utterance-001",
    "text": "保证充足的睡眠对恢复精力非常关键",

    "audio": {
      "source": "https://cdn.example.com/audio/utterance-001.webm",
      "format": "webm",
      "duration_ms": 3500
    },

    "phonemes": [
      {"phoneme": "b", "start_ms": 0, "end_ms": 80},
      {"phoneme": "ao", "start_ms": 80, "end_ms": 200}
      // ... 自动生成口型同步
    ],

    "prosody": {
      "emotion": "caring",
      "emphasis_words": [0, 4, 8],
      "speed_multiplier": 0.95
    },

    "facial_animation": {
      "lip_sync": "auto",
      "expression_overlay": {
        "smile": {"value": 0.5, "hold_duration_pct": 80}
      }
    }
  }
}
```

## 核心理念

### 1. 🔒 安全至上

- **声明式规范**：代理发送数据而非可执行代码
- **预批准资源库**：客户端维护可信的模型和动画库
- **内容过滤**：自动检测和阻止不当内容
- **品牌控制**：客户端完全控制虚拟形象的视觉风格

### 2. 🎯 上下文感知

- **情感智能**：根据对话情感调整表情
- **任务适配**：根据任务类型选择合适的虚拟形象风格
- **文化敏感**：支持本地化的表情和手势
- **可访问性**：支持字幕、手语等辅助功能

### 3. 🌐 跨平台一致性

- **框架无关**：支持 Three.js, Unity, Unreal, ARKit, ARCore
- **响应式**：从低端移动设备到高端 VR 头显
- **渐进增强**：根据设备能力提供不同质量级别
- **离线支持**：缓存常用资源实现离线渲染

### 4. ⚡ 高性能

- **增量更新**：仅传输变化的状态
- **优先级系统**：关键动作优先渲染
- **LOD 支持**：根据距离和重要性调整细节
- **流式传输**：支持动画和语音的流式播放

### 5. 🤝 多代理协作

- **场景管理**：多个虚拟形象在同一场景中
- **互动协调**：代理之间的眼神交流和互动
- **角色区分**：清晰的视觉区分不同代理
- **对话轮次**：配合 A2A 协议管理多代理对话

## 技术规范

### 核心设计原则

#### 1. 分层消息架构

ADA 采用**消息分类**而非单一状态对象，实现：

- ✅ **高效增量更新**：仅传输变化的部分 (UPDATE)
- ✅ **精确时序控制**：统一时间轴和同步标记
- ✅ **清晰职责分离**：初始化、更新、动作、语音各司其职
- ✅ **多代理支持**：场景级别的协调和编排

#### 2. 资源引用规范

使用 URI scheme 确保安全和可扩展：

```
ada://models/humanoid/professional-v2      # ADA 内部资源
a2ui://element/sleep-tip-card              # A2UI 元素引用
https://cdn.example.com/assets/custom.glb  # 外部资源
```

**安全保障**：
- 客户端维护 `ada://` 资源白名单
- 外部资源需明确授权
- 防止代码注入和恶意内容

#### 3. 时序同步系统

```typescript
// 全局时间轴
interface Timeline {
  timestamp: number;           // Unix 毫秒时间戳（基准）
  start_offset_ms: number;     // 相对偏移
  duration_ms: number;         // 持续时间
  sync_markers: SyncMarker[];  // 同步锚点
}

// 同步标记（与语音、UI 事件对齐）
interface SyncMarker {
  marker_id: string;           // 如 "speech-segment-1"
  align_to_step: string;       // 对齐的动画步骤
  offset_ms: number;           // 微调偏移
}
```

### 状态模型

ADA 定义了虚拟形象的多层状态：

```typescript
// 消息基础接口
interface ADAMessage {
  version: string;              // 协议版本 (如 "1.0.0")
  type: MessageType;            // 消息类型
  timestamp: number;            // Unix 毫秒时间戳
  agent_id: string;             // 代理唯一标识
  session_id?: string;          // 会话 ID
  correlation_id?: string;      // A2A 关联 ID
  priority?: 'low' | 'medium' | 'high' | 'critical';
}

// 虚拟形象状态（持久层）
interface AvatarAppearance {
  model: {
    asset_id: string;           // 如 "ada://models/humanoid/v2"
    fallback: string[];         // 降级选项
    lod_levels: string[];       // 细节级别
  };
  appearance: {
    preset: string;             // 预设外观
    overrides: object;          // 自定义覆盖
  };
}

// 表现状态（瞬时层，高频更新）
interface ExpressionState {
  preset: string;               // 预设表情（如 "empathetic"）
  blend: {                      // 细粒度混合
    smile: number;              // 0-1
    eye_openness: number;       // 0-1
    brow_position: number;      // 0-1
    [key: string]: number;
  };
  transition?: {
    duration_ms: number;
    easing: string;
  };
}

// 动作序列（行为层）
interface ActionSequence {
  id: string;
  sequence: ActionStep[];
  sync_markers: SyncMarker[];
}

interface ActionStep {
  step_id: string;
  action_ref: string;           // URI: ada://animations/...
  parameters: object;           // 动作参数
  start_offset_ms: number;      // 相对开始时间
  duration_ms: number;
  trigger?: EventTrigger;       // 条件触发
}

// 语音同步
interface SpeechMessage {
  utterance_id: string;
  text: string;
  audio: {
    source: string;             // URL 或 data URI
    format: string;
    duration_ms: number;
  };
  phonemes: Phoneme[];          // 音素序列（口型同步）
  prosody: {
    emotion: string;
    emphasis_words: number[];   // 强调的词索引
    speed_multiplier: number;   // 语速
  };
  facial_animation: {
    lip_sync: 'auto' | 'manual';
    expression_overlay: object; // 叠加表情
  };
}
```

### 渲染器接口

客户端实现标准渲染器接口：

```typescript
interface ADARenderer {
  // 初始化虚拟形象
  initialize(message: InitMessage): Promise<Avatar>;

  // 处理消息（统一入口）
  handleMessage(message: ADAMessage): Promise<void>;

  // 增量更新状态
  applyUpdates(updates: StateUpdate[]): Promise<void>;

  // 执行动作序列
  performAction(action: ActionSequence): Promise<void>;

  // 同步语音和动画
  syncSpeech(speech: SpeechMessage): Promise<void>;

  // 处理用户交互
  handleInteraction(event: InteractionEvent): void;

  // 资源管理
  preloadResources(resources: ResourceManifest): Promise<void>;

  // 场景管理（多代理）
  updateScene(scene: SceneUpdate): Promise<void>;
}
```

### 最佳实践指南

#### 1. 何时使用哪种消息类型

```typescript
// ✅ 初始化：会话开始或代理首次出现
{
  "type": "avatar.init",
  "avatar": {...},
  "initial_state": {...}
}

// ✅ 状态更新：表情、注视等瞬时变化
{
  "type": "avatar.update",
  "updates": [
    {"path": "expression.preset", "value": "happy"}
  ]
}

// ✅ 动作序列：复杂的手势、演示动作
{
  "type": "avatar.action",
  "action": {
    "sequence": [...]
  }
}

// ✅ 语音输出：带口型和表情的语音
{
  "type": "avatar.speech",
  "speech": {
    "text": "...",
    "audio": {...}
  }
}

// ✅ 场景协调：多代理交互
{
  "type": "scene.update",
  "scene": {
    "participants": [...],
    "interactions": [...]
  }
}
```

#### 2. 增量更新 vs 完整状态

```typescript
// ❌ 低效：每次都发送完整状态
{
  "type": "avatar.init",  // 错误：不应该重复初始化
  "avatar": {...全部数据...}
}

// ✅ 高效：仅发送变化
{
  "type": "avatar.update",
  "updates": [
    {"path": "expression.preset", "value": "surprised"}
  ]
}
```

#### 3. 时序同步模式

```typescript
// 模式 1: 基于时间戳的绝对时序
{
  "timestamp": 1738329600000,  // 全局基准
  "action": {
    "sequence": [
      {"start_offset_ms": 0},    // 立即开始
      {"start_offset_ms": 500}   // 500ms 后
    ]
  }
}

// 模式 2: 基于标记的相对同步（推荐用于语音）
{
  "action": {
    "sequence": [...],
    "sync_markers": [
      {
        "marker_id": "speech-word-3",
        "align_to_step": "gesture-point"
      }
    ]
  }
}
```

#### 4. 与 A2UI 的协同

```typescript
// ADA 手势指向 A2UI 元素
{
  "type": "avatar.action",
  "action": {
    "sequence": [
      {
        "action_ref": "ada://animations/gestures/point",
        "parameters": {
          // 引用 A2UI 元素
          "target_ui": "a2ui://element/tip-card-1"
        }
      }
    ]
  }
}

// A2UI 元素事件触发 ADA 反应
{
  "type": "avatar.interaction",
  "interaction": {
    "event_type": "a2ui_button_clicked",
    "source": "a2ui://element/submit-button",
    "response": {
      "expression": {"preset": "pleased"},
      "gesture": "thumbs-up"
    }
  }
}
```

#### 5. 性能优化策略

```typescript
// 策略 1: 资源预加载
{
  "type": "resource.preload",
  "resources": {
    "animations": [
      {
        "asset_id": "ada://animations/gestures/nod",
        "priority": "high",
        "preload_strategy": "eager"  // 立即加载
      }
    ]
  }
}

// 策略 2: 优先级系统
{
  "type": "avatar.update",
  "priority": "critical",  // 面部表情优先
  "updates": [...]
}

// 策略 3: LOD 自适应
{
  "avatar": {
    "model": {
      "lod_levels": ["high", "medium", "low"],
      "auto_select": true  // 客户端根据性能选择
    }
  }
}
```

#### 6. 错误处理和降级

```typescript
// 资源降级
{
  "avatar": {
    "model": {
      "asset_id": "ada://models/custom/advanced",
      "fallback": [
        "ada://models/humanoid/standard",
        "ada://models/humanoid/basic",
        "ada://models/2d/sprite"  // 最终降级到 2D
      ]
    }
  }
}

// 功能检测
{
  "avatar": {
    "capabilities": {
      "expressions": ["basic"],     // 必需
      "gestures": ["professional"], // 可选
      "voice": {"enabled": true}    // 如果不支持则静音
    }
  }
}
```

## 与生态系统的集成

### ADA + A2UI：完整的用户体验

```
┌────────────────────────────────────────┐
│         智能代理                        │
├────────────────┬───────────────────────┤
│   ADA 生成器   │    A2UI 生成器         │
│   (虚拟形象)   │   (用户界面)           │
└───────┬────────┴──────────┬────────────┘
        │                   │
        ▼                   ▼
┌───────────────────────────────────────┐
│         客户端应用                     │
│  ┌─────────────┐    ┌──────────────┐  │
│  │ ADA 渲染器  │    │  A2UI 渲染器  │  │
│  │  (形象)     │    │   (界面)     │  │
│  └─────────────┘    └──────────────┘  │
│                                       │
│  统一的品牌化用户体验                  │
└───────────────────────────────────────┘
```

**示例场景：健康顾问代理**
- **ADA**：显示友好的医生形象，用手势指向建议项
- **A2UI**：展示交互式健康计划表单
- **协同**：虚拟形象与 UI 元素同步交互

### ADA + A2A：多代理虚拟会议

```
场景：三个代理协作解决问题

┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  项目经理   │  │  技术专家   │  │  设计师     │
│   代理      │  │   代理      │  │   代理      │
└──────┬──────┘  └──────┬──────┘  └──────┬──────┘
       │ ADA            │ ADA            │ ADA
       └────────────────┼────────────────┘
                        ▼
              ┌──────────────────┐
              │  虚拟会议室       │
              │  三个虚拟形象     │
              │  互相交流讨论     │
              └──────────────────┘
```

### 支持的平台和框架

#### 当前支持 ✅
- **Web**: Three.js, Babylon.js
- **移动端**: Unity (iOS/Android)
- **框架**: React Three Fiber

#### 计划支持 🔜
- **VR/AR**: ARKit, ARCore, WebXR
- **游戏引擎**: Unreal Engine
- **3D 工具**: Blender 插件
- **实时通信**: WebRTC 集成

## 实际应用案例

### 1. 客户服务代理
**场景**：电商平台的 AI 客服

```json
{
  "type": "avatar.init",
  "agent_id": "customer-service-001",

  "avatar": {
    "model": {
      "asset_id": "ada://models/service/retail-assistant"
    },
    "appearance": {
      "preset": "professional-friendly",
      "branding": "company-theme"
    }
  },

  "initial_state": {
    "expression": {
      "preset": "helpful",
      "blend": {"smile": 0.8}
    },
    "pose": {"gesture": "welcome"}
  }
}
```

**效果**：
- 增加客户信任感
- 降低沟通障碍
- 提升品牌形象

### 2. 教育辅导代理
**场景**：在线编程教师

```json
{
  "type": "avatar.action",
  "agent_id": "teacher-coding-001",

  "action": {
    "id": "encourage-student",
    "sequence": [
      {
        "step_id": "point-to-code",
        "action_ref": "ada://animations/teaching/point",
        "parameters": {
          "target_ui": "a2ui://element/code-editor#line-42"
        },
        "start_offset_ms": 0,
        "duration_ms": 1500
      },
      {
        "step_id": "show-approval",
        "action_ref": "ada://animations/gestures/thumbs-up",
        "parameters": {"enthusiasm": 0.9},
        "start_offset_ms": 2000,
        "duration_ms": 800,
        "trigger": {
          "event": "user-success",
          "condition": "code_passes_test"
        }
      }
    ]
  }
}
```

**效果**：
- 提高学习动机
- 及时情感反馈
- 更好的指导效果

### 3. 医疗健康助理
**场景**：心理健康支持

```json
{
  "type": "avatar.update",
  "agent_id": "therapist-001",

  "updates": [
    {
      "path": "expression.preset",
      "value": "empathetic",
      "transition": {
        "duration_ms": 1200,
        "easing": "ease-in-out"
      }
    },
    {
      "path": "expression.blend.eye_softness",
      "value": 0.95
    },
    {
      "path": "gaze.target",
      "value": "user",
      "transition": {
        "duration_ms": 600,
        "easing": "ease-out"
      }
    },
    {
      "path": "voice.tone",
      "value": "warm"
    },
    {
      "path": "voice.pace_multiplier",
      "value": 0.85
    }
  ]
}
```

**效果**：
- 建立安全感
- 促进情感表达
- 提供情感支持

### 4. 虚拟团队协作
**场景**：多代理项目会议

```json
{
  "type": "scene.update",
  "timestamp": 1738329620000,
  "session_id": "meeting-xyz-789",

  "scene": {
    "scene_id": "virtual-meeting-room",
    "layout": "conference-table",

    "participants": [
      {
        "agent_id": "project-manager-001",
        "avatar_state": "ada://states/active-speaker",
        "position": {"role": "head-of-table"},
        "attention_level": 1.0
      },
      {
        "agent_id": "developer-002",
        "avatar_state": "ada://states/listening-attentive",
        "position": {"role": "left-side", "seat": 1},
        "attention_level": 0.9
      },
      {
        "agent_id": "designer-003",
        "avatar_state": "ada://states/listening",
        "position": {"role": "right-side", "seat": 1},
        "attention_level": 0.85
      }
    ],

    "interactions": [
      {
        "type": "gaze_directed",
        "from": "project-manager-001",
        "to": "developer-002",
        "intensity": 0.8,
        "start_ms": 0,
        "duration_ms": 2000
      },
      {
        "type": "gesture_reference",
        "from": "project-manager-001",
        "target_ui": "a2ui://element/project-timeline",
        "start_ms": 500
      }
    ],

    "turn_taking": {
      "current_speaker": "project-manager-001",
      "a2a_turn_id": "turn-12345"
    }
  }
}
```

**效果**：
- 清晰的角色区分
- 自然的互动流程
- 提升协作效率

## 快速开始

### 安装

```bash
# 克隆仓库
git clone https://github.com/your-org/ADA.git
cd ADA

# 安装依赖（Web 示例）
cd samples/web-threejs
npm install
npm run dev
```

### 基础示例（客户端）

```javascript
import { ADARenderer } from 'ada-renderer-web';

// 1. 初始化渲染器
const renderer = new ADARenderer({
  container: document.getElementById('avatar-container'),
  framework: 'threejs',
  quality: 'auto'  // 自动根据设备选择质量
});

// 2. 处理初始化消息
await renderer.handleMessage({
  type: 'avatar.init',
  version: '1.0.0',
  timestamp: Date.now(),
  agent_id: 'assistant-001',

  avatar: {
    model: {
      asset_id: 'ada://models/humanoid/professional-v2'
    },
    appearance: {
      preset: 'friendly-assistant'
    }
  },

  initial_state: {
    expression: {preset: 'neutral-friendly'},
    pose: {posture: 'standing-relaxed'}
  }
});

// 3. 增量更新表情
await renderer.handleMessage({
  type: 'avatar.update',
  timestamp: Date.now(),
  agent_id: 'assistant-001',

  updates: [
    {
      path: 'expression.preset',
      value: 'happy',
      transition: {duration_ms: 600}
    }
  ]
});

// 4. 执行手势动画
await renderer.handleMessage({
  type: 'avatar.action',
  timestamp: Date.now(),
  agent_id: 'assistant-001',

  action: {
    id: 'greeting',
    sequence: [
      {
        step_id: 'wave',
        action_ref: 'ada://animations/gestures/wave',
        parameters: {hand: 'right'},
        start_offset_ms: 0,
        duration_ms: 1200
      }
    ]
  }
});

// 5. 播放语音同步动画
await renderer.handleMessage({
  type: 'avatar.speech',
  timestamp: Date.now(),
  agent_id: 'assistant-001',

  speech: {
    utterance_id: 'greeting-001',
    text: '你好！我能帮你什么？',
    audio: {
      source: audioDataURL,
      format: 'webm',
      duration_ms: 2000
    },
    prosody: {
      emotion: 'welcoming',
      speed_multiplier: 1.0
    },
    facial_animation: {
      lip_sync: 'auto',
      expression_overlay: {
        smile: {value: 0.7}
      }
    }
  }
});
```

### 与智能代理集成（服务端）

```python
from ada import ADAClient, MessageType
from your_agent import Agent

# 初始化
agent = Agent()
ada_client = ADAClient(
    agent_id="health-advisor-001",
    session_id="session-abc-123"
)

# 场景 1: 代理首次出现
await ada_client.send_init({
    "avatar": {
        "model": {"asset_id": "ada://models/humanoid/professional-v2"},
        "appearance": {"preset": "medical-professional"}
    },
    "initial_state": {
        "expression": {"preset": "neutral-friendly"},
        "pose": {"posture": "standing-relaxed"}
    }
})

# 场景 2: 处理用户输入
user_input = "我感觉有点累"
response = agent.process_input(user_input)

# 检测到关切情绪 -> 更新表情
if response.detected_emotion == "concerned":
    await ada_client.send_update([
        {
            "path": "expression.preset",
            "value": "empathetic",
            "transition": {"duration_ms": 800}
        },
        {
            "path": "gaze.target",
            "value": "user"
        }
    ], priority="high")

# 场景 3: 生成响应并同步动作
speech_text = response.text  # "充足的睡眠很重要..."

# 先发送动作序列
await ada_client.send_action({
    "id": "explain-with-gesture",
    "sequence": [
        {
            "step_id": "nod",
            "action_ref": "ada://animations/gestures/nod",
            "parameters": {"intensity": 0.7},
            "start_offset_ms": 0,
            "duration_ms": 600
        },
        {
            "step_id": "point-to-tips",
            "action_ref": "ada://animations/gestures/point",
            "parameters": {
                "hand": "right",
                "target_ui": "a2ui://element/health-tips"
            },
            "start_offset_ms": 1200,
            "duration_ms": 1500
        }
    ],
    "sync_markers": [
        {"marker_id": "speech-start", "align_to_step": "nod"}
    ]
})

# 然后发送语音消息（自动与动作同步）
audio_data = await text_to_speech(speech_text, emotion="caring")

await ada_client.send_speech({
    "utterance_id": f"utterance-{uuid.uuid4()}",
    "text": speech_text,
    "audio": {
        "source": audio_data.url,
        "format": "webm",
        "duration_ms": audio_data.duration
    },
    "prosody": {
        "emotion": "caring",
        "speed_multiplier": 0.95
    },
    "facial_animation": {
        "lip_sync": "auto",
        "expression_overlay": {
            "smile": {"value": 0.5, "hold_duration_pct": 80}
        }
    }
})

# 场景 4: 高级用法 - 与 A2UI 协同
# 代理显示健康建议卡片，同时用手势指向
await ada_client.send_action({
    "id": "point-to-ui-card",
    "sequence": [
        {
            "step_id": "point",
            "action_ref": "ada://animations/gestures/point",
            "parameters": {
                "target_ui": "a2ui://element/sleep-schedule-card",
                "look_at_target": True
            },
            "start_offset_ms": 0,
            "duration_ms": 2000
        }
    ]
})
```

### 高级集成模式

#### 与 A2A 协议集成

```python
from ada import ADAClient
from a2a import A2AProtocol

# 多代理场景：三个代理协作
a2a_session = A2AProtocol.create_session("team-meeting")

# 代理 1: 项目经理
pm_agent = ADAClient(agent_id="pm-001", session_id=a2a_session.id)
await pm_agent.send_init({...})

# 代理 2: 开发者
dev_agent = ADAClient(agent_id="dev-002", session_id=a2a_session.id)
await dev_agent.send_init({...})

# 更新场景：项目经理向开发者提问
await ADAClient.update_scene({
    "scene_id": "virtual-meeting",
    "participants": [
        {
            "agent_id": "pm-001",
            "avatar_state": "ada://states/active-speaker",
            "attention_level": 1.0
        },
        {
            "agent_id": "dev-002",
            "avatar_state": "ada://states/listening",
            "attention_level": 0.9
        }
    ],
    "interactions": [
        {
            "type": "gaze_directed",
            "from": "pm-001",
            "to": "dev-002",
            "intensity": 0.9
        }
    ],
    "turn_taking": {
        "current_speaker": "pm-001",
        "a2a_turn_id": a2a_session.current_turn.id
    }
})
```

## 与其他标准的比较

### ADA vs. VRM/glTF
- **VRM/glTF**：静态 3D 模型格式
- **ADA**：动态、上下文感知的虚拟形象控制协议
- **关系**：ADA 可以使用 VRM/glTF 作为底层模型格式

### ADA vs. 传统动画管道
- **传统**：预制动画，设计师手动创建
- **ADA**：AI 代理实时生成，上下文驱动
- **关系**：ADA 可以调用预制动画作为构建块

### ADA vs. 虚拟主播 (VTuber) 技术
- **VTuber**：真人驱动，实时面捕
- **ADA**：AI 驱动，自主决策表情和动作
- **关系**：技术可以互补，ADA 可集成面捕输入

## 技术原理

### 情感映射系统

ADA 使用情感计算模型将对话上下文映射到虚拟形象表现：

```
文本情感分析 ──┐
              ├──> 情感向量 ──> 面部表情参数
语音韵律分析 ──┘                     │
                                    ├──> ADA 状态
对话意图识别 ──┐                     │
              ├──> 行为决策 ──> 肢体动作序列
用户反馈分析 ──┘
```

### 优先级和带宽优化

```typescript
// 状态更新优先级
enum UpdatePriority {
  CRITICAL = 0,   // 面部表情（高频更新）
  HIGH = 1,       // 手势和姿态
  MEDIUM = 2,     // 视线和微表情
  LOW = 3         // 环境和氛围
}

// 智能差分更新
function createDeltaUpdate(
  previousState: ADAState,
  newState: ADAState
): ADADelta {
  // 仅传输变化的部分
  return computeDelta(previousState, newState);
}
```

### 性能指标

目标性能标准：
- **初始加载**：< 2秒（标准模型）
- **状态更新延迟**：< 100ms
- **动画流畅度**：60 FPS（桌面），30 FPS（移动端）
- **带宽占用**：< 50 KB/s（持续对话）

## 路线图

### v1.0（当前）✅
- [x] 核心规范定义
- [x] Web 渲染器（Three.js）
- [x] 基础情感表情系统
- [x] Python/JavaScript SDK
- [x] 示例应用

### v1.5（Q2 2026）🔜
- [ ] Unity/Unreal 渲染器
- [ ] 高级动画混合
- [ ] 语音-动画自动同步
- [ ] AR/VR 支持
- [ ] 性能优化工具

### v2.0（Q4 2026）🎯
- [ ] 实时面部动捕集成
- [ ] 多代理场景编排
- [ ] 自定义模型训练工具
- [ ] 企业级部署方案
- [ ] 完整的辅助功能支持

### 未来展望 🚀
- [ ] 全息投影支持
- [ ] 触觉反馈集成
- [ ] 神经接口探索
- [ ] 元宇宙平台集成

## 社区与生态

### 合作伙伴

我们很荣幸能与以下组织合作：

**技术伙伴**
- 🌐 **A2UI 项目**：无缝的 UI + Avatar 集成
- 🔗 **A2A 协议**：多代理通信标准
- 🎮 **Unity Technologies**：游戏引擎集成
- 🎨 **Ready Player Me**：虚拟形象资源库

**行业应用**
- 🏥 **医疗健康**：心理咨询、康复训练
- 🎓 **教育培训**：在线教学、技能培训
- 🛍️ **电子商务**：虚拟导购、客户服务
- 🏢 **企业协作**：虚拟会议、远程协作

### 贡献指南

我们欢迎社区贡献！您可以：

1. **开发渲染器**：为您喜爱的平台/框架添加支持
2. **创建资源**：贡献虚拟形象模型和动画
3. **改进规范**：提出增强建议和用例
4. **编写文档**：教程、示例、最佳实践
5. **报告问题**：Bug 反馈和功能请求

```bash
# Fork 项目
git clone https://github.com/your-org/ADA.git
cd ADA

# 创建功能分支
git checkout -b feature/your-feature

# 提交更改
git commit -m "Add: your feature description"

# 推送并创建 PR
git push origin feature/your-feature
```

### 许可证

ADA 采用 **Apache 2.0** 开源许可证，您可以自由使用、修改和分发。

## 资源链接

- 📘 **官方文档**：https://ada.dev/docs
- 💻 **GitHub 仓库**：https://github.com/your-org/ADA
- 🎮 **在线演示**：https://ada.dev/playground
- 💬 **社区论坛**：https://community.ada.dev
- 📧 **邮件列表**：ada-dev@googlegroups.com
- 🐦 **Twitter**：@ADAOpenSource

## 常见问题

### Q: ADA 与 A2UI 的关系？
A: ADA 和 A2UI 是互补的。A2UI 处理界面元素，ADA 处理虚拟形象。它们可以协同工作，提供完整的代理用户体验。

### Q: 需要 3D 建模经验吗？
A: 不需要。ADA 提供预制的虚拟形象库。开发者只需通过 JSON 配置即可使用。

### Q: 支持自定义虚拟形象吗？
A: 支持。您可以导入 VRM/glTF 格式的自定义模型，并映射到 ADA 的控制系统。

### Q: 性能要求高吗？
A: ADA 支持多级质量。从低端移动设备（2D sprite）到高端 VR（完整 3D），都能良好运行。

### Q: 如何保证内容安全？
A: 客户端完全控制资源库和渲染。代理只能从预批准的库中选择，无法注入任意内容。

### Q: 商业使用需要付费吗？
A: 不需要。ADA 是完全开源和免费的，包括商业用途。

## JSON 格式完整参考

详细的消息格式规范请参考：
- 📘 [JSON Schema 定义](https://ada.dev/schemas/v1/message.json)
- 📄 [格式设计分析文档](./JSON_DESIGN_ANALYSIS.md)
- 🔍 [互动式格式浏览器](https://ada.dev/schema-explorer)

### 消息类型速查表

| 消息类型 | 用途 | 频率 | 带宽 | 示例场景 |
|---------|------|------|------|---------|
| `avatar.init` | 初始化虚拟形象 | 一次/会话 | 高 | 代理首次出现 |
| `avatar.update` | 增量状态更新 | 频繁 | 低 | 表情变化 |
| `avatar.action` | 执行动作序列 | 中等 | 中 | 手势、演示 |
| `avatar.speech` | 语音+口型同步 | 中等 | 高 | 语音输出 |
| `avatar.interaction` | 用户交互响应 | 按需 | 低 | 点击反馈 |
| `scene.update` | 场景管理 | 低 | 中 | 多代理协调 |
| `resource.preload` | 资源预加载 | 低 | 中 | 性能优化 |
| `avatar.event` | 事件通知 | 按需 | 低 | 状态回调 |

### 字段约束规范

```typescript
// 时间相关
timestamp: number;          // Unix 毫秒时间戳, >= 0
duration_ms: number;        // 毫秒, 0-300000 (5分钟)
start_offset_ms: number;    // 毫秒, >= 0

// 数值范围
intensity: number;          // 0.0 - 1.0
attention_level: number;    // 0.0 - 1.0
speed_multiplier: number;   // 0.5 - 2.0
blend_values: number;       // 0.0 - 1.0

// 字符串格式
agent_id: string;           // 正则: ^[a-zA-Z0-9-_]+$
asset_id: string;           // URI: ada://path/to/asset
version: string;            // 语义版本: X.Y.Z

// 枚举值
priority: 'low' | 'medium' | 'high' | 'critical';
easing: 'linear' | 'ease-in' | 'ease-out' | 'ease-in-out';
```

### 常见错误和解决方案

#### ❌ 错误 1: 混淆消息类型

```json
// 错误：使用 UPDATE 初始化
{
  "type": "avatar.update",
  "updates": [
    {"path": "avatar.model", "value": {...}}  // 太重了
  ]
}

// 正确：使用 INIT
{
  "type": "avatar.init",
  "avatar": {...}
}
```

#### ❌ 错误 2: 时序不一致

```json
// 错误：没有时间基准
{
  "action": {
    "sequence": [
      {"duration_ms": 500},  // 何时开始？
      {"duration_ms": 300}
    ]
  }
}

// 正确：明确时序
{
  "timestamp": 1738329600000,
  "action": {
    "sequence": [
      {"start_offset_ms": 0, "duration_ms": 500},
      {"start_offset_ms": 500, "duration_ms": 300}
    ]
  }
}
```

#### ❌ 错误 3: 资源引用不规范

```json
// 错误：裸字符串
{
  "action_ref": "nod"  // 不清楚是什么资源
}

// 正确：URI 引用
{
  "action_ref": "ada://animations/gestures/nod"
}
```

### 版本演进策略

```json
// v1.0.0 (当前)
{
  "version": "1.0.0",
  "type": "avatar.update",
  "updates": [...]
}

// v1.1.0 (向后兼容，添加新字段)
{
  "version": "1.1.0",
  "type": "avatar.update",
  "updates": [...],
  "batch_id": "optional-new-field"  // v1.0 客户端会忽略
}

// v2.0.0 (重大变更，不兼容)
{
  "version": "2.0.0",
  "type": "avatar.state.update",  // 类型名称变化
  "changes": [...]  // 字段名称变化
}
```

## 开始使用 🚀

今天就开始探索 ADA！

```bash
# 快速开始
git clone https://github.com/your-org/ADA.git
cd ADA/samples/quickstart
npm install && npm start

# 在浏览器中查看
# http://localhost:3000
```

访问我们的[互动教程](https://ada.dev/tutorial)，10 分钟内构建您的第一个智能虚拟形象！

**推荐学习路径**：
1. 📖 阅读 [JSON 格式设计分析](./JSON_DESIGN_ANALYSIS.md)
2. 🎮 试玩 [在线演示](https://ada.dev/playground)
3. 💻 克隆仓库并运行示例
4. 🛠️ 集成到您的代理项目

---

## 联系我们

有问题或建议？我们很乐意倾听！

- 💡 功能请求：[GitHub Issues](https://github.com/your-org/ADA/issues)
- 🐛 Bug 报告：[GitHub Issues](https://github.com/your-org/ADA/issues)
- 💬 一般讨论：[Discussions](https://github.com/your-org/ADA/discussions)
- 📧 商务合作：ada-partnerships@example.com

---

**让我们一起构建更人性化的 AI 未来！** 🤖❤️

---

*ADA 项目 - Agent Driven Avatar*
*由开源社区驱动，为更好的人机交互而生*

**版本**：1.0.0
**更新日期**：2026-01-31
**许可证**：Apache 2.0
