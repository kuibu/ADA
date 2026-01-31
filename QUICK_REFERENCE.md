# ADA JSON 格式快速参考

> **一页式速查表**：快速查找 ADA 消息格式

---

## 🎯 8 种消息类型

```
avatar.init        → 初始化虚拟形象（会话开始）
avatar.update      → 增量更新状态（高频）⭐
avatar.action      → 执行动作序列（手势、演示）
avatar.speech      → 语音+口型同步
avatar.interaction → 响应用户交互
scene.update       → 多代理场景管理
resource.preload   → 资源预加载（性能优化）
avatar.event       → 事件通知
```

---

## 📦 消息基础结构

所有消息都包含这些字段：

```json
{
  "version": "1.0.0",           // 必需：协议版本
  "type": "avatar.update",      // 必需：消息类型
  "timestamp": 1738329600000,   // 必需：Unix 毫秒时间戳
  "agent_id": "agent-001",      // 必需：代理 ID
  "session_id": "session-xyz",  // 可选：会话 ID
  "correlation_id": "a2a-123",  // 可选：A2A 关联 ID
  "priority": "high"            // 可选：优先级
}
```

---

## 🎭 常用场景速查

### 1️⃣ 初始化虚拟形象

```json
{
  "type": "avatar.init",
  "avatar": {
    "model": {"asset_id": "ada://models/humanoid/professional-v2"},
    "appearance": {"preset": "medical-professional"}
  },
  "initial_state": {
    "expression": {"preset": "neutral-friendly"},
    "pose": {"posture": "standing-relaxed"}
  }
}
```

### 2️⃣ 改变表情（最常用）⭐

```json
{
  "type": "avatar.update",
  "updates": [
    {
      "path": "expression.preset",
      "value": "happy",
      "transition": {"duration_ms": 600}
    }
  ]
}
```

### 3️⃣ 执行手势

```json
{
  "type": "avatar.action",
  "action": {
    "sequence": [
      {
        "step_id": "wave",
        "action_ref": "ada://animations/gestures/wave",
        "start_offset_ms": 0,
        "duration_ms": 1200
      }
    ]
  }
}
```

### 4️⃣ 语音输出

```json
{
  "type": "avatar.speech",
  "speech": {
    "text": "你好！",
    "audio": {"source": "https://...", "duration_ms": 1500},
    "prosody": {"emotion": "friendly"},
    "facial_animation": {"lip_sync": "auto"}
  }
}
```

### 5️⃣ 手势指向 UI 元素

```json
{
  "type": "avatar.action",
  "action": {
    "sequence": [
      {
        "action_ref": "ada://animations/gestures/point",
        "parameters": {
          "target_ui": "a2ui://element/tip-card"
        }
      }
    ]
  }
}
```

---

## 🔗 URI 引用系统

```
ada://models/humanoid/v2           → ADA 内部模型
ada://animations/gestures/wave     → ADA 内部动画
a2ui://element/button-1            → A2UI 元素
https://cdn.example.com/model.glb  → 外部资源（需授权）
```

---

## 📊 字段值域约束

```typescript
// 数值范围
intensity: 0.0 - 1.0
attention_level: 0.0 - 1.0
speed_multiplier: 0.5 - 2.0
blend_values: 0.0 - 1.0

// 时间
timestamp: Unix 毫秒，>= 0
duration_ms: 0 - 300000 (5分钟)
start_offset_ms: >= 0

// 字符串
agent_id: /^[a-zA-Z0-9-_]+$/
version: "X.Y.Z"

// 枚举
priority: "low" | "medium" | "high" | "critical"
easing: "linear" | "ease-in" | "ease-out" | "ease-in-out"
```

---

## 🎬 时序控制模式

### 绝对时序（推荐用于独立动作）

```json
{
  "timestamp": 1738329600000,  // 基准时间
  "action": {
    "sequence": [
      {"start_offset_ms": 0},    // timestamp + 0
      {"start_offset_ms": 500}   // timestamp + 500
    ]
  }
}
```

### 相对同步（推荐用于语音同步）

```json
{
  "action": {
    "sequence": [...],
    "sync_markers": [
      {
        "marker_id": "speech-word-5",
        "align_to_step": "gesture-point",
        "offset_ms": 100
      }
    ]
  }
}
```

---

## 🔄 增量更新路径

```json
{
  "type": "avatar.update",
  "updates": [
    {"path": "expression.preset", "value": "happy"},
    {"path": "expression.blend.smile", "value": 0.8},
    {"path": "gaze.target", "value": "user"},
    {"path": "pose.posture", "value": "sitting"},
    {"path": "voice.tone", "value": "warm"}
  ]
}
```

