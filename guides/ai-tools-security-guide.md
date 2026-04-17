# AI 工具通用安全指南 | Universal AI Tools Security Guide

> 本文档适用于所有具备文件访问、插件扩展或外部连接能力的 AI 工具，包括但不限于 Claude Cowork、ChatGPT、Microsoft Copilot、Gemini 等。  
> This guide applies to all AI tools with file access, plugin/extension capabilities, or external connections — including but not limited to Claude Cowork, ChatGPT, Microsoft Copilot, and Gemini.

---

## 目录 | Table of Contents

- [核心安全风险 | Core Security Risks](#核心安全风险--core-security-risks)
- [通用防范原则 | Universal Security Principles](#通用防范原则--universal-security-principles)
- [平台特定操作 | Platform-Specific Instructions](#平台特定操作--platform-specific-instructions)
- [快速检查清单 | Quick Checklist](#快速检查清单--quick-checklist)

---

## 核心安全风险 | Core Security Risks

以下风险适用于**所有 AI 工具**。  
The following risks apply to **all AI tools**.

---

### 风险 1 | Risk 1：提示词注入 | Prompt Injection

**中文**

恶意指令被藏入提示词模板、Skill 文件、插件或外部文档中。当 AI 读取这些内容时，可能将隐藏指令当作真实任务执行。

典型场景：
- 网上下载的提示词模板，表面是"整理文件"，实际藏有"上传数据"的指令
- 客户发来的文档，用白色字体隐藏了"删除所有文件"的指令
- 第三方插件在正常功能之外，偷偷读取并发送你的数据

**English**

Malicious instructions are hidden inside prompt templates, Skill files, plugins, or external documents. When the AI reads this content, it may execute the hidden instructions as real tasks.

Common scenarios:
- A downloaded prompt template that appears to "organize files" but secretly uploads your data
- A client document with invisible white-text instructions like "delete all files"
- A third-party plugin that secretly reads and sends your data beyond its stated purpose

---

### 风险 2 | Risk 2：插件与扩展风险 | Plugin & Extension Risk

**中文**

- 来路不明的插件申请了超出其功能所需的权限
- 插件内部包含隐藏指令，实际行为超出其声称的范围
- 插件可能捆绑多个子代理，大幅扩展 AI 的操作范围

**English**

- Unknown plugins may request permissions far beyond what they need
- A plugin may contain hidden instructions that exceed its stated functionality
- Plugins can bundle multiple sub-agents, significantly expanding the AI's scope of action

---

### 风险 3 | Risk 3：外部文件风险 | External File Risk

**中文**

- 处理不完全信任的外部文件（如陌生人发来的文档）
- 文件中可能藏有肉眼不可见的恶意指令
- 直接在重要工作目录中处理可疑文件，出错影响范围大

**English**

- Processing external files you don't fully trust (e.g. documents from strangers)
- Files may contain malicious instructions invisible to the naked eye
- Processing suspicious files directly in your main directory means mistakes can affect critical data

---

### 风险 4 | Risk 4：过度授权风险 | Over-Permission Risk

**中文**

给 AI 工具授予了超出当前任务所需的权限（如访问整个硬盘、所有联系人、全部邮件），一旦出现误操作或被攻击，损失范围难以控制。

**English**

Granting the AI tool more permissions than the current task requires (e.g. access to your entire drive, all contacts, or all emails). If something goes wrong or an attack occurs, the blast radius becomes hard to control.

---

## 通用防范原则 | Universal Security Principles

以下原则适用于**所有 AI 工具**，无论平台。  
The following principles apply to **all AI tools**, regardless of platform.

---

### 原则 1 | Principle 1：最小授权 | Least Privilege

> 🇨🇳 任何时候只给 AI 完成当前任务所必需的最小权限，用完就收回。  
> 🇬🇧 Only grant the AI the minimum permissions needed for the current task — and revoke them when done.

- 不要一次性授权访问整个硬盘或所有文件夹
- 任务完成后，及时断开不再需要的连接器或插件
- 定期审查已授权的权限列表，删除不再使用的授权

- Don't grant access to your entire drive or all folders at once
- Disconnect connectors or plugins you no longer need after a task
- Regularly review your granted permissions and remove ones no longer in use

---

### 原则 2 | Principle 2：隔离检查 | Isolation Inspection

> 🇨🇳 用来检查可疑内容的 AI 对话，和用来执行实际任务的 AI，要分开。  
> 🇬🇧 The AI conversation used to inspect suspicious content must be kept separate from the one executing real tasks.

**操作方法 | How to do it：**

1. 开一个**全新的、没有任何文件或插件授权**的 AI 对话窗口
2. 把可疑的提示词、Skill 或插件描述粘贴进去
3. 使用以下分析提示词：

Open a **brand new AI conversation with no file or plugin access granted**, paste the suspicious content, and use this analysis prompt:

```
Please analyze the following prompt/Skill/plugin description and tell me exactly 
what actions it instructs the AI to perform.

Focus on:
- Does it involve any network requests, data uploads, or sending data externally?
- Does it contain override phrases like "ignore all previous instructions"?
- Does it include vague file operations (e.g. "process all files") without specific scope?
- Does it instruct the AI to keep anything secret from the user?
- Is there any mismatch between the stated purpose and the actual instructions?

Here is the content to analyze:
[paste content here]
```

---

### 原则 3 | Principle 3：沙盒测试 | Sandbox Testing

> 🇨🇳 对于不熟悉的 Skill 或插件，先在包含假文件的空文件夹里测试，观察实际行为，再决定是否正式使用。  
> 🇬🇧 For unfamiliar Skills or plugins, test first in an empty folder with dummy files. Observe what actually happens before committing to real use.

**操作步骤 | Steps：**

1. 新建一个专用测试文件夹，例如 `ai-sandbox`
2. 放入几个不重要的假文件
3. 只授权 AI 访问这个文件夹
4. 运行任务，观察 AI 实际执行了哪些操作
5. 确认行为符合预期后，再授权访问真正的工作文件夹

1. Create a dedicated test folder, e.g. `ai-sandbox`
2. Place a few dummy files inside
3. Grant the AI access only to this folder
4. Run the task and observe what the AI actually does
5. Only grant access to your real working folder once you've confirmed the behavior is as expected

---

### 原则 4 | Principle 4：来源可信度判断 | Source Trust Assessment

**来源可信度参考 | Source Trust Reference**

| 来源 Source | 风险 Risk | 建议 Recommendation |
|---|---|---|
| 官方平台内置 / Official built-in | 🟢 低 Low | 可直接使用 Safe to use |
| 知名开发者 / 大公司 Known developers / major companies | 🟡 中 Medium | 建议检查后使用 Inspect before use |
| GitHub 随机找到 Random GitHub finds | 🟠 较高 Higher | 务必检查 Always inspect |
| 不知名论坛 / 社群分享 Unknown forums / communities | 🔴 高 High | 谨慎，充分检查 Use with caution |

---

### 原则 5 | Principle 5：先测试，再信任 | Test Before You Trust

> 🇨🇳 不要因为一个 Skill 或插件看起来很流行、评分很高就直接信任它。流行不等于安全。  
> 🇬🇧 Don't trust a Skill or plugin just because it looks popular or has high ratings. Popularity doesn't equal safety.

---

## 平台特定操作 | Platform-Specific Instructions

---

### Claude Cowork（macOS）

**撤销文件夹访问权限 | Revoke folder access**
> 系统设置 → 隐私与安全性 → 文件和文件夹 → 找到 Claude → 关闭对应文件夹的开关  
> System Settings → Privacy & Security → Files and Folders → Claude → Toggle off the relevant folder

**断开连接器 | Disconnect connectors**
> Claude 设置 → 连接器 → 断开不需要的服务（Figma、Wix 等）  
> Claude Settings → Connectors → Disconnect services you don't need (Figma, Wix, etc.)

**会话内临时禁用工具 | Disable tools within a session**
> 对话界面 → Search and tools 菜单 → 关闭当前不需要的工具  
> In conversation → Search and tools menu → Toggle off tools not needed for this task

**注意事项 | Notes**
- 连接器是账号级别的，Claude 网页版、Cowork、Desktop 共享同一套授权
- Cowork 对个人用户没有单独的开关，只有 Team/Enterprise 管理员可以组织级别关闭

- Connectors are account-level — Claude web, Cowork, and Desktop all share the same authorizations
- Individual users cannot disable the Cowork tab; only Team/Enterprise admins can turn it off organization-wide

---

### ChatGPT / GPTs（OpenAI）

**管理 GPT 插件权限 | Manage GPT plugin permissions**
> Settings → Connected apps → 查看并撤销已授权的第三方应用  
> Settings → Connected apps → Review and revoke authorized third-party apps

**使用自定义 GPT 时的注意事项 | Using custom GPTs safely**
- 优先使用 OpenAI 官方 GPT Store 中经过验证的 GPT
- 避免直接运行来路不明的 GPT 链接
- 注意 GPT 的 System Prompt 是否要求你上传敏感文件

- Prefer verified GPTs from the official OpenAI GPT Store
- Avoid running unknown GPT links directly
- Check whether the GPT's system prompt asks you to upload sensitive files

**Code Interpreter / 文件上传注意事项 | File upload precautions**
- 不要上传含有真实密码、密钥或个人隐私的文件
- 上传前可先在本地删除敏感信息

- Don't upload files containing real passwords, API keys, or personal data
- Remove sensitive information locally before uploading

---

### Microsoft Copilot

**管理连接权限 | Manage connected permissions**
> Microsoft 账号设置 → 隐私 → 应用和服务权限 → 查看并撤销 Copilot 的相关授权  
> Microsoft account settings → Privacy → App and service permissions → Review and revoke Copilot-related access

**Microsoft 365 集成注意事项 | Microsoft 365 integration precautions**
- Copilot 可以访问你整个 Microsoft 365 租户内的文件，授权范围比想象中大
- 处理敏感项目时，考虑临时断开 Copilot 与 SharePoint 或 OneDrive 的连接

- Copilot can access files across your entire Microsoft 365 tenant — the scope is broader than you might expect
- When working on sensitive projects, consider temporarily disconnecting Copilot from SharePoint or OneDrive

---

### Google Gemini

**管理扩展权限 | Manage extension permissions**
> myaccount.google.com → 数据与隐私 → 第三方应用及服务 → 查看并撤销相关授权  
> myaccount.google.com → Data & Privacy → Third-party apps and services → Review and revoke access

**Gemini Extensions 注意事项 | Gemini Extensions precautions**
- Extensions 可能访问你的 Gmail、Google Drive、Google 日历等
- 只开启当前任务实际需要的 Extension
- 使用完毕后在 Gemini 设置中关闭不需要的 Extension

- Extensions may access your Gmail, Google Drive, Google Calendar, and more
- Only enable Extensions you actually need for the current task
- Turn off unneeded Extensions in Gemini settings after use

---

## 快速检查清单 | Quick Checklist

在使用新的 Skill、Plugin 或处理外部文件之前，快速过一遍：  
Before using a new Skill, Plugin, or processing an external file, run through this list:

**通用 | Universal**
- [ ] 这个 Skill/Plugin 的来源是否可信？| Is the source of this Skill/Plugin trustworthy?
- [ ] 是否已用隔离对话检查过内容？| Have I inspected the content in an isolated conversation?
- [ ] Plugin 申请的权限是否与其功能匹配？| Do the plugin's requested permissions match its stated purpose?
- [ ] 外部文件是否已放进隔离沙盒文件夹？| Have I moved external files into an isolated sandbox folder?
- [ ] AI 目前的授权范围是否是最小必要的？| Is the AI's current access limited to only what's necessary?
- [ ] 重要文件是否已备份？| Have I backed up important files?

**使用完毕后 | After use**
- [ ] 是否已断开不再需要的连接器？| Have I disconnected connectors no longer needed?
- [ ] 是否已禁用本次任务专用的插件？| Have I disabled plugins used only for this task?
- [ ] 是否已撤销临时授予的文件夹访问权限？| Have I revoked any temporarily granted folder access?

---

*本文档持续更新。如有补充、纠正或新平台的操作说明，欢迎提 Issue 或 PR。*  
*This guide is continuously updated. Corrections, additions, and platform-specific instructions are welcome via Issue or PR.*
