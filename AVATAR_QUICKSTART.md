# Avatar.md 快速开始指南

> **5 分钟创建你的第一个数字人 Avatar**

## 🎯 什么是 Avatar.md?

Avatar.md 是一个标准化的数字人配置文件，定义了 AI Agent 的：
- 🎭 **外观**：照片、3D 模型、表情
- 🧠 **知识**：专业领域、知识来源
- 💪 **技能**：模块化能力系统
- 👥 **关系**：与其他 Avatar 和人类的协作
- 🎬 **人格**：目标、价值观、性格特征
- 🏢 **透明公司**：人-AI 共同驾驶的商业实体

## 🚀 快速开始

### 1. 复制模板

```bash
cp Avatar.template.md my-avatar.md
```

### 2. 编辑基本信息

```yaml
basic_info:
  display_name: "Dr. Emma Chen"  # 显示名称
  age: 35                        # 虚拟年龄
  occupation: "AI Health Advisor" # 职业
  languages:
    - language: "English"
      proficiency: "native"
```

### 3. 配置外观

```yaml
appearance:
  ada_model:
    base_model: "ada://models/humanoid/professional-female-v2"
    style: "realistic-professional"

  visual_assets:
    photos:
      - url: "./assets/portrait.jpg"
        angle: "front"
    models_3d:
      - url: "./assets/model.vrm"
        format: "VRM"
```

### 4. 定义人格

```yaml
character:
  goals:
    primary: "帮助人们改善健康"

  personality:
    traits:
      positive: ["温暖", "专业", "善解人意"]

  interests:
    professional: ["AI 伦理", "医疗健康"]
    personal: ["古典音乐", "徒步"]
```

### 5. 添加技能

```yaml
skills:
  skill_files:
    - path: "./skills/consultation.skill.md"
      name: "健康咨询"
      enabled: true
```

### 6. 配置透明公司（可选但推荐）

```yaml
transparent_company:
  company_info:
    legal_name: "Emma Health AI Inc."
    mission: "让健康指导人人可及"

  driver_configuration:
    primary_driver:
      type: "avatar"
      decision_authority: 0.70  # Avatar 占 70% 决策权

    co_pilots:
      - name: "Dr. Michael Zhang"
        role: "人类副驾驶"
        decision_authority: 0.30
```

## 📋 核心部分解释

### 1. 形象配置 (Appearance)

**目的**：定义 Avatar 的视觉呈现

```yaml
appearance:
  # ADA 渲染模型
  ada_model:
    base_model: "ada://models/..."  # 基础 3D 模型
    style: "professional"            # 风格主题

  # 视觉资源
  visual_assets:
    photos: [...]      # 2D 照片（多角度）
    models_3d: [...]   # 3D 模型（VRM/glTF）
    expressions: [...]  # 表情集

  # 外观描述
  physical_traits:
    height: "165cm"
    hair_color: "dark brown"
```

**最佳实践**：
- ✅ 提供多角度照片（正面、侧面、全身）
- ✅ 使用标准 3D 格式（VRM 或 glTF）
- ✅ 准备常用表情的预览图

### 2. 知识库 (Knowledge)

**目的**：定义 Avatar 的专业知识范围

```yaml
knowledge:
  # 专业领域
  expertise_domains:
    - domain: "AI & Machine Learning"
      level: "expert"
      sub_areas: ["Healthcare AI", "NLP"]

  # 知识来源
  knowledge_sources:
    - type: "academic_papers"
      update_frequency: "weekly"

  # 明确限制
  knowledge_limitations:
    - "不提供具体药物处方"
    - "不诊断严重医疗状况"
```

**关键点**：
- ⚠️ **明确限制很重要**：告诉用户 Avatar 不能做什么
- 📚 **知识需要更新**：设定更新频率和机制
- 🔍 **可追溯性**：记录知识来源

### 3. 技能系统 (Skills)

**目的**：模块化的能力定义

```yaml
skills:
  # 引用外部技能文件
  skill_files:
    - path: "./skills/medical_consultation.skill.md"
      name: "医疗咨询"
      category: "healthcare"
      enabled: true

  # 核心能力总结
  core_capabilities:
    communication: ["清晰解释", "多语言"]
    analysis: ["数据解读", "风险评估"]
```

