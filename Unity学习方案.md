# Unity 学习方案 v1：从游戏引擎小白到 AI 驱动实时体验开发者

> 依据：2026-08-31 认知聊天 + Unity 6.2 / CLI 2026.7 / MCP Mode 官方文档调研
> 定位：从「Unity 零基础」→「能独立做完整项目」→「能用 CLI + MCP 做 AI 驱动的实时体验」→「能结合 AI 短剧方向做毕业作品」
> 核心思路：**先跑通再深入**——阶段 0–4 用最小知识量做出一个能玩的东西（建立信心），阶段 5–8 深入管线与自动化（CLI + MCP 是核心差异化），阶段 9–11 发布与毕业项目
> 配套讲义：《Unity学习课程讲义.md》（30 课完整内容，本文档为体系总纲）
> 层级对照：阶段 0 = L0 认知层；阶段 1–4 = L1 基础层；阶段 5–8 = L2 进阶层；阶段 9–10 = L3 精通层；阶段 11 = 毕业项目

---

## 〇、本机环境结论（2026-08-31 评估）

- **操作系统**：Windows（Unity Editor 在 Windows 上体验最好）+ WSL2（开发辅助）
- **云端 VM**：.133（Linux，无独显，不适合跑 Editor，适合跑 CLI 构建）
- **AWS 云端（可选）**：东京区可用 EC2 g4dn.xlarge（T4 显卡）做云端开发/构建（账号 ID 已脱敏）
- **本机配置要求**：
  - 最低：8 GB 内存 + 集显（DX11）+ SSD，可做 2D / 简单 3D 学习
  - 舒适：16 GB 内存 + GTX 1060 以上 + SSD，可做中等 3D 项目
  - 高端：32 GB + RTX 2070+，HDRP / 3A 级画质
- **网络**：pip→aliyun、apt→阿里云+USTC、GitHub→ghfast（沿用现有镜像方案）
- **双轨环境（写死）**：本机 Windows 跑 Editor 做开发；VM / AWS 跑 CLI 做构建与发布

---

## 一、前置地基与入口评估

  入口要求：Python 熟练（已满足）。C# 零基础可以，但需要适应。逐项自评：

- Python 基础 ✅（已熟练）
- 面向对象概念（类、继承、接口）—— 如果没系统学过，阶段 2 会补
- 3D 空间感（坐标轴、旋转、缩放）—— Blender 经验可迁移
- 命令行基础 ✅（WSL2 日常使用）
- Git 基础 ✅

  C# 与 Python 的关键差异（阶段 2 会展开）：
  - 静态类型（编译时报错，不是运行时）
  - 编译型（改完代码要编译，不是解释执行）
  - MonoBehaviour 生命周期（Start / Update / FixedUpdate 这些钩子）
  - 没有 REPL（不能像 Python 那样一行行试）

---

## 二、Unity 简史与 2026 现状

- 2005 Unity 诞生，最初只做 macOS 游戏
- 2008 Windows 支持 + 免费个人版 → 独立游戏爆发
- 2012–2016 手游黄金期，Unity 成为手游第一引擎
- 2017 引入 ECS / DOTS（高性能方向）、SRP（可编程渲染管线）
- 2020 Unity 6 预览，开始大重构
- 2023 "按安装量收费"骚操作 → CEO 下台 → 回归订阅制
- 2024 Unity 6 正式发布（URP / HDRP 成熟）
- 2025 Unity Muse（AI 助手）+ Sentis（引擎内跑神经网络）
- **2026.7 Unity CLI 发布**：官方命令行工具，CI/CD 一等公民
- **2026 Unity MCP Mode**：Editor 内置 MCP Server，AI 可直接操作场景

  2026 现状：Unity 从"人坐在 Editor 前"变成"人定策略、Agent 执行"。CLI 让机器驱动 Unity，MCP 让 AI 驱动 Unity。

---

## 三、2026 工具栈

