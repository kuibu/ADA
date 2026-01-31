# ADA JSON 格式深度反思与改进方案

## 当前设计的问题分析

### 1. **消息类型缺失**
- ❌ 没有区分初始化、增量更新、完整状态等不同消息类型
- ❌ 无法高效实现增量更新（核心性能需求）
- ❌ 无法区分命令式（执行动作）vs 声明式（设置状态）

### 2. **时序控制不清晰**
- ❌ `timing_ms` 是相对时间还是绝对时间？
- ❌ 缺少全局时间轴参考
- ❌ 动画序列如何与语音精确同步？
- ❌ 如何处理打断和状态转换？

### 3. **状态层次混乱**
- ❌ `state`、`animation`、`speech` 平级，但实际上语音和动画应该是状态的一部分
- ❌ 缺少明确的持久状态 vs 瞬时状态区分
- ❌ 更新粒度控制不足

### 4. **资源引用不规范**
- ❌ 字符串类型的动作名称（如 "nod", "gesture-point"）无法扩展
- ❌ 缺少资源版本控制
- ❌ 无法预加载和缓存管理

### 5. **多代理场景支持不足**
- ❌ 场景管理和单个虚拟形象混在一起
- ❌ 缺少代理间交互的明确表达
- ❌ 无法表示注意力转移和对话轮次

### 6. **安全和验证**
- ❌ 缺少消息签名和来源验证
- ❌ 没有明确的值域约束
- ❌ 缺少错误处理机制

### 7. **与 A2UI/A2A 集成**
- ❌ 缺少与 A2UI 元素的明确关联机制
- ❌ 缺少 A2A 对话轮次的同步

## 改进设计原则

### 核心原则
1. **消息类型明确化** - 使用 `type` 字段区分不同消息
2. **时序精确控制** - 统一的时间轴和同步机制
3. **分层状态管理** - 基础层 / 表现层 / 行为层
4. **资源引用规范** - 使用 URI 或结构化引用
5. **增量优先** - 默认支持差分更新
6. **协议互操作** - 与 A2UI、A2A 无缝集成

## 改进后的 JSON 架构

### 消息类型体系

```typescript
// 消息基础接口
interface ADAMessage {
  version: string;              // 协议版本
  type: MessageType;            // 消息类型
  timestamp: number;            // Unix 时间戳（毫秒）
  agent_id: string;             // 代理标识
  session_id?: string;          // 会话 ID
  correlation_id?: string;      // 关联 ID（与 A2A 对应）
  priority?: Priority;          // 优先级
}

enum MessageType {
  INIT = 'avatar.init',              // 初始化虚拟形象
  UPDATE = 'avatar.update',          // 增量更新状态
  ACTION = 'avatar.action',          // 执行动作
  SPEECH = 'avatar.speech',          // 语音输出
  INTERACTION = 'avatar.interaction', // 用户交互
  SCENE = 'scene.update',            // 场景管理（多代理）
  RESOURCE = 'resource.preload',     // 资源预加载
  EVENT = 'avatar.event'             // 事件通知
}
```

### 1. 初始化消息 (INIT)

