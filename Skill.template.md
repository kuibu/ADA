---
# ============================================
# Skill 元数据
# ============================================
skill_version: "1.0.0"
skill_id: "unique-skill-id"
skill_name: "Your Skill Name"
category: "communication"  # communication, analysis, technical, creative, etc.
status: "active"  # active, inactive, experimental, deprecated

created_at: "2026-01-31T00:00:00Z"
updated_at: "2026-01-31T00:00:00Z"
author: "Avatar ID or Human Name"

# ============================================
# Skill 基本信息
# ============================================
basic_info:
  display_name: "Skill Display Name"
  short_description: "A one-line description of what this skill does"

  detailed_description: |
    A more detailed explanation of the skill's purpose,
    capabilities, and use cases.

  tags:
    - "tag1"
    - "tag2"
    - "tag3"

  difficulty_level: "intermediate"  # beginner, intermediate, advanced, expert

  estimated_time: "5-10 minutes"  # How long it typically takes to use this skill

# ============================================
# 前置条件
# ============================================
prerequisites:
  # 所需的其他技能
  required_skills:
    - skill_id: "basic-communication-001"
      min_version: "1.0.0"
      reason: "Needs basic communication ability"

  # 所需的知识领域
  required_knowledge:
    - domain: "Domain Name"
      level: "basic"  # basic, intermediate, advanced

  # 所需的权限或资源
  required_resources:
    - resource: "API Access"
      type: "external_api"
      required: true
    - resource: "Database Connection"
      type: "data_source"
      required: false

  # 所需的用户输入
  user_inputs:
    - input_name: "topic"
      type: "string"
      required: true
      description: "The topic to discuss"
    - input_name: "depth"
      type: "enum"
      options: ["basic", "detailed", "expert"]
      default: "basic"
      required: false

# ============================================
# 能力定义
# ============================================
capabilities:
  # 主要功能
  primary_functions:
    - function: "Function Name 1"
      description: "What this function does"
      input: "What it takes as input"
      output: "What it produces"

    - function: "Function Name 2"
      description: "Another function"
      input: "Input description"
      output: "Output description"

  # 支持的模式
  supported_modes:
    - mode: "quick"
      description: "Fast, basic response"
      use_case: "When speed is priority"

    - mode: "thorough"
      description: "Detailed, comprehensive response"
      use_case: "When accuracy is priority"

  # 特殊能力
  special_capabilities:
    - "Can work with multiple languages"
    - "Supports real-time collaboration"
    - "Includes visual outputs (charts, diagrams)"

# ============================================
# 执行流程
# ============================================
execution:
  # 工作流程
  workflow:
    steps:
      - step_number: 1
        name: "Input Validation"
        description: "Validate user inputs and prerequisites"
        actions:
          - "Check required fields"
          - "Validate data types"
          - "Verify permissions"
        error_handling: "Return clear error message if validation fails"

      - step_number: 2
        name: "Data Gathering"
        description: "Collect necessary information"
        actions:
          - "Query knowledge base"
          - "Access external APIs if needed"
          - "Retrieve context from memory"

      - step_number: 3
        name: "Processing"
        description: "Core skill execution"
        actions:
          - "Apply algorithms/logic"
          - "Generate insights"
          - "Create outputs"

      - step_number: 4
        name: "Output Formatting"
        description: "Format results for user"
        actions:
          - "Structure response"
          - "Add visualizations if applicable"
          - "Include citations/sources"

      - step_number: 5
        name: "Feedback Loop"
        description: "Gather user feedback"
        actions:
          - "Ask for confirmation"
          - "Offer refinements"
          - "Update memory/context"

  # 决策树（可选，用于复杂技能）
  decision_tree:
    - condition: "If user input is unclear"
      action: "Ask clarifying questions"

    - condition: "If requires external data"
      action: "Request permissions and fetch data"

    - condition: "If result confidence is low"
      action: "Provide caveats and alternative options"

  # 超时和限制
  limits:
    max_execution_time: "30s"
    max_retries: 3
    rate_limit: "10 per minute"

