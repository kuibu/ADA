# ADA: Agent Driven Avatar

<div align="center">

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](https://github.com/kuibu/ADA/releases)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/kuibu/ADA/pulls)
[![GitHub Stars](https://img.shields.io/github/stars/kuibu/ADA?style=social)](https://github.com/kuibu/ADA/stargazers)

**[中文](./README.zh-CN.md) | English**

</div>

> **Give AI agents lifelike avatars for more natural human-computer interaction**

## Overview

Generative AI excels at generating text, images, and code. Now, it's time to use it for generating dynamic, emotionally expressive avatars. Today, we officially launch the **ADA (Agent Driven Avatar)** project, designed to provide a unified standard for avatar representation and interaction for AI agents.

ADA enables AI agents to:
- 🎭 **Dynamically generate avatars** - Create contextually appropriate avatars based on conversation and user preferences
- 🎬 **Express emotions and intent** - Communicate through facial expressions, gestures, and voice synchronization
- 🔄 **Cross-platform consistency** - Maintain consistent experiences across Web, mobile, VR/AR platforms
- 🔒 **Secure and controllable** - Declarative specifications ensure safe and controllable avatar generation
- 🤝 **Multi-agent collaboration** - Support multiple agents interacting as avatars in the same scene

## The Problem: Agents Need a More Human Presence

Imagine you're talking to an AI assistant about health advice:

**Text-only interaction:**
```
User: "I've been feeling fatigued lately, any suggestions?"
Agent: "Recommendations: 1. Get enough sleep 2. Balanced diet 3. Regular exercise"
```

**Interaction with ADA:**

In addition to text responses, the agent can:
- Appear with a caring facial expression
- Use gestures to emphasize key recommendations
- Adjust tone and expression based on topic seriousness
- Demonstrate actions when needed (e.g., exercise postures)

This multimodal interaction:
- Increases user engagement and trust
- Conveys richer emotions and intentions
- Makes complex guidance easier to understand
- Creates a more natural conversational experience

## The Challenge: Cross-Platform Avatar Standardization

In multi-agent and multi-platform environments, avatars face these challenges:

### 1. **Platform Fragmentation**
- Web uses WebGL/Three.js
- Mobile uses Unity/Unreal
- VR/AR has various rendering engines
- Each platform requires custom development

### 2. **Security Concerns**
- How to prevent malicious agents from generating inappropriate content?
- How to ensure avatars comply with brand guidelines?
- How to share avatars between untrusted agents?

### 3. **Performance and Bandwidth**
- 3D models and animation data are large
- Real-time rendering is demanding
- Mobile device performance is limited

### 4. **Multi-Agent Scenarios**
- Multiple agents interacting in the same scene
- Avatars need to be distinguished and coordinated
- Expressions and actions need synchronization

## Solution: Declarative Avatar Specification

ADA provides a **declarative, cross-platform avatar description format**, similar to how A2UI handles user interfaces:

### Core Architecture

```
┌─────────────────────────────────────────────────┐
│             AI Agent                             │
│  ┌──────────────────────────────────────────┐   │
│  │  Decision Engine: Determines expressions,│   │
│  │  actions, voice                          │   │
│  └──────────────┬───────────────────────────┘   │
│                 │                                │
│                 ▼                                │
│  ┌──────────────────────────────────────────┐   │
│  │  ADA Generator: Outputs JSON avatar      │   │
│  │  description                             │   │
│  └──────────────┬───────────────────────────┘   │
└─────────────────┼───────────────────────────────┘
                  │ ADA Message (JSON)
                  │
        ┌─────────┼─────────┐
        │         │         │
        ▼         ▼         ▼
    ┌─────┐  ┌──────┐  ┌─────────┐
    │ Web │  │Mobile│  │ VR/AR   │
    └─────┘  └──────┘  └─────────┘
        │         │         │
        └─────────┼─────────┘
                  ▼
        Client ADA Renderer
    (Renders using local components)
```

### ADA Message System

ADA adopts a **categorized message** architecture supporting efficient incremental updates and precise timing control.

#### Message Types

```typescript
enum MessageType {
  INIT = 'avatar.init',              // Initialize avatar
  UPDATE = 'avatar.update',          // Incremental state update
  ACTION = 'avatar.action',          // Execute action sequence
  SPEECH = 'avatar.speech',          // Speech + lip sync
  INTERACTION = 'avatar.interaction', // User interaction response
  SCENE = 'scene.update',            // Multi-agent scene management
  RESOURCE = 'resource.preload',     // Resource preloading
  EVENT = 'avatar.event'             // Event notification
}
```

#### Example 1: Initialization Message

Complete avatar initialization with appearance, capabilities, and initial state.

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

#### Example 2: Incremental Update Message

Only transmit changed state to optimize bandwidth (similar to A2UI's incremental updates).

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

#### Example 3: Action Execution Message

Orchestrate complex animation sequences with precise UI element synchronization.

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

#### Example 4: Speech Synchronization Message

Precisely synchronized expressions and lip animation with speech.

```json
{
  "version": "1.0.0",
  "type": "avatar.speech",
  "timestamp": 1738329612000,
  "agent_id": "health-advisor-001",

  "speech": {
    "utterance_id": "utterance-001",
    "text": "Getting enough sleep is crucial for energy recovery",

    "audio": {
      "source": "https://cdn.example.com/audio/utterance-001.webm",
      "format": "webm",
      "duration_ms": 3500
    },

    "phonemes": [
      {"phoneme": "g", "start_ms": 0, "end_ms": 80},
      {"phoneme": "eh", "start_ms": 80, "end_ms": 200}
      // ... Auto-generated lip sync
    ],

    "prosody": {
      "emotion": "caring",
      "emphasis_words": [0, 3, 7],
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

## Core Principles

### 1. 🔒 Security First

- **Declarative Specification**: Agents send data, not executable code
- **Pre-approved Resource Library**: Clients maintain trusted model and animation libraries
- **Content Filtering**: Automatically detect and block inappropriate content
- **Brand Control**: Clients have full control over avatar visual style

### 2. 🎯 Context-Aware

- **Emotional Intelligence**: Adjust expressions based on conversation emotion
- **Task Adaptation**: Select appropriate avatar styles based on task types
- **Cultural Sensitivity**: Support localized expressions and gestures
- **Accessibility**: Support subtitles, sign language, and other assistive features

### 3. 🌐 Cross-Platform Consistency

- **Framework-Agnostic**: Support Three.js, Unity, Unreal, ARKit, ARCore
- **Responsive**: From low-end mobile devices to high-end VR headsets
- **Progressive Enhancement**: Provide different quality levels based on device capabilities
- **Offline Support**: Cache common resources for offline rendering

### 4. ⚡ High Performance

- **Incremental Updates**: Only transmit changes
- **Priority System**: Critical actions rendered first
- **LOD Support**: Adjust detail based on distance and importance
- **Streaming**: Support streaming of animations and speech

### 5. 🤝 Multi-Agent Collaboration

- **Scene Management**: Multiple avatars in the same scene
- **Interaction Coordination**: Eye contact and interaction between agents
- **Role Distinction**: Clear visual distinction of different agents
- **Turn Taking**: Manage multi-agent dialogue with A2A protocol

## Technical Specification

### Core Design Principles

#### 1. Layered Message Architecture

ADA uses **message categorization** instead of a single state object, achieving:

- ✅ **Efficient incremental updates**: Only transmit changes (UPDATE)
- ✅ **Precise timing control**: Unified timeline and sync markers
- ✅ **Clear separation of concerns**: Init, update, action, speech each has distinct responsibilities
- ✅ **Multi-agent support**: Scene-level coordination and orchestration

#### 2. Resource Reference Specification

Using URI schemes ensures security and extensibility:

```
ada://models/humanoid/professional-v2      # ADA internal resources
ada://animations/gestures/wave             # ADA internal animations
a2ui://element/sleep-tip-card              # A2UI element reference
https://cdn.example.com/assets/custom.glb  # External resources
```

**Security Guarantees**:
- Clients maintain `ada://` resource whitelist
- External resources require explicit authorization
- Prevents code injection and malicious content

#### 3. Timing Synchronization System

```typescript
// Global timeline
interface Timeline {
  timestamp: number;           // Unix millisecond timestamp (baseline)
  start_offset_ms: number;     // Relative offset
  duration_ms: number;         // Duration
  sync_markers: SyncMarker[];  // Sync anchors
}

// Sync markers (aligned with speech, UI events)
interface SyncMarker {
  marker_id: string;           // e.g., "speech-segment-1"
  align_to_step: string;       // Animation step to align to
  offset_ms: number;           // Fine-tune offset
}
```

### State Model

ADA defines multi-layer avatar state:

```typescript
// Message base interface
interface ADAMessage {
  version: string;              // Protocol version (e.g., "1.0.0")
  type: MessageType;            // Message type
  timestamp: number;            // Unix millisecond timestamp
  agent_id: string;             // Agent unique ID
  session_id?: string;          // Session ID
  correlation_id?: string;      // A2A correlation ID
  priority?: 'low' | 'medium' | 'high' | 'critical';
}

// Avatar state (persistent layer)
interface AvatarAppearance {
  model: {
    asset_id: string;           // e.g., "ada://models/humanoid/v2"
    fallback: string[];         // Fallback options
    lod_levels: string[];       // Detail levels
  };
  appearance: {
    preset: string;             // Preset appearance
    overrides: object;          // Custom overrides
  };
}

// Expression state (transient layer, high-frequency updates)
interface ExpressionState {
  preset: string;               // Preset expression (e.g., "empathetic")
  blend: {                      // Fine-grained blending
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

// Action sequence (behavior layer)
interface ActionSequence {
  id: string;
  sequence: ActionStep[];
  sync_markers: SyncMarker[];
}

interface ActionStep {
  step_id: string;
  action_ref: string;           // URI: ada://animations/...
  parameters: object;           // Action parameters
  start_offset_ms: number;      // Relative start time
  duration_ms: number;
  trigger?: EventTrigger;       // Conditional trigger
}

// Speech synchronization
interface SpeechMessage {
  utterance_id: string;
  text: string;
  audio: {
    source: string;             // URL or data URI
    format: string;
    duration_ms: number;
  };
  phonemes: Phoneme[];          // Phoneme sequence (lip sync)
  prosody: {
    emotion: string;
    emphasis_words: number[];   // Emphasized word indices
    speed_multiplier: number;   // Speech rate
  };
  facial_animation: {
    lip_sync: 'auto' | 'manual';
    expression_overlay: object; // Overlay expressions
  };
}
```

### Renderer Interface

Clients implement the standard renderer interface:

```typescript
interface ADARenderer {
  // Initialize avatar
  initialize(message: InitMessage): Promise<Avatar>;

  // Handle messages (unified entry point)
  handleMessage(message: ADAMessage): Promise<void>;

  // Apply incremental state updates
  applyUpdates(updates: StateUpdate[]): Promise<void>;

  // Perform action sequence
  performAction(action: ActionSequence): Promise<void>;

  // Synchronize speech and animation
  syncSpeech(speech: SpeechMessage): Promise<void>;

  // Handle user interaction
  handleInteraction(event: InteractionEvent): void;

  // Resource management
  preloadResources(resources: ResourceManifest): Promise<void>;

  // Scene management (multi-agent)
  updateScene(scene: SceneUpdate): Promise<void>;
}
```

### Best Practices

#### 1. When to Use Which Message Type

```typescript
// ✅ Init: Session start or agent first appearance
{
  "type": "avatar.init",
  "avatar": {...},
  "initial_state": {...}
}

// ✅ State update: Transient changes like expressions, gaze
{
  "type": "avatar.update",
  "updates": [
    {"path": "expression.preset", "value": "happy"}
  ]
}

// ✅ Action sequence: Complex gestures, demonstrations
{
  "type": "avatar.action",
  "action": {
    "sequence": [...]
  }
}

// ✅ Speech output: Speech with lip sync and expressions
{
  "type": "avatar.speech",
  "speech": {
    "text": "...",
    "audio": {...}
  }
}

// ✅ Scene coordination: Multi-agent interaction
{
  "type": "scene.update",
  "scene": {
    "participants": [...],
    "interactions": [...]
  }
}
```

#### 2. Incremental Updates vs Full State

```typescript
// ❌ Inefficient: Send full state every time
{
  "type": "avatar.init",  // Wrong: shouldn't re-initialize
  "avatar": {...complete data...}
}

// ✅ Efficient: Only send changes
{
  "type": "avatar.update",
  "updates": [
    {"path": "expression.preset", "value": "surprised"}
  ]
}
```

#### 3. Timing Synchronization Patterns

```typescript
// Pattern 1: Absolute timing based on timestamp
{
  "timestamp": 1738329600000,  // Global baseline
  "action": {
    "sequence": [
      {"start_offset_ms": 0},    // Start immediately
      {"start_offset_ms": 500}   // Start after 500ms
    ]
  }
}

// Pattern 2: Relative sync based on markers (recommended for speech)
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

#### 4. Integration with A2UI

```typescript
// ADA gesture points to A2UI element
{
  "type": "avatar.action",
  "action": {
    "sequence": [
      {
        "action_ref": "ada://animations/gestures/point",
        "parameters": {
          // Reference A2UI element
          "target_ui": "a2ui://element/tip-card-1"
        }
      }
    ]
  }
}

// A2UI element event triggers ADA reaction
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

#### 5. Performance Optimization Strategies

```typescript
// Strategy 1: Resource preloading
{
  "type": "resource.preload",
  "resources": {
    "animations": [
      {
        "asset_id": "ada://animations/gestures/nod",
        "priority": "high",
        "preload_strategy": "eager"  // Load immediately
      }
    ]
  }
}

// Strategy 2: Priority system
{
  "type": "avatar.update",
  "priority": "critical",  // Facial expressions first
  "updates": [...]
}

// Strategy 3: LOD adaptive
{
  "avatar": {
    "model": {
      "lod_levels": ["high", "medium", "low"],
      "auto_select": true  // Client selects based on performance
    }
  }
}
```

#### 6. Error Handling and Fallback

```typescript
// Resource fallback
{
  "avatar": {
    "model": {
      "asset_id": "ada://models/custom/advanced",
      "fallback": [
        "ada://models/humanoid/standard",
        "ada://models/humanoid/basic",
        "ada://models/2d/sprite"  // Final fallback to 2D
      ]
    }
  }
}

// Capability detection
{
  "avatar": {
    "capabilities": {
      "expressions": ["basic"],     // Required
      "gestures": ["professional"], // Optional
      "voice": {"enabled": true}    // Mute if not supported
    }
  }
}
```

## Ecosystem Integration

### ADA + A2UI: Complete User Experience

```
┌────────────────────────────────────────┐
│         AI Agent                        │
├────────────────┬───────────────────────┤
│   ADA Generator│    A2UI Generator     │
│   (Avatar)     │    (UI)               │
└───────┬────────┴──────────┬────────────┘
        │                   │
        ▼                   ▼
┌───────────────────────────────────────┐
│         Client Application             │
│  ┌─────────────┐    ┌──────────────┐  │
│  │ ADA Renderer│    │  A2UI Renderer│ │
│  │  (Avatar)   │    │   (UI)       │  │
│  └─────────────┘    └──────────────┘  │
│                                       │
│  Unified branded user experience      │
└───────────────────────────────────────┘
```

**Example Scenario: Health Advisor Agent**
- **ADA**: Displays friendly doctor avatar, uses gestures to point to recommendations
- **A2UI**: Shows interactive health plan form
- **Synergy**: Avatar interacts with UI elements synchronously

### ADA + A2A: Multi-Agent Virtual Meeting

```
Scenario: Three agents collaborating to solve a problem

┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  Project    │  │  Technical  │  │  Designer   │
│  Manager    │  │  Expert     │  │             │
│  Agent      │  │  Agent      │  │  Agent      │
└──────┬──────┘  └──────┬──────┘  └──────┬──────┘
       │ ADA            │ ADA            │ ADA
       └────────────────┼────────────────┘
                        ▼
              ┌──────────────────┐
              │  Virtual Meeting │
              │  Three Avatars   │
              │  Discussing      │
              └──────────────────┘
```

### Supported Platforms and Frameworks

#### Currently Supported ✅
- **Web**: Three.js, Babylon.js
- **Mobile**: Unity (iOS/Android)
- **Framework**: React Three Fiber

#### Planned Support 🔜
- **VR/AR**: ARKit, ARCore, WebXR
- **Game Engines**: Unreal Engine
- **3D Tools**: Blender Plugin
- **Real-time Communication**: WebRTC Integration

## Real-World Use Cases

### 1. Customer Service Agent
**Scenario**: E-commerce platform AI customer service

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

**Benefits**:
- Increases customer trust
- Reduces communication barriers
- Enhances brand image

### 2. Educational Tutor Agent
**Scenario**: Online programming instructor

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

**Benefits**:
- Increases learning motivation
- Timely emotional feedback
- Better instructional effectiveness

### 3. Healthcare Assistant
**Scenario**: Mental health support

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

**Benefits**:
- Establishes sense of safety
- Promotes emotional expression
- Provides emotional support

### 4. Virtual Team Collaboration
**Scenario**: Multi-agent project meeting

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

**Benefits**:
- Clear role distinction
- Natural interaction flow
- Improved collaboration efficiency

## Quick Start

### Installation

```bash
# Clone repository
git clone https://github.com/your-org/ADA.git
cd ADA

# Install dependencies (Web example)
cd samples/web-threejs
npm install
npm run dev
```

### Basic Example (Client-side)

```javascript
import { ADARenderer } from 'ada-renderer-web';

// 1. Initialize renderer
const renderer = new ADARenderer({
  container: document.getElementById('avatar-container'),
  framework: 'threejs',
  quality: 'auto'  // Auto-select quality based on device
});

// 2. Handle initialization message
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

// 3. Incrementally update expression
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

// 4. Execute gesture animation
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

// 5. Play speech with synchronized animation
await renderer.handleMessage({
  type: 'avatar.speech',
  timestamp: Date.now(),
  agent_id: 'assistant-001',

  speech: {
    utterance_id: 'greeting-001',
    text: 'Hello! How can I help you?',
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

### Agent Integration (Server-side)

```python
from ada import ADAClient, MessageType
from your_agent import Agent

# Initialize
agent = Agent()
ada_client = ADAClient(
    agent_id="health-advisor-001",
    session_id="session-abc-123"
)

# Scenario 1: Agent first appearance
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

# Scenario 2: Process user input
user_input = "I've been feeling tired"
response = agent.process_input(user_input)

# Detected concerned emotion -> Update expression
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

# Scenario 3: Generate response and synchronize actions
speech_text = response.text  # "Sleep is important..."

# First send action sequence
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

# Then send speech message (auto-syncs with actions)
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
```

### Advanced Integration Patterns

#### Integration with A2A Protocol

```python
from ada import ADAClient
from a2a import A2AProtocol

# Multi-agent scenario: Three agents collaborating
a2a_session = A2AProtocol.create_session("team-meeting")

# Agent 1: Project Manager
pm_agent = ADAClient(agent_id="pm-001", session_id=a2a_session.id)
await pm_agent.send_init({...})

# Agent 2: Developer
dev_agent = ADAClient(agent_id="dev-002", session_id=a2a_session.id)
await dev_agent.send_init({...})

# Update scene: Project manager asks developer a question
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

## Comparison with Other Standards

### ADA vs. VRM/glTF
- **VRM/glTF**: Static 3D model format
- **ADA**: Dynamic, context-aware avatar control protocol
- **Relationship**: ADA can use VRM/glTF as underlying model format

### ADA vs. Traditional Animation Pipeline
- **Traditional**: Pre-made animations, manually created by designers
- **ADA**: AI-generated in real-time, context-driven
- **Relationship**: ADA can call pre-made animations as building blocks

### ADA vs. VTuber Technology
- **VTuber**: Human-driven, real-time facial capture
- **ADA**: AI-driven, autonomous expression and action decisions
- **Relationship**: Technologies can complement each other, ADA can integrate facial capture inputs

## Technical Principles

### Emotion Mapping System

ADA uses emotion computing models to map conversation context to avatar expressions:

```
Text sentiment analysis ──┐
                         ├──> Emotion vector ──> Facial expression parameters
Voice prosody analysis ──┘                             │
                                                       ├──> ADA State
Dialogue intent recognition ──┐                         │
                             ├──> Behavior decision ──> Body action sequence
User feedback analysis ──────┘
```

### Priority and Bandwidth Optimization

```typescript
// State update priority
enum UpdatePriority {
  CRITICAL = 0,   // Facial expressions (high-frequency updates)
  HIGH = 1,       // Gestures and posture
  MEDIUM = 2,     // Gaze and micro-expressions
  LOW = 3         // Environment and ambiance
}

// Smart differential updates
function createDeltaUpdate(
  previousState: ADAState,
  newState: ADAState
): ADADelta {
  // Only transmit changed parts
  return computeDelta(previousState, newState);
}
```

### Performance Metrics

Target performance standards:
- **Initial Load**: < 2s (standard model)
- **State Update Latency**: < 100ms
- **Animation Smoothness**: 60 FPS (desktop), 30 FPS (mobile)
- **Bandwidth Usage**: < 50 KB/s (ongoing conversation)

## Roadmap

### v1.0 (Current) ✅
- [x] Core specification definition
- [x] Web renderer (Three.js)
- [x] Basic emotion expression system
- [x] Python/JavaScript SDK
- [x] Sample applications

### v1.5 (Q2 2026) 🔜
- [ ] Unity/Unreal renderers
- [ ] Advanced animation blending
- [ ] Speech-animation auto-sync
- [ ] AR/VR support
- [ ] Performance optimization tools

### v2.0 (Q4 2026) 🎯
- [ ] Real-time facial motion capture integration
- [ ] Multi-agent scene orchestration
- [ ] Custom model training tools
- [ ] Enterprise deployment solutions
- [ ] Complete accessibility support

### Future Vision 🚀
- [ ] Holographic projection support
- [ ] Haptic feedback integration
- [ ] Neural interface exploration
- [ ] Metaverse platform integration

## Community & Ecosystem

### Partners

We're honored to collaborate with the following organizations:

**Technology Partners**
- 🌐 **A2UI Project**: Seamless UI + Avatar integration
- 🔗 **A2A Protocol**: Multi-agent communication standard
- 🎮 **Unity Technologies**: Game engine integration
- 🎨 **Ready Player Me**: Avatar resource library

**Industry Applications**
- 🏥 **Healthcare**: Psychological counseling, rehabilitation training
- 🎓 **Education**: Online teaching, skills training
- 🛍️ **E-commerce**: Virtual shopping guides, customer service
- 🏢 **Enterprise Collaboration**: Virtual meetings, remote collaboration

### Contributing

We welcome community contributions! You can:

1. **Develop Renderers**: Add support for your favorite platform/framework
2. **Create Resources**: Contribute avatar models and animations
3. **Improve Specification**: Propose enhancements and use cases
4. **Write Documentation**: Tutorials, examples, best practices
5. **Report Issues**: Bug reports and feature requests

```bash
# Fork the project
git clone https://github.com/your-org/ADA.git
cd ADA

# Create a feature branch
git checkout -b feature/your-feature

# Commit changes
git commit -m "Add: your feature description"

# Push and create PR
git push origin feature/your-feature
```

### License

ADA is licensed under **Apache 2.0**, free to use, modify, and distribute.

## JSON Format Complete Reference

For detailed message format specifications, see:
- 📘 [JSON Schema Definition](https://ada.dev/schemas/v1/message.json)
- 📄 [Format Design Analysis](./JSON_DESIGN_ANALYSIS.md)
- 🔍 [Interactive Format Explorer](https://ada.dev/schema-explorer)

### Message Type Quick Reference

| Message Type | Purpose | Frequency | Bandwidth | Example Scenario |
|-------------|---------|-----------|-----------|------------------|
| `avatar.init` | Initialize avatar | Once/session | High | Agent first appears |
| `avatar.update` | Incremental state update | Frequent | Low | Expression changes |
| `avatar.action` | Execute action sequence | Medium | Medium | Gestures, demonstrations |
| `avatar.speech` | Speech + lip sync | Medium | High | Voice output |
| `avatar.interaction` | User interaction response | On-demand | Low | Click feedback |
| `scene.update` | Scene management | Low | Medium | Multi-agent coordination |
| `resource.preload` | Resource preloading | Low | Medium | Performance optimization |
| `avatar.event` | Event notification | On-demand | Low | State callbacks |

### Field Constraint Specifications

```typescript
// Time-related
timestamp: number;          // Unix millisecond timestamp, >= 0
duration_ms: number;        // Milliseconds, 0-300000 (5 minutes)
start_offset_ms: number;    // Milliseconds, >= 0

// Numeric ranges
intensity: number;          // 0.0 - 1.0
attention_level: number;    // 0.0 - 1.0
speed_multiplier: number;   // 0.5 - 2.0
blend_values: number;       // 0.0 - 1.0

// String formats
agent_id: string;           // Regex: ^[a-zA-Z0-9-_]+$
asset_id: string;           // URI: ada://path/to/asset
version: string;            // Semantic version: X.Y.Z

// Enum values
priority: 'low' | 'medium' | 'high' | 'critical';
easing: 'linear' | 'ease-in' | 'ease-out' | 'ease-in-out';
```

### Common Errors and Solutions

#### ❌ Error 1: Confusing Message Types

```json
// Wrong: Using UPDATE for initialization
{
  "type": "avatar.update",
  "updates": [
    {"path": "avatar.model", "value": {...}}  // Too heavy
  ]
}

// Correct: Using INIT
{
  "type": "avatar.init",
  "avatar": {...}
}
```

#### ❌ Error 2: Inconsistent Timing

```json
// Wrong: No time baseline
{
  "action": {
    "sequence": [
      {"duration_ms": 500},  // When to start?
      {"duration_ms": 300}
    ]
  }
}

// Correct: Explicit timing
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

#### ❌ Error 3: Non-standard Resource References

```json
// Wrong: Bare string
{
  "action_ref": "nod"  // Unclear what resource this is
}

// Correct: URI reference
{
  "action_ref": "ada://animations/gestures/nod"
}
```

### Version Evolution Strategy

```json
// v1.0.0 (current)
{
  "version": "1.0.0",
  "type": "avatar.update",
  "updates": [...]
}

// v1.1.0 (backward compatible, new fields)
{
  "version": "1.1.0",
  "type": "avatar.update",
  "updates": [...],
  "batch_id": "optional-new-field"  // v1.0 clients will ignore
}

// v2.0.0 (breaking changes, incompatible)
{
  "version": "2.0.0",
  "type": "avatar.state.update",  // Type name changed
  "changes": [...]  // Field name changed
}
```

## Resources

- 📘 **Official Documentation**: https://ada.dev/docs
- 💻 **GitHub Repository**: https://github.com/your-org/ADA
- 🎮 **Online Demo**: https://ada.dev/playground
- 💬 **Community Forum**: https://community.ada.dev
- 📧 **Mailing List**: ada-dev@googlegroups.com
- 🐦 **Twitter**: @ADAOpenSource

## FAQ

### Q: What's the relationship between ADA and A2UI?
A: ADA and A2UI are complementary. A2UI handles interface elements, ADA handles avatars. They work together to provide a complete agent user experience.

### Q: Do I need 3D modeling experience?
A: No. ADA provides pre-made avatar libraries. Developers only need to configure via JSON.

### Q: Does it support custom avatars?
A: Yes. You can import custom models in VRM/glTF format and map them to ADA's control system.

### Q: What are the performance requirements?
A: ADA supports multiple quality levels. From low-end mobile devices (2D sprites) to high-end VR (full 3D), it runs smoothly.

### Q: How is content security ensured?
A: Clients have full control over resource libraries and rendering. Agents can only select from pre-approved libraries and cannot inject arbitrary content.

### Q: Is commercial use free?
A: Yes. ADA is fully open-source and free, including for commercial use.

## Get Started 🚀

Start exploring ADA today!

```bash
# Quick start
git clone https://github.com/your-org/ADA.git
cd ADA/samples/quickstart
npm install && npm start

# View in browser
# http://localhost:3000
```

Visit our [Interactive Tutorial](https://ada.dev/tutorial) to build your first intelligent avatar in 10 minutes!

**Recommended Learning Path**:
1. 📖 Read [JSON Format Design Analysis](./JSON_DESIGN_ANALYSIS.md)
2. 🎮 Try [Online Demo](https://ada.dev/playground)
3. 💻 Clone repository and run examples
4. 🛠️ Integrate into your agent project

---

## Contact Us

Have questions or suggestions? We'd love to hear from you!

- 💡 Feature Requests: [GitHub Issues](https://github.com/your-org/ADA/issues)
- 🐛 Bug Reports: [GitHub Issues](https://github.com/your-org/ADA/issues)
- 💬 General Discussion: [Discussions](https://github.com/your-org/ADA/discussions)
- 📧 Business Partnerships: ada-partnerships@example.com

---

**Let's build a more human AI future together!** 🤖❤️

---

*ADA Project - Agent Driven Avatar*
*Driven by the open-source community, for better human-computer interaction*

**Version**: 1.0.0
**Updated**: 2026-01-31
**License**: Apache 2.0
