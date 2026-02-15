# skill-dfyx_code_security_review
东方隐侠团队出品，代码审计skill

# dfyx_code_security_review

> 基于深度数据流分析和业务逻辑理解的专家级代码安全审计 Skill

## 简介

**dfyx_code_security_review** 是为 Claude Code、Trae 等 AI 客户端设计的专业代码安全审计 Skill。采用白盒静态分析方法论，通过五阶段标准化审计协议，系统性发现和验证源代码中的安全漏洞。

### 核心能力

- **9 种语言**: Java, Python, Go, PHP, JavaScript/Node.js, C/C++, .NET/C#, Ruby, Rust
- **10 个安全维度**: 注入、认证、授权、反序列化、文件操作、SSRF、加密、配置、业务逻辑、供应链
- **双轨审计模型**: Sink-driven（注入/RCE）+ Control-driven（授权/业务逻辑）+ Config-driven（配置/加密）
- **五阶段审计协议**: 侦察 → 模式匹配 → 污点追踪 → 验证 → 报告
- **丰富案例库**: 基于 WooYun 真实漏洞案例（2010-2016）

---

## 快速开始

### 安装

将本 Skill 复制到 AI 客户端的 skills 目录：

```bash
# Claude Code
cp -r dfyx_code_security_review ~/.claude/skills/

# Trae
# 将文件夹复制到 Trae 的 skills 目录（具体路径取决于 Trae 版本）
```

### 触发方式

在 AI 客户端中，通过以下方式触发审计：

```
"审计这个项目"
"检查代码安全"
"找出安全漏洞"
"/audit" 或 "/code-audit"
```

### 使用示例

**用户**: 
```
请审计 /path/to/project 项目的代码安全
```

**AI 响应**:
```
[MODE] deep
[RECON] 识别技术栈: PHP + MySQL + ProcessWire
[PLAN] 启动 3 个 Agent, D1-D10 覆盖
[SCOPE] 预估 50 turns, 发现 5-10 个漏洞
确认开始审计? (yes/no)
```

---

## 文档结构

```
dfyx_code_security_review/
├── SKILL.md                          # 核心技能文档（入口文件）
├── README.md                         # 本文件
├── requirements.txt                  # Python 依赖
├── resources/                        # 核心资源
│   ├── knowledge/                    # 知识库（13个文档）
│   │   ├── architecture_analysis.md      # Phase 1: 架构分析
│   │   ├── pattern_scanning.md           # Phase 2: 模式扫描
│   │   ├── data_flow_analysis.md         # Phase 3: 数据流分析
│   │   ├── taint_analysis_enhanced.md    # 增强污点分析
│   │   ├── vulnerability_validation.md   # Phase 4: 漏洞验证
│   │   ├── attack_chain_analysis.md      # 攻击链分析
│   │   ├── reporting.md                  # Phase 5: 报告生成
│   │   ├── secret_detection.md           # 敏感信息检测
│   │   ├── dependency_analysis.md        # 依赖分析
│   │   ├── anti_hallucination.md         # 防幻觉机制
│   │   ├── docker_verification.md        # Docker 验证
│   │   ├── phase2_deep_methodology.md    # Phase 2 深度方法论
│   │   └── security_controls_matrix.yaml # 安全控制矩阵
│   ├── checklists/                   # 检查清单
│   │   ├── architecture_level_checklist.md
│   │   ├── code_level_checklist.md
│   │   └── coverage_matrix.md
│   ├── rules/                        # 漏洞检测规则
│   │   ├── command_injection_rules.md
│   │   └── sql_injection_rules.md
│   ├── wooyun/                       # WooYun 漏洞案例库
│   │   └── wooyun_cases_by_vulnerability_type.md
│   ├── tools/                        # 工具说明
│   │   └── security_tools.md
│   └── compliance/                   # 合规框架
│       └── compliance_frameworks.md
├── scripts/                          # 辅助脚本（7个）
│   ├── code_scan.py                  # 主扫描入口
│   ├── pattern_scanner.py            # 模式扫描
│   ├── data_flow_analyzer.py         # 数据流分析
│   ├── secret_finder.py              # 敏感信息检测
│   ├── dependency_analyzer.py        # 依赖分析
│   ├── vulnerability_validator.py    # 漏洞验证
│   └── report_generator.py           # 报告生成
├── templates/                        # 报告模板
│   ├── report-templates/
│   │   └── web-app-report-template.md    # Web应用报告模板
│   ├── architecture_diagram_templates.md
│   └── reproduction_steps_template.md
└── examples/                         # 案例与示例
    ├── audit_examples.md             # 审计示例
    ├── vulnerability_cases.md        # 漏洞案例库
    ├── vulnerability_analysis.md     # 漏洞分析方法
    ├── detailed_vulnerability_chains.md  # 攻击链与POC
    ├── config_file_vulnerabilities.md    # 配置文件漏洞
    └── environment_simulation.md     # 环境模拟
```

---

## 核心概念

### 五阶段标准化审计协议

```
Phase 1: 侦察与绘图 (10%)
    ↓ 产出: 架构图、攻击面清单
Phase 2: 并行模式匹配 (30%)
    ↓ 产出: 高风险区域清单
Phase 3: 深度污点追踪与实际测试验证 (40%)
    ↓ 产出: 确认的漏洞清单、实际测试验证报告
Phase 4: 验证与攻击链构建 (15%)
    ↓ 产出: 漏洞验证报告
Phase 5: 结构化报告 (5%)
    ↓ 产出: 完整审计报告
```

### 双轨审计模型