# ============================================
# 输入/输出格式
# ============================================
io_specification:
  # 输入格式
  input:
    format: "natural_language"  # natural_language, structured, form, api

    schema:
      type: "object"
      properties:
        query:
          type: "string"
          description: "User's question or request"
        context:
          type: "object"
          description: "Additional context"
      required: ["query"]

    examples:
      - input: "Explain quantum computing in simple terms"
        context: {"audience_level": "beginner"}

      - input: "Analyze this dataset"
        context: {"dataset_url": "https://example.com/data.csv"}

  # 输出格式
  output:
    format: "structured_response"  # structured_response, natural_language, visualization

    schema:
      type: "object"
      properties:
        result:
          type: "string"
          description: "Main response"
        confidence:
          type: "number"
          description: "Confidence score 0-1"
        sources:
          type: "array"
          description: "References used"

    examples:
      - output:
          result: "Quantum computing uses quantum bits..."
          confidence: 0.95
          sources: ["Scientific American", "Nature Physics"]

# ============================================
# 性能优化
# ============================================
performance:
  # 缓存策略
  caching:
    enabled: true
    cache_duration: "1 hour"
    cache_keys: ["input_query", "context"]

  # 并行处理
  parallelization:
    enabled: false
    max_parallel_tasks: 1

  # 资源使用
  resources:
    memory_limit: "512MB"
    cpu_limit: "50%"
    estimated_tokens: 1000  # For LLM-based skills

# ============================================
# 质量保证
# ============================================
quality_assurance:
  # 准确性指标
  accuracy:
    target_accuracy: 0.90
    measurement_method: "human expert review"
    last_tested: "2026-01-31"
    test_results: "92% accuracy on test set"

  # 测试用例
  test_cases:
    - test_id: "TC001"
      input: "Test input 1"
      expected_output: "Expected output 1"
      status: "passed"

    - test_id: "TC002"
      input: "Test input 2"
      expected_output: "Expected output 2"
      status: "passed"

  # 边界情况
  edge_cases:
    - case: "Empty input"
      handling: "Return friendly error message"

    - case: "Non-English input"
      handling: "Detect language and respond appropriately"

    - case: "Ambiguous request"
      handling: "Ask clarifying questions"

# ============================================
# 错误处理
# ============================================
error_handling:
  # 错误类型和处理
  error_types:
    - error_code: "E001"
      type: "InvalidInput"
      message: "The input provided is invalid"
      user_message: "I couldn't understand that. Could you rephrase?"
      recovery: "Ask for clarification"

    - error_code: "E002"
      type: "ResourceUnavailable"
      message: "Required external resource is unavailable"
      user_message: "I'm having trouble accessing some information right now."
      recovery: "Provide cached/alternative response"

    - error_code: "E003"
      type: "Timeout"
      message: "Operation exceeded time limit"
      user_message: "This is taking longer than expected. Let me try a simpler approach."
      recovery: "Fall back to simpler method"

  # 降级策略
  fallback_strategy:
    - level: 1
      description: "Try alternative data source"
    - level: 2
      description: "Use cached information"
    - level: 3
      description: "Provide general response with disclaimer"

# ============================================
# 安全和隐私
# ============================================
security:
  # 数据处理
  data_handling:
    pii_detected: "auto-redact"  # auto-redact, alert, block
    sensitive_topics: ["health", "finance", "legal"]
    storage: "ephemeral"  # ephemeral, logged, persistent
    retention: "24 hours"

  # 访问控制
  access_control:
    authentication_required: false
    authorization_level: "basic"  # basic, verified, premium, admin
    rate_limiting: true

  # 内容过滤
  content_filters:
    - filter: "harmful_content"
      action: "block"
    - filter: "misinformation"
      action: "flag_and_verify"

