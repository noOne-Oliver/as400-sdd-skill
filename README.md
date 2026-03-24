# AS400-SDD Framework Skill

> Spec-Driven Development Framework for AS400 RPG Development
> 支持 Copilot Chat 和 OpenCode CLI

## 概述

这是一个 AS400 RPG 开发的 AI 辅助框架，基于 Spec-Driven Development (SDD) 理念设计。

### 核心能力

- **Spec 生成** — 规范化需求文档
- **代码生成** — 支持 Fixed/Free Format
- **Code Review** — 完整检查清单
- **幻觉预防** — 防止 Copilot 生成错误字段

### 适用场景

- AS400 RPG 开发
- Fixed Format（旧版 RPG）和 Free Format（现代 RPGLE）双支持
- 封闭环境（仅有 GitHub Copilot + OpenCode CLI）

---

## 安装

### Copilot Chat

将 `.copilot/md/instructions.md` 复制到你的项目 `.copilot/md/` 目录：

```bash
cp -r .copilot /your-project/
```

或在 Copilot Chat 中直接引用：

```markdown
@path/to/.copilot/md/instructions.md
```

### OpenCode CLI

将 `.opencode/instructions.md` 复制到你的项目 `.opencode/` 目录：

```bash
cp -r .opencode /your-project/
```

或在 OpenCode CLI 中加载：

```
/skill load @path/to/as400-sdd-skill
```

---

## 快速开始

### 1. 启动开发任务

告诉 AI：

```
我需要开发一个 AS400 程序，功能是读取订单表 ORDHDR，
筛选已审批的订单(OHSTAT='A')，返回订单号和客户号。
使用 Free Format。
```

### 2. AI 生成 Spec

AI 会按照框架模板生成规范的 Spec 文档。

### 3. 确认 Spec

检查 Spec 中的：
- 业务规则是否完整
- 文件定义是否正确
- 代码格式是否合适

### 4. AI 生成代码

基于 Spec，AI 生成符合规范的代码。

### 5. AI Review

AI 按照 Review 清单检查代码。

---

## 目录结构

```
as400-sdd-skill/
├── .copilot/
│   └── md/
│       └── instructions.md    ← Copilot Chat Skill
├── .opencode/
│   └── instructions.md       ← OpenCode CLI Skill
├── README.md
└── LICENSE
```

---

## 代码格式

### Fixed Format（旧版 RPG）

基于列位置的的传统写法：

```rpgle
     C     *ENTRY        PLIST
     C                   PARM                    P_ORDNO      10
     C     P_ORDNO       CHAIN     ORDHDR                      90
     C                   IF        %FOUND
     C                   MOVEL     'A'         W_STATX
     C                   ENDIF
     C                   RETURN
```

### Free Format（现代 RPGLE）

现代自由格式：

```rpgle
**FREE

DCL-PI MAIN;
  p_OrderNo CHAR(10);
END-PI;

chain p_OrderNo ORDHDR;
if %found;
  W_STATX = 'A';
endif;

return;
```

---

## 流程图

```
用户需求
    │
    ▼
┌─────────┐
│ 写 Spec │  ← AI 按模板生成
└─────────┘
    │
    ▼
┌─────────┐
│ 生成代码 │  ← 按 Spec 和指定格式生成
└─────────┘
    │
    ▼
┌─────────┐
│ Code Review │  ← 按清单检查
└─────────┘
    │
    ▼
   完成
```

---

## 常见问题

### Q: 如何指定代码格式？

在需求中说明：
- "使用 Fixed Format"
- "使用 Free Format"
- "主程序 Free，子程序 Fixed"

### Q: Spec 需要包含什么？

- 基本信息（负责人、优先级）
- 业务规则（主逻辑、校验、权限、事务）
- 数据库设计（PF/LF 文件、字段）
- 程序结构
- 代码格式

### Q: 如何避免 Copilot 幻觉？

1. Spec 中提供文件定义源码路径
2. 明确列出禁止的字段
3. Review 时对照文件定义检查

---

## License

MIT