- **Unity 版本**：Unity 6.2+（含 CLI + MCP Mode）
- **渲染管线**：URP（Universal Render Pipeline）—— 默认选这个，手机/PC/WebGL 通吃
- **脚本**：C#（Mono + IL2CPP 双后端）
- **CLI**：`unity` 命令行工具（create / build / package / cloud-build）
- **MCP**：Unity Editor 内置 MCP Server（Edit → Project Settings → MCP Server）
- **AI 客户端**：Claude Code / Claude Desktop 连 Unity MCP
- **外部协同**：
  - Blender → Unity（FBX / glTF 导出）
  - ComfyUI → Unity（AI 生成贴图 / 材质）
  - three.js ↔ Unity（WebGL 场景互通）
- **云端**：AWS EC2 g4dn（东京区，按小时计费）
- **镜像铁律**：Unity Asset Store 走官方；Package Manager 走官方；GitHub 插件走 ghfast

---

## 四、学习原则（写死，每课遵守）

1. **命令全明文（最高优先）**：每个命令先在对话完整写出（含参数）→ 解释参数 → 我执行 → 贴完整输出 → 解释
2. **验收带数据（硬指标）**：每课有可测通过标准，过不了不进入下一课
3. **先跑通再深入**：先用最小知识量做出能看的东西，再回头补原理
4. **双轨环境**：本机 Editor 做开发；VM / AWS 做构建发布
5. **CLI + MCP 是核心差异化（不是可选）**：从阶段 0 开始就贯穿
6. **与 Blender / ComfyUI 协同贯穿全程**：不是孤立学 Unity
7. **卡点优先**：装不上 / 跑不通，先解决再学内容
8. **费用护栏（写死）**：AWS 实操按现有协议，每次跑前明文给目的/预算/参数，批准才动
9. **反馈闭环（固定三件事）**：每课结尾 = ①验收数据贴出 → ②提问/挑刺 → ③点评反馈
10. **允许打断深挖**：你说"没懂 / 展开 / 结合实例 / 5 条讲清"，按指示重讲
11. **固定对比尾部**：多概念对比用列表逐条列，不用表格
12. **代码/命令全明文展示**：纯讲授模式也按"代码 + 预期输出 + 它证明了什么"结构

---

## 五、环境就绪（开始前一次性做掉）

```bash
# ① 安装 Unity Hub（Windows 本机，手动下载）
# 下载地址：https://unity.com/download
# 安装后登录 Unity ID（个人版免费）

# ② 通过 Hub 安装 Unity 6.2+ Editor（选 LTS 版本）
# Hub → Installs → Install Editor → 勾选：
#   - Microsoft Visual Studio Community（C# IDE）
#   - Windows Build Support（如果要发桌面）
#   - WebGL Build Support（如果要发网页）
#   - Android Build Support（如果要发手机）

# ③ 安装 Unity CLI
# Unity Hub → Preferences → Unity CLI → Install
# 验证：
unity --version

# ④ 创建第一个项目
unity project create --name MyFirstUnity --template "Universal 3D" --path "C:/UnityProjects/"

# ⑤ 打开项目并启用 MCP Mode
# Unity Editor 打开项目 → Edit → Project Settings → MCP Server → Enable
# 验证 MCP 端口：
curl http://localhost:7123/health   # 应该返回 200 OK

# ⑥ Claude Code / Claude Desktop 配置 Unity MCP
# 在 settings.json 的 mcpServers 加：
# "unity": { "url": "http://localhost:7123/mcp" }
```

> 卡点预警：Unity Hub 安装需要管理员权限；CLI 安装路径可能不在 PATH，需要手动加；MCP Mode 默认只监听 localhost，远程访问需要 SSH tunnel。

---

## 六、阶段拆解（11 阶段 30 课 + 毕业项目）