常用路径：
```
expression.preset
expression.blend.{smile|eye_openness|brow_position}
gaze.target
gaze.focus
pose.posture
pose.gesture
voice.tone
voice.pace_multiplier
```

---

## 🎨 常用预设值

### 表情预设
```
neutral-friendly, happy, empathetic, surprised,
concerned, proud, supportive, thinking, pleased
```

### 姿势预设
```
standing-relaxed, standing-formal, sitting-relaxed,
sitting-attentive, leaning-forward
```

### 手势动作
```
ada://animations/gestures/wave
ada://animations/gestures/nod
ada://animations/gestures/point
ada://animations/gestures/thumbs-up
ada://animations/gestures/open-hands
ada://animations/gestures/thinking-pose
```

### 情感韵律
```
friendly, professional, caring, enthusiastic,
calm, serious, playful, empathetic
```

---

## 🚀 性能优化技巧

### 1. 使用增量更新而非完整状态

```json
// ✅ 好
{"type": "avatar.update", "updates": [{"path": "expression.preset", "value": "happy"}]}

// ❌ 差
{"type": "avatar.init", "avatar": {...全部数据...}}
```

### 2. 预加载常用资源

```json
{
  "type": "resource.preload",
  "resources": {
    "animations": [
      {"asset_id": "ada://animations/gestures/nod", "priority": "high"}
    ]
  }
}
```

### 3. 使用优先级系统

```json
{"type": "avatar.update", "priority": "critical"}  // 面部表情
{"type": "avatar.update", "priority": "high"}      // 手势
{"type": "avatar.update", "priority": "medium"}    // 视线
```

### 4. 启用 LOD 自适应

```json
{
  "avatar": {
    "model": {
      "lod_levels": ["high", "medium", "low"],
      "auto_select": true
    }
  }
}
```

---

## 🛡️ 安全最佳实践

1. **仅使用 `ada://` 资源**（客户端白名单）
2. **验证所有数值范围**（0-1, 枚举等）
3. **使用 JSON Schema 验证**
4. **检查版本兼容性**
5. **外部资源需明确授权**

---

## 🔗 与其他协议集成

### 与 A2UI 集成

```json
// ADA 指向 A2UI 元素
{
  "parameters": {
    "target_ui": "a2ui://element/card-1"
  }
}

// A2UI 事件触发 ADA 响应
{
  "type": "avatar.interaction",
  "interaction": {
    "event_type": "a2ui_button_clicked",
    "source": "a2ui://element/button-1"
  }
}
```

### 与 A2A 集成

```json
{
  "correlation_id": "a2a-turn-12345",  // A2A 对话轮次 ID
  "type": "scene.update",
  "scene": {
    "turn_taking": {
      "current_speaker": "agent-001",
      "a2a_turn_id": "turn-12345"
    }
  }
}
```

---

## 📚 相关文档

- 📘 [完整 README](./README.md)
- 📄 [JSON 设计分析](./JSON_DESIGN_ANALYSIS.md)
- 🔀 [改进对比文档](./DESIGN_COMPARISON.md)
- 🌐 [在线文档](https://ada.dev/docs)
- 🎮 [互动演示](https://ada.dev/playground)

---

## 💡 快速示例

### 最小化示例

```json
{
  "version": "1.0.0",
  "type": "avatar.update",
  "timestamp": 1738329600000,
  "agent_id": "demo",
  "updates": [{"path": "expression.preset", "value": "happy"}]
}
```

### 完整示例

```json
{
  "version": "1.0.0",
  "type": "avatar.action",
  "timestamp": 1738329600000,
  "agent_id": "teacher-001",
  "session_id": "lesson-123",
  "priority": "high",

  "action": {
    "id": "explain-concept",
    "sequence": [
      {
        "step_id": "point-to-board",
        "action_ref": "ada://animations/teaching/point",
        "parameters": {
          "hand": "right",
          "target_ui": "a2ui://element/explanation-card"
        },
        "start_offset_ms": 0,
        "duration_ms": 2000
      }
    ],
    "sync_markers": [
      {
        "marker_id": "speech-emphasis",
        "align_to_step": "point-to-board",
        "offset_ms": 500
      }
    ]
  }
}
```

---

**提示**：在生产环境中，始终使用 TypeScript 类型定义或 JSON Schema 验证以确保消息格式正确！

```bash
npm install ada-types  # TypeScript 类型
npm install ada-validator  # JSON Schema 验证器
```

---

*ADA v1.0.0 | 更新于 2026-01-31*