完整的虚拟形象初始化，类似于 A2UI 的初始 render。

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
      "fallback": ["ada://models/humanoid/professional-v1", "ada://models/humanoid/basic"],
      "lod_levels": ["high", "medium", "low"]
    },

    "appearance": {
      "preset": "medical-professional",
      "overrides": {
        "skin_tone": "medium",
        "hair_style": "short-professional",
        "clothing": {
          "upper": "white-coat",
          "lower": "formal-pants",
          "accessories": ["stethoscope", "id-badge"]
        }
      }
    },

    "capabilities": {
      "expressions": ["basic", "medical-empathy"],
      "gestures": ["professional", "medical-demo"],
      "voice": {
        "enabled": true,
        "language": "zh-CN",
        "accent": "neutral"
      }
    }
  },

  "initial_state": {
    "pose": {
      "posture": "standing-relaxed",
      "position": {"x": 0, "y": 0, "z": 0},
      "rotation": {"x": 0, "y": 0, "z": 0}
    },
    "expression": {
      "preset": "neutral-friendly",
      "blend": {
        "smile": 0.3,
        "eye_openness": 0.9,
        "brow_position": 0.5
      }
    },
    "gaze": {
      "target": "camera",
      "focus": 1.0
    }
  },

  "scene_context": {
    "environment": "medical-office",
    "lighting": "soft-professional",
    "camera_distance": "medium"
  }
}
```

### 2. 增量更新消息 (UPDATE)

仅传输变化的状态，优化带宽。

```json
{
  "version": "1.0.0",
  "type": "avatar.update",
  "timestamp": 1738329605000,
  "agent_id": "health-advisor-001",
  "session_id": "session-abc-123",
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
      "path": "expression.blend.smile",
      "value": 0.6,
      "transition": {
        "duration_ms": 600
      }
    },
    {
      "path": "gaze.target",
      "value": "user",
      "transition": {
        "duration_ms": 400,
        "easing": "ease-out"
      }
    }
  ]
}
```

### 3. 动作执行消息 (ACTION)

执行复杂的动画序列和手势。

```json
{
  "version": "1.0.0",
  "type": "avatar.action",
  "timestamp": 1738329610000,
  "agent_id": "health-advisor-001",
  "session_id": "session-abc-123",

  "action": {
    "id": "explain-sleep-tips",
    "sequence": [
      {
        "step_id": "nod-acknowledgement",
        "action_ref": "ada://animations/gestures/nod",
        "parameters": {
          "intensity": 0.7,
          "speed": 1.0
        },
        "start_offset_ms": 0,
        "duration_ms": 600
      },
      {
        "step_id": "gesture-point-right",
        "action_ref": "ada://animations/gestures/point",
        "parameters": {
          "hand": "right",
          "direction": {"x": 0.5, "y": 0.2, "z": -0.3},
          "target_ui": "ada-ui://element/sleep-tip-card"
        },
        "start_offset_ms": 700,
        "duration_ms": 1200
      },
      {
        "step_id": "return-to-rest",
        "action_ref": "ada://animations/transitions/to-rest",
        "start_offset_ms": 2000,
        "duration_ms": 500
      }
    ],

    "sync_markers": [
      {
        "marker_id": "speech-segment-1-start",
        "align_to_step": "nod-acknowledgement",
        "offset_ms": 0
      },
      {
        "marker_id": "speech-segment-2-start",
        "align_to_step": "gesture-point-right",
        "offset_ms": 200
      }
    ]
  }
}
```

### 4. 语音同步消息 (SPEECH)

与 A2UI 配合，支持精确的口型和表情同步。

```json
{
  "version": "1.0.0",
  "type": "avatar.speech",
  "timestamp": 1738329612000,
  "agent_id": "health-advisor-001",
  "session_id": "session-abc-123",

  "speech": {
    "utterance_id": "utterance-001",
    "text": "保证充足的睡眠对恢复精力非常关键",

    "audio": {
      "source": "data:audio/webm;base64,GkXf...",
      "format": "webm",
      "duration_ms": 3500,
      "sample_rate": 24000
    },

    "phonemes": [
      {"phoneme": "b", "start_ms": 0, "end_ms": 80},
      {"phoneme": "ao", "start_ms": 80, "end_ms": 200},
      {"phoneme": "zh", "start_ms": 200, "end_ms": 320}
      // ... 完整音素序列
    ],

    "prosody": {
      "emotion": "caring",
      "emphasis_words": [0, 4, 8],  // 词索引
      "pitch_curve": [0.5, 0.6, 0.7, 0.6, 0.5],
      "speed_multiplier": 0.95
    },

    "sync_markers": [
      {
        "marker_id": "speech-segment-1-start",
        "time_ms": 0
      },
      {
        "marker_id": "speech-segment-2-start",
        "time_ms": 1200
      }
    ],

    "facial_animation": {
      "lip_sync": "auto",  // 从音素自动生成
      "expression_overlay": {
        "smile": {"value": 0.5, "hold_duration_pct": 80},
        "eye_contact": {"intensity": 0.9}
      }
    }
  }
}
```

### 5. 交互消息 (INTERACTION)

处理用户与虚拟形象的交互。

```json
{
  "version": "1.0.0",
  "type": "avatar.interaction",
  "timestamp": 1738329615000,
  "agent_id": "health-advisor-001",
  "session_id": "session-abc-123",

  "interaction": {
    "event_type": "user_click",
    "target": "avatar.hand_gesture",

    "response": {
      "immediate": {
        "expression": {
          "preset": "friendly-surprised",
          "duration_ms": 500
        },
        "gaze": {
          "target": "interaction_point",
          "duration_ms": 800
        }
      },

      "callback": {
        "action": "ada://callbacks/on-avatar-clicked",
        "parameters": {
          "gesture_type": "wave"
        }
      }
    }
  }
}
```

### 6. 场景管理消息 (SCENE)

多代理场景的协调。

```json
{
  "version": "1.0.0",
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
        "avatar_state": "ada://states/listening",
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
        "start_ms": 500,
        "duration_ms": 1500
      }
    ],

    "turn_taking": {
      "current_speaker": "project-manager-001",
      "next_speaker": "developer-002",
      "cooldown_ms": 500
    }
  }
}
```

### 7. 资源预加载消息 (RESOURCE)

优化性能的资源管理。

```json
{
  "version": "1.0.0",
  "type": "resource.preload",
  "timestamp": 1738329600000,
  "agent_id": "health-advisor-001",

  "resources": {
    "animations": [
      {
        "asset_id": "ada://animations/gestures/point",
        "priority": "high",
        "preload_strategy": "eager"
      },
      {
        "asset_id": "ada://animations/gestures/nod",
        "priority": "high",
        "preload_strategy": "eager"
      },
      {
        "asset_id": "ada://animations/expressions/medical-empathy",
        "priority": "medium",
        "preload_strategy": "lazy"
      }
    ],

    "audio": [
      {
        "asset_id": "ada://audio/ambient/office",
        "priority": "low",
        "preload_strategy": "background"
      }
    ],

    "textures": [
      {
        "asset_id": "ada://textures/clothing/white-coat-hd",
        "priority": "medium",
        "lod_level": "adaptive"
      }
    ]
  }
}
```

## 关键改进点总结

### ✅ 1. 消息类型体系
- 明确的 8 种消息类型
- 每种类型有清晰的职责
- 支持增量更新和完整状态

### ✅ 2. 时序控制
- 统一的时间戳基准
- 相对偏移 + 同步标记
- 精确的动画编排

### ✅ 3. 资源引用规范
- URI scheme: `ada://` 用于内部资源
- `a2ui://` 用于 UI 元素引用
- 版本控制和降级策略