- **阶段0 Unity 全景与第一个 Cube（课1–2，L0 认知层，半天）**：引擎定位 + 编辑器界面 + Hub 安装 + CLI 验证 + MCP 连接 + 终端创建第一个 Cube + 构建运行
- **阶段1 场景与组件（课3–5，L1 基础层，3–4 天）**：Transform / GameObject / Component 模型 ★；摄像机 / 光照 / 材质基础；场景保存与构建
- **阶段2 C# 脚本基础（课6–8，L1，4–5 天）**：从 Python 到 C# 的差异 ★；变量 / 函数 / 协程；调试与 Profiler 入门
- **阶段3 物理与输入（课9–11，L1，4–5 天）**：Rigidbody / Collider / 射线检测 ★；New Input System；做一个能跑能跳的角色（最小闭环）▲
- **阶段4 动画与 UI（课12–14，L1，4–5 天）**：Mecanim 状态机 / Blend Tree；UGUI 菜单与 HUD；动画 + 输入 + UI 闭环
- **阶段5 渲染管线与材质（课15–17，L2 进阶层，1 周）**：URP 深入 / 材质球 / Shader Graph 拖节点 ★；光照贴图 / 全局光照基础；性能 Profiler 解读
- **阶段6 资源管线与外部协同（课18–20，L2，1 周）**：Blender → Unity 导入规范（FBX/glTF）★；ComfyUI 生成贴图 → Unity 材质 ★；Audio / 粒子 / 特效基础
- **阶段7 Unity CLI 自动化（课21–23，L2，1 周）**：CLI 全流程（create / build / package / cloud-build）★；CI/CD 集成（GitHub Actions + CLI）；多平台构建矩阵 ▲
- **阶段8 MCP Mode 与 AI 驱动（课24–26，L2，1 周）**：Unity 内置 MCP Server 原理 ★；Claude Code 连 Unity 做场景编辑 ★；自定义 MCP Tool 注册（C# 侧）；Agent 自动搭场景 ▲
- **阶段9 发布与部署（课27–28，L3 精通层，4–5 天）**：WebGL 发布 + 浏览器跑 ★；AWS EC2 + Unity CLI 云端构建 ▲；桌面 / 移动端发布
- **阶段10 进阶主题（课29–30，L3，4–5 天）**：自定义渲染效果（Shader Graph 进阶）；ECS 概念（了解）；多人联网入门（Unity Gaming Services）
- **阶段11 毕业项目（2–3 周）**：结合 AI 短剧方向——用 ComfyUI 生成资产 → Unity 组装成可交互体验 → WebGL 发布 → AWS 部署 ▲

> ★ = 关键概念 / 代码演示课（≥15 处贯穿）；▲ = AWS / 云端实操课（≥5 处）

---

## 七、每课固定结构模板 + 课1 示范

### 7.1 每课固定结构

```
## 第N课：主题
- 课程目的（一句话，学完能干嘛）
- 历史动机（这技术从哪来、治前人的什么病）
- 原理（类比引入 + 操作，标注深度）
- 关键操作 / 代码演示（可运行代码 + 预期输出 + "它证明了什么"）
- 坑（本课卡点 + 解法）
- 验收（可测硬指标，过不了不进下一课）
- 反馈（你提问 → 我点评）
```

### 7.2 课1 示范（环境安装 + 第一个 Cube）

**课程目的**：装好 Unity 全家桶（Hub + Editor + CLI），通过 MCP 连上 Editor，用终端创建第一个 Cube 并构建运行。

**历史动机**：2026 年之前，Unity 的所有操作都要在 Editor GUI 里点——CLI 缺失让自动化几乎不可能。2026.7 Unity CLI 发布后，构建、打包、资源管理全部可以终端驱动；MCP Mode 则让 AI 能直接操作 Editor。这是 Unity 从"人驱动"到"机器/AI 驱动"的分水岭。

**代码演示 ① 验证 CLI 安装**：

```bash
unity --version
# 预期输出：Unity CLI version 2026.7.x
```

  **它证明了什么**：CLI 装好了，可以从终端驱动 Unity。

**代码演示 ② 创建第一个项目**：

```bash
unity project create --name MyFirstUnity --template "Universal 3D" --path "C:/UnityProjects/"
# 预期输出：Project created at C:/UnityProjects/MyFirstUnity
```

  **它证明了什么**：项目从终端创建，不需要打开 Editor 点菜单。

**代码演示 ③ 通过 MCP 让 AI 创建一个 Cube**：

