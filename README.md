# Unity 学习课程：从游戏引擎小白到 AI 驱动实时体验开发者

> Unity Course — from Engine Novice to AI-Driven Real-Time Experience Dev
>
> 一套 **11 阶段 · 30 课 + 毕业项目** 的 Unity 学习路线：引擎与第一个 Cube → 场景/组件 → C# 脚本 → 物理与输入 → 动画/UI → 渲染管线 → 资源管线（Blender/ComfyUI 协同）→ CLI 自动化 → MCP Mode 与 AI 驱动 → 发布部署 → 进阶。定位是 **AI 短剧/实时体验的交互层**：ComfyUI 生成资产 → Unity 组装成可交互体验 → WebGL 发布 → 云端部署。Unity 6.2 + CLI + MCP Mode。

- 学习方案：《[Unity学习方案.md](Unity学习方案.md)》——11 阶段拆解、每课模板与课1示范、里程碑、与现有学习线关系、费用预估
- 课程讲义：《[Unity学习课程讲义.md](Unity学习课程讲义.md)》——逐课展开（讲解版）

## 课程地图（11 阶段 30 课 + 毕业项目）

- **阶段0 Unity 全景与第一个 Cube**（课1–2 · L0）——引擎定位 + 编辑器 + Hub/CLI 安装 + MCP 连接 + 终端创建第一个 Cube
- **阶段1 场景与组件**（课3–5 · L1）——Transform / GameObject / Component 模型，摄像机 / 光照 / 材质基础
- **阶段2 C# 脚本基础**（课6–8 · L1）——Python → C# 差异，变量 / 函数 / 协程，调试与 Profiler
- **阶段3 物理与输入**（课9–11 · L1）——Rigidbody / Collider / 射线检测，New Input System，做最小可跑跳角色
- **阶段4 动画与 UI**（课12–14 · L1）——Mecanim 状态机 / Blend Tree，UGUI 菜单 / HUD
- **阶段5 渲染管线与材质**（课15–17 · L2）——URP / Shader Graph 拖节点，光照贴图 / 全局光照，Profiler
- **阶段6 资源管线与外部协同**（课18–20 · L2）——Blender → Unity 导入（FBX/glTF），ComfyUI 生成贴图 → Unity 材质
- **阶段7 Unity CLI 自动化**（课21–23 · L2）——create / build / package / cloud-build，CI/CD（GitHub Actions）
- **阶段8 MCP Mode 与 AI 驱动**（课24–26 · L2）——Unity 内置 MCP Server，Claude Code 连 Unity 编辑场景，自定义 MCP Tool
- **阶段9 发布与部署**（课27–28 · L3）——WebGL 发布 + 浏览器跑，云端构建，桌面 / 移动端
- **阶段10 进阶主题**（课29–30 · L3）——Shader Graph 进阶 / ECS / Unity Gaming Services 入门
- **阶段11 毕业项目**（2–3 周）——ComfyUI 生成资产 → Unity 组装可交互体验 → WebGL → 云端部署

> ★ = 关键概念/代码演示课；▲ = 云端实操课。完整标注见《Unity学习方案.md》。

## 阅读顺序

1. 先读《方案》"〇 本机环境结论"与"四 学习原则"
2. 按阶段 0 → 11 推进；先装好 Unity Hub + Unity 6.2 Editor + CLI（阶段0），再逐课过
3. 面向"AI 短剧实时交互"：阶段 6 / 8 / 11 与外部资产（Blender / ComfyUI）联动最多，可提前串练

## 目录结构

```
unity-course/
├── README.md
├── Unity学习方案.md       # 11 阶段 30 课 + 毕业项目全案
└── Unity学习课程讲义.md    # 逐课讲义（讲解版，约 83 KB）
```

## 说明

- 纯文档仓库，源自一段系统学习沉淀整理而成；正文命令需在装好 Unity 6.2 + CLI 的机器上执行。
- 已做脱敏：云端资源仅以通用写法出现，不含账号等敏感信息。