| 轨道 | 维度 | 方法 | 发现目标 |
|------|------|------|----------|
| **Sink-driven** | D1 注入、D4 反序列化、D5 文件、D6 SSRF | Grep 危险函数 → 追踪数据流 → 验证无防护 | 存在的危险代码 |
| **Control-driven** | D3 授权、D9 业务逻辑 | 枚举端点 → 验证安全控制是否存在 → 缺失=漏洞 | 缺失的安全控制 |
| **Config-driven** | D2 认证、D7 加密、D8 配置、D10 供应链 | 搜索配置 → 对比安全基线 | 错误配置 |

### 10 个安全维度

| # | 维度 | 覆盖内容 |
|---|------|---------|
| D1 | 注入 | SQL/Cmd/LDAP/SSTI/SpEL/JNDI |
| D2 | 认证 | Token/Session/JWT/Filter 链 |
| D3 | 授权 | CRUD 权限一致性、IDOR、水平越权 |
| D4 | 反序列化 | Java/Python/PHP Gadget 链 |
| D5 | 文件操作 | 上传/下载/路径遍历 |
| D6 | SSRF | URL 注入、协议限制 |
| D7 | 加密 | 密钥管理、加密模式、KDF |
| D8 | 配置 | Actuator、CORS、错误信息暴露 |
| D9 | 业务逻辑 | 竞态条件、Mass Assignment、状态机、多租户隔离 |
| D10 | 供应链 | 依赖 CVE、版本检查 |

---

## 扫描模式

| 模式 | 适用场景 | 范围 | 时间 |
|------|----------|------|------|
| **Quick** | CI/CD、小项目 | 高危漏洞、敏感信息、依赖 CVE | 5-10 min |
| **Standard** | 常规审计 | OWASP Top 10、认证授权、加密 | 30-60 min |
| **Deep** | 重要项目、渗透测试准备 | 全覆盖、攻击链、业务逻辑 | 1-3 hours |

---

## 使用流程

### 1. 启动审计

在 AI 客户端中输入：
```
请审计 /path/to/project 项目的代码安全
```

### 2. 确认审计计划

AI 会输出审计计划，确认后开始：
```
[MODE] deep
[RECON] 874 文件, Spring Boot 1.5 + Shiro 1.6 + JPA + Freemarker
[PLAN] 5 个 Agent, D1-D10 覆盖, 预估 125 turns
... (用户确认) ...
[REPORT] 10 Critical, 14 High, 12 Medium, 4 Low
```

### 3. 查看报告

审计完成后，AI 会生成：
- **安全审计报告** (Markdown)
- **漏洞详情**（包含代码片段、数据流图、修复建议）
- **攻击链分析**（漏洞组合利用场景）

---

## 辅助脚本（可选）

虽然 Skill 主要通过 AI 客户端使用，但也提供了 Python 辅助脚本：

```bash
# 安装依赖
pip install -r requirements.txt

# 代码扫描
python scripts/code_scan.py /path/to/project

# 生成报告
python scripts/report_generator.py --input results.json --output report.md
```

---

## 报告模板

提供标准化的报告模板：

- **快速版**: 适合日常审计、CI/CD集成
- **完整版**: 适合深度审计、渗透测试准备

模板位置：`templates/report-templates/web-app-report-template.md`

---

## 最佳实践

### 审计前准备
- 确保代码完整可访问
- 了解项目基本功能
- 确认审计范围

### 审计中配合
- 及时回答 AI 的澄清问题
- 提供额外的上下文信息
- 确认关键发现

### 审计后跟进
- 验证修复方案可行性
- 安排漏洞修复优先级
- 建立定期审计机制

---

## 常见问题

### Q1: 这个 Skill 和传统的 SAST 工具有什么区别？

**A**: 传统 SAST 工具依赖预定义规则，误报率高。本 Skill 结合 AI 的推理能力和专家知识库，能够：
- 理解业务逻辑，发现规则无法覆盖的漏洞
- 追踪复杂的数据流，识别多阶段攻击
- 提供具体可操作的修复建议

### Q2: 支持哪些编程语言和框架？

**A**: 支持 9 种语言和 14 种框架：
- **语言**: Java, Python, Go, PHP, JavaScript/Node.js, C/C++, .NET/C#, Ruby, Rust
- **框架**: Spring Boot, Django, Flask, FastAPI, Express, Koa, Gin, Laravel, Rails, ASP.NET Core, Rust Web, NestJS/Fastify, MyBatis

### Q3: 审计过程需要多长时间？

**A**: 取决于项目规模和审计模式：
- Quick 模式：5-10 分钟
- Standard 模式：30-60 分钟
- Deep 模式：1-3 小时

### Q4: 如何减少误报？

**A**: 本 Skill 采用多层验证机制：
- 静态分析 + 动态测试验证
- 多 Agent 交叉验证
- 实际测试验证（POC）
- 防幻觉机制（证据要求、路径完整、实际测试）

### Q5: 发现漏洞后如何修复？

**A**: 审计报告包含：
- 漏洞代码和修复代码对比
- 多种修复方案（推荐方案 + 备选方案）
- 修复验证方法
- 优先级建议

---

## 贡献指南

欢迎贡献漏洞案例、改进建议或修复方案：

1. Fork 本项目
2. 创建特性分支
3. 提交更改
4. 创建 Pull Request

### 贡献内容

- **漏洞案例**: 添加到 `examples/vulnerability_cases.md`
- **检测规则**: 添加到 `resources/rules/`
- **知识文档**: 添加到 `resources/knowledge/`

---

## 许可证

MIT License

---

## 联系方式

- 提交 Issue
- 邮件: EasternSman@163.com]

---

**注意**: 本 Skill 仅供安全研究和教育目的，请勿用于非法用途。