### ✅ 4. 状态分层
- 持久状态（appearance）
- 瞬时状态（expression）
- 行为序列（action）

### ✅ 5. 协议互操作
- 与 A2UI 的元素引用
- 与 A2A 的 correlation_id
- 统一的会话管理

### ✅ 6. 性能优化
- 增量更新 (UPDATE)
- 资源预加载 (RESOURCE)
- LOD 和降级支持
- 优先级系统

### ✅ 7. 安全性
- 资源白名单（URI scheme）
- 值域约束（0-1 范围）
- 客户端验证机制

## JSON Schema 验证

为确保互操作性，提供完整的 JSON Schema：

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://ada.dev/schemas/v1/message.json",
  "title": "ADA Message",
  "type": "object",
  "required": ["version", "type", "timestamp", "agent_id"],
  "properties": {
    "version": {
      "type": "string",
      "pattern": "^\\d+\\.\\d+\\.\\d+$"
    },
    "type": {
      "type": "string",
      "enum": [
        "avatar.init",
        "avatar.update",
        "avatar.action",
        "avatar.speech",
        "avatar.interaction",
        "scene.update",
        "resource.preload",
        "avatar.event"
      ]
    },
    "timestamp": {
      "type": "number",
      "minimum": 0
    },
    "agent_id": {
      "type": "string",
      "pattern": "^[a-zA-Z0-9-_]+$"
    }
  }
}
```

## 向后兼容策略

1. **版本协商**：客户端和代理通过版本字段协商
2. **降级支持**：新特性在旧客户端上优雅降级
3. **扩展字段**：使用 `x-*` 前缀的实验性字段
4. **弃用警告**：逐步迁移旧格式

## 实现建议

### SDK 接口示例

```python
from ada import ADAClient, MessageType

client = ADAClient()

# 发送初始化消息
await client.send(MessageType.INIT, {
    "avatar": {
        "model": {"asset_id": "ada://models/humanoid/professional-v2"},
        "appearance": {"preset": "medical-professional"}
    }
})

# 发送增量更新
await client.update({
    "expression.preset": "empathetic",
    "gaze.target": "user"
}, transition_ms=800)

# 执行动作序列
await client.perform_action("explain-sleep-tips", {
    "sync_with_speech": True,
    "ui_target": "sleep-tip-card"
})
```

## 测试用例

提供完整的测试套件：

1. **消息验证测试**：JSON Schema 验证
2. **时序测试**：同步精度测试
3. **性能测试**：增量更新效率
4. **互操作测试**：与 A2UI/A2A 集成
5. **降级测试**：版本兼容性

---

**结论**：改进后的 JSON 格式更加系统化、可扩展，并且与现代 Web 标准和 A2UI/A2A 协议深度集成。