**优势**：
- 🔌 **模块化**：技能可独立开发和测试
- 🔄 **可重用**：不同 Avatar 可共享技能
- 🎚️ **可控制**：随时启用/禁用特定技能

### 4. 人物设定 (Character)

**目的**：赋予 Avatar 独特的人格

```yaml
character:
  # 目标驱动
  goals:
    primary: "主要目标"
    secondary: ["次要目标1", "次要目标2"]

  # 价值观
  values:
    core: ["以人为本", "科学严谨", "同理心"]

  # 性格特征
  personality:
    traits:
      positive: ["温暖", "专业", "好奇"]
      areas_for_growth: ["有时过于追求完美"]

    quirks: ["喜欢用比喻", "会用手势强调"]

  # 背景故事
  backstory: |
    Emma 在一个医生家庭长大...
```

**为什么重要**：
- 🎭 **人格化**：让 Avatar 更真实、可信
- 🎯 **一致性**：指导 Avatar 的行为决策
- 💬 **对话质量**：影响互动的自然度

### 5. 社会关系 (Relationships)

**目的**：定义 Avatar 的社交网络

```yaml
relationships:
  # 其他 Avatar
  peer_avatars:
    - avatar_id: "dr-wilson-002"
      relationship: "colleague"
      collaboration_areas: ["心脏健康咨询"]

  # 人类协作者
  human_collaborators:
    - name: "Dr. Robert Lee"
      role: "医疗顾问"
      relationship: "复杂案例监督"

  # 组织关系
  organizational_affiliations:
    - organization: "全球健康 AI 联盟"
      role: "贡献成员"
```

**应用场景**：
- 🤝 **转介**：将用户转介给更专业的 Avatar
- 👥 **协作**：多个 Avatar 共同解决问题
- 🏛️ **信任背书**：组织关系增加可信度

### 6. 透明公司 (Transparent Company)

**目的**：建立可信的商业模式

```yaml
transparent_company:
  # Avatar 主驾驶 + 人类副驾驶
  driver_configuration:
    primary_driver:
      type: "avatar"
      decision_authority: 0.70
      constraints:
        - "重大财务决策需人类批准 (>$10,000)"

    co_pilots:
      - name: "Michael Zhang"
        decision_authority: 0.30
        responsibilities: ["财务监督", "法律合规"]

  # 完全透明
  transparency:
    financial_transparency:
      public_dashboard: "https://transparency.company.com"
      update_frequency: "monthly"

    decision_transparency:
      public_log: "所有重大决策公开"
```

**核心原则**：
1. **人-AI 共同驾驶**：Avatar 不能完全自主运营
2. **决策透明**：所有重大决策公开记录
3. **财务透明**：收入和支出定期公开
4. **公众监督**：社区可以提供反馈和建议

## 🎨 创建流程

### 阶段 1: 概念设计 (1-2 天)

1. **确定目标受众**：谁会使用这个 Avatar？
2. **定义核心价值**：它解决什么问题？
3. **设计人格原型**：性格、背景、动机
4. **规划技能集**：需要哪些能力？

### 阶段 2: 内容创建 (3-5 天)

1. **视觉资产**：
   - 拍摄/生成多角度照片
   - 创建或购买 3D 模型
   - 设计表情集

2. **知识库构建**：
   - 收集专业资料
   - 建立知识图谱
   - 定义限制边界

3. **技能开发**：
   - 编写 Skill.md 文件
   - 实现核心能力
   - 测试和优化

4. **人格细化**：
   - 撰写背景故事
   - 定义对话风格
   - 设计互动模式

### 阶段 3: 集成和测试 (2-3 天)

1. **填写 Avatar.md**
2. **ADA 系统集成**
3. **功能测试**
4. **用户测试**

### 阶段 4: 部署和运营 (持续)

1. **发布到生产环境**
2. **监控性能指标**
3. **收集用户反馈**
4. **持续迭代改进**

## 💡 实用技巧