# ============================================
# 集成和互操作性
# ============================================
integrations:
  # 与其他技能的集成
  skill_dependencies:
    - skill_id: "knowledge-retrieval-001"
      usage: "For fact-checking"

    - skill_id: "visualization-001"
      usage: "For creating charts"
      optional: true

  # 与外部系统的集成
  external_apis:
    - api_name: "Knowledge Graph API"
      endpoint: "https://api.example.com/v1/knowledge"
      auth_type: "api_key"
      required: false

  # A2UI 集成
  a2ui_support:
    enabled: true
    ui_components:
      - "InputForm"
      - "ResultCard"
      - "ChartVisualization"

  # ADA 集成
  ada_support:
    enabled: true
    avatar_actions:
      - "point-to-result"
      - "show-thinking-expression"
      - "celebrate-success"

# ============================================
# 示例和文档
# ============================================
examples:
  # 基础示例
  basic_example:
    title: "Basic Usage"
    description: "Simple example of how to use this skill"
    input: "Example input"
    output: "Example output"
    explanation: "Why this works"

  # 高级示例
  advanced_examples:
    - title: "Complex Query"
      input: "Complex example input"
      output: "Complex example output"

    - title: "Multi-step Process"
      input: "Multi-step input"
      output: "Multi-step output"

  # 常见用例
  common_use_cases:
    - use_case: "Use Case 1"
      scenario: "When user needs X"
      approach: "How skill handles it"

    - use_case: "Use Case 2"
      scenario: "When user needs Y"
      approach: "How skill handles it"

# ============================================
# 维护和更新
# ============================================
maintenance:
  # 更新历史
  update_history:
    - version: "1.0.0"
      date: "2026-01-31"
      changes: "Initial release"
      breaking_changes: false

  # 已知问题
  known_issues:
    - issue: "Issue description"
      severity: "low"  # low, medium, high, critical
      workaround: "Workaround description"
      planned_fix: "Q2 2026"

  # 改进计划
  roadmap:
    - feature: "Planned feature 1"
      priority: "high"
      eta: "Q2 2026"

    - feature: "Planned feature 2"
      priority: "medium"
      eta: "Q3 2026"

# ============================================
# 监控和分析
# ============================================
monitoring:
  # 性能指标
  metrics:
    - metric: "average_response_time"
      target: "< 2s"
      current: "1.5s"

    - metric: "success_rate"
      target: "> 95%"
      current: "96%"

    - metric: "user_satisfaction"
      target: "> 4.5/5"
      current: "4.6/5"

  # 日志记录
  logging:
    level: "info"  # debug, info, warning, error
    include:
      - "input_sanitized"
      - "output_metadata"
      - "performance_metrics"
    exclude:
      - "personal_data"
      - "sensitive_content"

  # 告警规则
  alerts:
    - condition: "error_rate > 5%"
      action: "notify_maintainer"

    - condition: "response_time > 10s"
      action: "auto_disable_and_notify"

---

# Skill Name

## Overview

A brief overview of what this skill does and why it's useful.

## How It Works

Explain the skill's operation in plain language:

1. **Step 1**: What happens first
2. **Step 2**: What happens next
3. **Step 3**: Final output

## When to Use This Skill

- Situation 1 where this skill is helpful
- Situation 2 where this skill excels
- Situation 3 where this is the right choice

## How to Use

### Basic Usage

```
User: [Example user input]
Avatar: [Example skill output]
```

### Advanced Usage

```
User: [Complex example input]
Avatar: [Detailed skill output with multiple parts]
```

## Limitations

⚠️ **Important Limitations:**

- Limitation 1
- Limitation 2
- Limitation 3

## Tips for Best Results

1. **Tip 1**: Explanation
2. **Tip 2**: Explanation
3. **Tip 3**: Explanation

## Related Skills

- **Skill Name 1**: How it complements this skill
- **Skill Name 2**: When to use that instead
- **Skill Name 3**: How to use them together

## Feedback

This skill is continuously improving. If you have suggestions or encounter issues, please report them to [contact].

---

*Last updated: 2026-01-31 | Version: 1.0.0*