```text
# 在 Claude Code 里输入：
请通过 Unity MCP 在当前场景中创建一个 Cube，放在 (0, 2, 0) 位置，命名为 MyFirstCube
```

  预期：Unity Editor 里出现一个 Cube 在 (0, 2, 0)。

  **它证明了什么**：AI 可以通过 MCP 直接操作 Unity 场景——这是 2026 的新能力。

**坑**：
- Unity Hub 安装需要管理员权限，学校/公司电脑可能被拦
- CLI 安装后不在 PATH，需要手动加环境变量
- MCP Mode 默认只监听 localhost，如果 Claude Code 在远程机器上，需要 SSH tunnel
- 第一次构建很慢（缓存冷），别被吓到

**验收**：能说出 `unity --version` 的输出含义；能解释 CLI 和 MCP 分别解决什么问题；能说出"AI 通过 MCP 操作 Unity"的技术路径（HTTP + JSON-RPC + Tool 定义）。

---

## 八、里程碑与验收总览（M0–M11）

- **M0**（阶段0）：环境通 + CLI 跑通 + MCP 连上 + 第一个 Cube 从终端出现 —— 能回答"CLI 和 MCP 分别是什么、解决什么问题"
- **M1**（阶段1）：能独立搭建一个多对象场景（>5 个 GameObject）—— 场景文件能打开、能运行
- **M2**（阶段2）：能写 C# 脚本控制 GameObject 行为 —— 至少 3 个脚本跑通
- **M3**（阶段3）：做一个能跑能跳的角色（最小闭环）—— 键盘控制 + 物理碰撞 + 跳跃，无 bug 运行 1 分钟
- **M4**（阶段4）：动画 + UI 闭环 —— 角色有跑/跳动画，屏幕有 HUD，菜单能切换场景
- **M5**（阶段5）：URP 渲染调优 —— 能解释 URP 和 Built-in 的区别，能改 Shader Graph 节点
- **M6**（阶段6）：外部资产管线通 —— Blender 模型 → Unity 无报错；ComfyUI 贴图 → Unity 材质
- **M7**（阶段7）：CLI 全自动化 —— GitHub PR 触发 → 自动构建 → 上传 Artifact，零手动
- **M8**（阶段8）：MCP 自定义 Tool 注册成功 —— 至少 3 个自定义 Tool，Agent 能自动搭一个完整场景
- **M9**（阶段9）：WebGL 发布 + AWS 部署 —— 浏览器能打开，AWS 上能跑
- **M10**（阶段10）：进阶主题入门 —— 能解释 ECS 概念，能跑通一个多人联网 demo
- **M11**（阶段11）：毕业项目完成 —— ComfyUI 资产 + Unity 交互 + WebGL 发布 + AWS 部署，完整链路

---

## 九、与现有学习线的关系

  Unity 不是孤立的一门课，它和你正在学的东西有明确的协同关系：

- **Blender**：Blender 造资产（模型/动画） → Unity 组装成可交互体验
- **ComfyUI**：ComfyUI 生成资产（贴图/视频） → Unity 做实时呈现
- **three.js**：three.js 在网页做 3D → Unity 也能发 WebGL，但能力更强（物理/动画/音频全内置）
- **AWS**：AWS 做云端构建 + 部署 → Unity CLI 驱动构建，EC2 做开发/测试环境
- **AI 短剧方向**：Unity 可以做"可交互的 AI 短剧体验"——观众不是看视频，而是"进入"故事

  学习顺序建议：Unity 放在 Blender + ComfyUI 基础掌握之后（至少 L1 完成），这样进入 Unity 时已经有资产可以用了。

---

## 十、费用与时间预估

- **时间**：
  - L0（阶段0）：半天
  - L1（阶段1–4）：2–3 周（每天 2–3 小时）
  - L2（阶段5–8）：3–4 周（每天 2–3 小时）
  - L3（阶段9–10）：1–2 周
  - 毕业项目：2–3 周
  - 总计：约 2–3 个月

- **费用**：
  - Unity 个人版：免费（年收入 < $200K）
  - AWS EC2 g4dn.xlarge：约 $0.5/小时（按实际使用计）
  - AWS 预算护栏：每次实操前明文给预算，批准才动（沿用现有协议）
  - Unity Asset Store：大部分学习用免费资产够了，不额外花钱