### ✅ 推荐做法

1. **从模板开始**：不要从零开始，使用 `Avatar.template.md`
2. **渐进式完善**：先完成基础部分，再添加高级功能
3. **版本控制**：使用 Git 跟踪所有变更
4. **人类监督**：即使是高度自主的 Avatar，也要有人类副驾驶
5. **透明优先**：公开决策过程和限制
6. **测试驱动**：编写测试用例验证技能

### ❌ 常见错误

1. **过度承诺**：声称能做的比实际能力更多
2. **忽视限制**：不明确说明 Avatar 不能做什么
3. **缺少人类监督**：完全自动化关键决策
4. **不透明运营**：隐藏决策过程和财务
5. **孤立开发**：不与用户和社区互动
6. **静态配置**：从不更新知识和技能

## 📊 示例：三种类型的 Avatar

### 1. 专业顾问型（如 Dr. Emma Chen）

**特点**：
- 深度专业知识
- 高度负责任
- 必须有人类监督
- 透明公司模式

**适用场景**：医疗、法律、金融咨询

### 2. 教育导师型

**特点**：
- 教学能力强
- 耐心友好
- 个性化学习路径
- 进度追踪

**适用场景**：在线教育、技能培训

### 3. 创意伙伴型

**特点**：
- 创造力激发
- 协作式互动
- 开放探索
- 灵活调整

**适用场景**：艺术创作、头脑风暴、内容生成

## 🔗 相关资源

### 核心文档
- 📘 [AVATAR_SPEC.md](./AVATAR_SPEC.md) - 完整规范和示例
- 📄 [Avatar.template.md](./Avatar.template.md) - 可直接使用的模板
- 🔧 [Skill.template.md](./Skill.template.md) - 技能定义模板

### ADA 生态
- 🎭 [ADA Protocol](./README.md) - Avatar 渲染和交互协议
- 🎨 [A2UI](https://a2ui.org) - 动态 UI 生成协议
- 🤝 [A2A](https://a2a.org) - 多代理通信协议

### 工具推荐
- **3D 建模**：Blender, Ready Player Me
- **照片处理**：Adobe Photoshop, GIMP
- **配置编辑**：VSCode + YAML extension
- **版本控制**：Git + GitHub
- **测试**：自定义测试框架

## ❓ 常见问题

### Q: Avatar.md 必须包含所有章节吗？
A: 不必须。根据你的 Avatar 类型，可以省略不相关的部分。但建议至少包含：基本信息、外观、人格、技能。

### Q: 如何处理多个 Avatar 之间的协作？
A: 在 `relationships.peer_avatars` 中定义关系，使用 A2A 协议实现通信。

### Q: 透明公司是必须的吗？
A: 不是必须，但强烈推荐。尤其是涉及重要决策（医疗、金融、法律）的 Avatar。

### Q: 如何更新 Avatar 的知识？
A: 在 `knowledge.knowledge_updates` 中定义更新机制，定期审查和更新知识库。

### Q: 3D 模型必须用 VRM 格式吗？
A: VRM 是推荐格式，但也支持 glTF。关键是要与 ADA 渲染器兼容。

### Q: 如何确保 Avatar 的伦理性？
A: 填写 `ethics_and_safety` 部分，定期审计，有人类监督，透明运营。

## 🚀 下一步

1. **复制模板**：`cp Avatar.template.md your-avatar.md`
2. **填写基本信息**：从简单的开始
3. **配置外观**：准备视觉资产
4. **定义技能**：编写 Skill.md 文件
5. **测试和迭代**：逐步完善
6. **发布和监控**：持续改进

## 📞 获取帮助

- 💬 [GitHub Discussions](https://github.com/kuibu/ADA/discussions)
- 📧 Email: ada-dev@googlegroups.com
- 📚 [完整文档](./AVATAR_SPEC.md)

---

**准备好创建你的第一个数字人了吗？** 🎉

从 `Avatar.template.md` 开始，构建一个独特、可信、有价值的 AI Avatar！

---

*ADA 项目 | Avatar.md 规范 v1.0.0*
*最后更新：2026-01-31*
