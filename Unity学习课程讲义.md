# Unity 学习课程讲义：从游戏引擎小白到 AI 驱动实时体验开发者

> 课程模式：讲授 + 实操（命令全明文 + 执行 + 贴输出；代码为展示用，可随时实跑）
> 覆盖：阶段 0–10，共 30 课 + 毕业项目；先跑通再深入
> 定位：从 Unity 零基础 → 能独立做完整项目 → 能用 CLI + MCP 做 AI 驱动的实时体验
> 配套总纲：《Unity学习方案.md》（体系总纲，本文档为每课详细内容）

---

## 一、前置地基与入口评估

  入口要求：Python 熟练（已满足），C# 零基础可以。逐项自评：

- Python 基础 ✅
- 面向对象概念（类、继承、接口）—— 如果没系统学过，阶段 2 会补
- 3D 空间感（坐标轴、旋转、缩放）—— Blender 经验可迁移
- 命令行基础 ✅
- Git 基础 ✅

  本文档所有命令遵循：Unity CLI 走官方；Package Manager 走官方；GitHub 插件走 ghfast；AWS 实操按现有协议。

---

## 二、Unity 简史与 2026 现状（历史主线，贯穿全文）

  每个技术都不孤立——它们各自治前人的一个病，也各自留下一个新问题。这条主线让全文的"为什么"串成一条链。

- 2005 Unity 诞生，最初只做 macOS 游戏——治的是"做游戏太贵太难"的病。
- 2008 引入 Windows 支持 + 免费个人版 → 独立游戏爆发。
- 2012–2016 手游黄金期，Unity 成为手游第一引擎——治的是"跨平台发布太麻烦"。
- 2017 引入 ECS / DOTS（高性能方向）、SRP（可编程渲染管线）—— 治的是"渲染不够灵活"。
- 2023 "按安装量收费"骚操作 → CEO 下台 → 回归订阅制 —— 差点把自己作死。
- 2024 Unity 6 正式发布（URP / HDRP 成熟）。
- 2025 Unity Muse（AI 助手）+ Sentis（引擎内跑神经网络）。
- **2026.7 Unity CLI 发布**：官方命令行工具，CI/CD 一等公民 —— 治的是"Unity 不能自动化"。
- **2026 Unity MCP Mode**：Editor 内置 MCP Server，AI 可直接操作场景 —— 治的是"人必须坐在 Editor 前"。

  2026 现状：Unity 从"人驱动"变成"机器/AI 驱动"。CLI 让机器驱动 Unity，MCP 让 AI 驱动 Unity。

---

# 阶段 0 · Unity 全景与第一个 Cube（L0 认知层）

**阶段目的**：建立第一直觉——Unity 是什么、能做什么、CLI + MCP 是什么；半天跑通第一个闭环。

## 课 1：环境安装 + 第一个 Cube

**课程目的**：装好 Unity 全家桶（Hub + Editor + CLI），通过 MCP 连上 Editor，用终端创建第一个 Cube 并构建运行。

**历史动机**：2026 年之前，Unity 的所有操作都要在 Editor GUI 里点——CLI 缺失让自动化几乎不可能。2026.7 Unity CLI 发布后，构建、打包、资源管理全部可以终端驱动；MCP Mode 则让 AI 能直接操作 Editor。这是 Unity 从"人驱动"到"机器/AI 驱动"的分水岭。

**原理**：

- Unity Hub：管理 Unity 版本、项目、许可证的中央控制台
- Unity Editor：实际的编辑器，打开项目后在这里做开发
- Unity CLI：命令行工具，从终端驱动 Editor 的功能
- MCP Mode：Editor 内置的 HTTP Server，接受 AI 客户端的结构化指令

  （定义）一个 Unity 项目 = 一个文件夹，里面包含场景、脚本、资源、配置文件。Unity Editor 打开这个文件夹，把它变成可编辑的环境。

**代码演示 ① 安装 Unity Hub**：

```text
# Windows 本机，浏览器打开：
https://unity.com/download
# 下载 Unity Hub Setup.exe，双击安装
# 安装后登录 Unity ID（没有就注册一个，个人版免费）
```

  **它证明了什么**：Hub 是起点，所有后续操作都从 Hub 开始。

**代码演示 ② 通过 Hub 安装 Unity 6.2+ Editor**：

```text
# Unity Hub → Installs → Install Editor
# 选择 Unity 6.2.x LTS（长期支持版）
# 勾选模块：
#   - Microsoft Visual Studio Community（C# IDE）
#   - Windows Build Support（发桌面用）
#   - WebGL Build Support（发网页用）
# 安装完大约 10-15 GB，需要 20-30 分钟
```

  **它证明了什么**：Editor 是实际干活的地方，Hub 只是管理器。

**代码演示 ③ 安装并验证 Unity CLI**：

```text
# Unity Hub → Preferences → Unity CLI → Install
# 安装完后，打开新的命令行窗口：
unity --version
```

  预期输出：

```text
Unity CLI version 2026.7.x
```

  **它证明了什么**：CLI 装好了，可以从终端驱动 Unity。这是 2026 的新能力。

**代码演示 ④ 创建第一个项目**：

```bash
unity project create --name MyFirstUnity --template "Universal 3D" --path "C:/UnityProjects/"
```

  预期输出：

```text
Project created at C:/UnityProjects/MyFirstUnity
```

  **它证明了什么**：项目从终端创建，不需要打开 Editor 点菜单。CLI 的第一个实际应用。

**代码演示 ⑤ 打开项目并启用 MCP Mode**：

```text
# Unity Editor 打开项目（双击项目文件夹里的 .unity 文件，或从 Hub 打开）
# Editor 里：Edit → Project Settings → MCP Server → Enable
# 看到 "MCP Server is running on port 7123" 就对了
```

  验证 MCP 端口：

```bash
curl http://localhost:7123/health
```

  预期输出：

```text
{"status": "ok"}
```

  **它证明了什么**：MCP Server 在 Editor 里跑起来了，可以接受外部指令。

**代码演示 ⑥ Claude Code 连 Unity MCP 创建 Cube**：

```text
# 在 Claude Code 的 settings.json 里加：
# "mcpServers": {
#   "unity": { "url": "http://localhost:7123/mcp" }
# }
# 然后在 Claude Code 里输入：
请通过 Unity MCP 在当前场景中创建一个 Cube，放在 (0, 2, 0) 位置，命名为 MyFirstCube
```

  预期：Unity Editor 里出现一个 Cube 在 (0, 2, 0)。

  **它证明了什么**：AI 可以通过 MCP 直接操作 Unity 场景——这是 2026 的新能力，也是本课程的核心差异化。

**坑**：
- Unity Hub 安装需要管理员权限，学校/公司电脑可能被拦
- CLI 安装后不在 PATH，需要手动加环境变量（Windows：系统属性 → 环境变量 → Path）
- MCP Mode 默认只监听 localhost，如果 Claude Code 在远程机器上，需要 SSH tunnel
- 第一次构建很慢（缓存冷），别被吓到

**验收**：
- 能说出 `unity --version` 的输出含义
- 能解释 CLI 和 MCP 分别解决什么问题
- 能说出"AI 通过 MCP 操作 Unity"的技术路径（HTTP + JSON-RPC + Tool 定义）

---

## 课 2：Unity 编辑器界面 + 第一个场景

**课程目的**：熟悉 Unity Editor 的主要面板，能独立搭建一个包含多个对象的简单场景。

**历史动机**：Unity Editor 的界面设计继承自 3D DCC 工具（Maya / 3ds Max），但针对游戏开发做了大量简化。理解每个面板的功能，是后续所有操作的基础。

**原理**：

- Scene 视图：3D 编辑区，拖拽、旋转、缩放对象
- Game 视图：从摄像机视角看场景，运行后在这里看效果
- Hierarchy：场景里所有对象的列表，树状结构
- Project：项目文件夹的映射，所有资源（模型、脚本、材质等）都在这里
- Inspector：选中对象后，显示它的属性（位置、旋转、组件等）
- Console：日志、警告、错误的输出区

  （定义）GameObject = 场景里的一个实体（可以是 Cube、Sphere、角色、摄像机、灯光……）；Component = 挂在 GameObject 上的功能模块（Transform、MeshRenderer、Rigidbody……）；GameObject 本身只是个壳，Component 给它功能。

**代码演示 ① 在场景里创建多个对象**：

```text
# Unity Editor → Hierarchy 窗口右键 → 3D Object → Cube
# 重复创建：Sphere、Cylinder、Plane、Capsule
# 选中每个对象，在 Inspector 里改 Transform 的 Position
```

  预期：场景里有 5 个对象，分布在不同位置。

  **它证明了什么**：GameObject 是场景的基本单位，Transform 组件控制位置/旋转/缩放。

**代码演示 ② 通过 CLI 创建对象（对比）**：

```bash
# CLI 目前没有直接创建 GameObject 的命令
# 但可以通过 MCP 让 AI 创建：
# 在 Claude Code 里：
请在场景里创建 3 个 Sphere，排成一排，间距 2 米，颜色分别是红、绿、蓝
```

  预期：Unity Editor 里出现 3 个彩色 Sphere。

  **它证明了什么**：CLI 适合项目级操作（创建/构建/发布），MCP 适合场景级操作（创建对象/调参数/搭场景）。两者互补。

**坑**：
- Scene 视图的导航（WASD + 鼠标右键）和 Blender 不同，需要适应
- Inspector 里改数字时，按住 Shift 可以快速调整
- 对象的命名很重要，后面写代码要找对象，乱命名会后悔

**验收**：
- 能说出 6 个主要面板的功能
- 能独立搭建一个 >5 个对象的场景
- 能解释 GameObject 和 Component 的关系

---

# 阶段 1 · 场景与组件（L1 基础层）

**阶段目的**：深入理解 Transform / GameObject / Component 模型，掌握摄像机、光照、材质的基础操作。

## 课 3：Transform 组件深入

**课程目的**：理解 Transform 的 Position / Rotation / Scale，掌握局部坐标和世界坐标的区别。

**历史动机**：3D 空间里的坐标系统是计算机图形学的根基。Unity 用左手坐标系（X 右、Y 上、Z 前），和 Blender（右手，Z 上）不同——从 Blender 过来的人容易搞混。

**原理**：

- Position：对象在世界空间的位置 (x, y, z)
- Rotation：对象的旋转，Unity 内部用四元数（Quaternion），但 Inspector 显示欧拉角 (x, y, z)
- Scale：对象的缩放 (x, y, z)，默认 (1, 1, 1)
- 世界坐标 vs 局部坐标：世界坐标是绝对的，局部坐标是相对于父对象的

  （定义）父子关系：Hierarchy 里把一个对象拖到另一个对象上，就建立父子关系。子对象的 Transform 是相对于父对象的——父对象移动，子对象跟着动。

**代码演示 ① 通过脚本读取和修改 Transform**：

```csharp
// Assets/Scripts/TransformDemo.cs
using UnityEngine;

public class TransformDemo : MonoBehaviour
{
    void Start()
    {
        // 读取当前位置
        Vector3 pos = transform.position;
        Debug.Log($"Position: ({pos.x:F2}, {pos.y:F2}, {pos.z:F2})");

        // 修改位置
        transform.position = new Vector3(0, 2, 0);

        // 读取旋转（欧拉角）
        Vector3 rot = transform.eulerAngles;
        Debug.Log($"Rotation: ({rot.x:F2}, {rot.y:F2}, {rot.z:F2})");

        // 修改缩放
        transform.localScale = new Vector3(2, 2, 2);
    }
}
```

  预期输出（Console 窗口）：

```text
Position: (0.00, 0.00, 0.00)
```

  **它证明了什么**：Transform 是 GameObject 最基础的组件，每个 GameObject 都有，控制位置/旋转/缩放。

**代码演示 ② 父子关系演示**：

```csharp
// Assets/Scripts/ParentChildDemo.cs
using UnityEngine;

public class ParentChildDemo : MonoBehaviour
{
    void Start()
    {
        // 创建父对象
        GameObject parent = new GameObject("Parent");
        parent.transform.position = new Vector3(0, 0, 0);

        // 创建子对象
        GameObject child = new GameObject("Child");
        child.transform.SetParent(parent.transform);
        child.transform.localPosition = new Vector3(1, 0, 0);  // 局部坐标

        // 移动父对象，子对象跟着动
        parent.transform.position = new Vector3(5, 0, 0);
        Debug.Log($"Child world position: ({child.transform.position.x:F2}, {child.transform.position.y:F2}, {child.transform.position.z:F2})");
    }
}
```

  预期输出：

```text
Child world position: (6.00, 0.00, 0.00)
```

  **它证明了什么**：子对象的世界坐标 = 父对象的世界坐标 + 子对象的局部坐标。这是场景层级结构的数学基础。

**坑**：
- 修改 Transform 后，如果对象有 Rigidbody（物理组件），不要直接改 transform.position，用 Rigidbody.MovePosition()
- 旋转的欧拉角有万向锁问题，复杂旋转用 Quaternion
- 父子关系在 Hierarchy 里可以拖拽建立，但代码里用 SetParent() 更灵活

**验收**：
- 能解释世界坐标和局部坐标的区别
- 能说出父子关系的数学含义（子世界坐标 = 父世界坐标 + 子局部坐标）
- 能通过脚本读取和修改 Transform

---

## 课 4：摄像机与光照

**课程目的**：理解摄像机和光照的工作原理，能独立调整场景的视觉效果。

**历史动机**：摄像机是"玩家的眼睛"，光照是"场景的氛围"。早期的 Unity 只有简单的点光源，2024 Unity 6 的 URP 引入了完整的光照系统（实时光 + 烘焙光 + 环境光）。

**原理**：

- Camera：从某个位置/角度渲染场景，输出到 Game 视图
  - Projection：透视（Perspective，近大远小）vs 正交（Orthographic，没有透视）
  - Field of View：视野角度，默认 60°
  - Clear Flags：摄像机背景怎么处理（天空盒 / 纯色 / 不渲染）
- Light：照亮场景
  - Directional：平行光（模拟太阳光，方向固定，位置无关）
  - Point：点光源（向四面八方发光，如灯泡）
  - Spot：聚光灯（锥形光束，如手电筒）
  - Area：面光源（只能烘焙，不能实时）
- 光照模式：实时（Realtime，每帧计算，慢但动态）vs 烘焙（Baked，预先计算，快但不能动）vs 混合（Mixed）

**代码演示 ① 通过脚本控制摄像机**：

```csharp
// Assets/Scripts/CameraFollow.cs
using UnityEngine;

public class CameraFollow : MonoBehaviour
{
    public Transform target;  // 在 Inspector 里拖一个对象进来
    public Vector3 offset = new Vector3(0, 5, -10);
    public float smoothSpeed = 5f;

    void LateUpdate()  // LateUpdate 在所有 Update 之后执行，适合摄像机跟随
    {
        Vector3 desiredPosition = target.position + offset;
        Vector3 smoothedPosition = Vector3.Lerp(transform.position, desiredPosition, smoothSpeed * Time.deltaTime);
        transform.position = smoothedPosition;
        transform.LookAt(target);  // 让摄像机始终看向目标
    }
}
```

  预期：摄像机平滑跟随目标对象，始终看向它。

  **它证明了什么**：摄像机也是一个 GameObject，可以通过脚本控制位置和旋转。LateUpdate 是摄像机跟随的标准做法。

**代码演示 ② 动态改变光照**：

```csharp
// Assets/Scripts/DayNightCycle.cs
using UnityEngine;

public class DayNightCycle : MonoBehaviour
{
    public Light sun;  // 在 Inspector 里拖 Directional Light 进来
    public float rotationSpeed = 10f;

    void Update()
    {
        // 让太阳光旋转，模拟昼夜循环
        sun.transform.Rotate(Vector3.right, rotationSpeed * Time.deltaTime);

        // 根据太阳高度改变颜色
        float height = sun.transform.forward.y;
        if (height > 0)  // 白天
        {
            sun.color = Color.Lerp(Color.white, new Color(1, 0.6f, 0.3f), 1 - height);
            sun.intensity = Mathf.Lerp(0.3f, 1f, height);
        }
        else  // 夜晚
        {
            sun.color = new Color(0.3f, 0.3f, 0.5f);
            sun.intensity = 0.1f;
        }
    }
}
```

  预期：太阳光旋转，场景颜色从白天的白色 → 黄昏的橙色 → 夜晚的蓝色循环变化。

  **它证明了什么**：光照的强度、颜色可以通过脚本动态调整。这是做昼夜循环、氛围变化的基础。

**坑**：
- 摄像机的 Clear Flags 如果是 Solid Color，背景就是纯色；如果是 Skybox，需要设置天空盒材质
- 方向光的旋转决定光照方向，位置不重要
- 实时光照很贵（每帧计算），场景里不要超过 4 个实时光源，其余用烘焙

**验收**：
- 能解释透视和正交投影的区别
- 能说出 4 种光源类型及其适用场景
- 能写出摄像机跟随脚本 + 昼夜循环脚本

---

## 课 5：材质与 Shader Graph 入门

**课程目的**：理解材质的概念，能用 Shader Graph 创建简单的材质效果。

**历史动机**：早期的 Unity 用标准材质（Standard Shader），参数固定，不够灵活。2017 引入 SRP 后，Shader Graph 让不写代码的人也能做材质——拖节点连线，像搭积木一样。

**原理**：

- Material（材质）：定义物体表面的外观（颜色、光滑度、金属度、透明度……）
- Shader（着色器）：计算材质的数学程序，告诉 GPU 怎么画这个表面
- Shader Graph：可视化编辑器，用节点和连线构建 Shader，不需要写代码
- URP 的默认 Shader：Lit（有光照反应）vs Unlit（不受光照影响，纯色）
- 关键参数：
  - Albedo：基础颜色
  - Metallic：金属度（0 = 非金属，1 = 金属）
  - Smoothness：光滑度（0 = 粗糙，1 = 镜面）
  - Normal Map：法线贴图，模拟表面细节（凹凸感）

**代码演示 ① 创建并应用材质**：

```text
# Unity Editor:
# 1. Project 窗口右键 → Create → Material
# 2. 命名为 "RedMetal"
# 3. 选中材质，Inspector 里：
#    - Shader 选 "Universal Render Pipeline/Lit"
#    - Albedo 颜色选红色
#    - Metallic 调到 0.8
#    - Smoothness 调到 0.9
# 4. 把材质拖到场景里的一个 Cube 上
```

  预期：Cube 变成红色金属质感，光滑反光。

  **它证明了什么**：材质控制物体的外观，通过 Inspector 调参数就能改。

**代码演示 ② Shader Graph 做发光效果**：

```text
# Unity Editor:
# 1. Project 窗口右键 → Create → Shader Graph → URP → Unlit Shader Graph
# 2. 命名为 "Emissive"
# 3. 双击打开 Shader Graph Editor
# 4. 添加节点：
#    - Time 节点（输出当前时间）
#    - Sine 节点（Time 输入，输出 -1 到 1 的正弦波）
#    - Remap 节点（把 -1~1 映射到 0~1）
#    - Color 节点（选绿色）
#    - Multiply 节点（Color × Remap 输出）
#    - 连接到 Master Stack 的 Emission 输入
# 5. 保存，创建材质应用这个 Shader
# 6. 放到场景里
```

  预期：物体发出脉动的绿色光，亮度随时间变化。

  **它证明了什么**：Shader Graph 用节点连线就能做动态效果，不需要写代码。这是做特殊视觉效果的基础。

**坑**：
- Shader Graph 只能用于 URP / HDRP，不能用于 Built-in RP
- 节点连线时，注意数据类型匹配（Vector3 不能连到 Float）
- Emission（发光）需要在 Lighting 设置里开启 Post Processing 才能看到辉光效果

**验收**：
- 能解释材质和 Shader 的关系
- 能创建并应用一个自定义材质
- 能用 Shader Graph 做一个简单的动态效果（如脉动发光）

---

# 阶段 2 · C# 脚本基础（L1 基础层）

**阶段目的**：从 Python 过渡到 C#，掌握 Unity 脚本的生命周期、变量、函数、协程。

## 课 6：从 Python 到 C# 的差异

**课程目的**：理解 C# 和 Python 的核心差异，能读懂和编写简单的 C# Unity 脚本。

**历史动机**：Unity 早期支持 JavaScript（UnityScript），2017 年放弃，现在只用 C#。C# 是静态类型、编译型语言，和 Python 的动态类型、解释型有本质区别。

**原理**：

- 静态类型 vs 动态类型：C# 编译时检查类型（`int x = 5;`），Python 运行时检查（`x = 5`）
- 编译型 vs 解释型：C# 改完代码要编译（Unity Editor 里按 Ctrl+S 自动编译），Python 直接执行
- 类与对象：C# 一切皆类，脚本文件必须有一个 public class，类名和文件名一致
- MonoBehaviour：Unity 脚本的基类，提供生命周期钩子（Start / Update / FixedUpdate……）

**代码演示 ① Python vs C# 对比**：

```python
# Python
class Player:
    def __init__(self, name, health):
        self.name = name
        self.health = health

    def take_damage(self, amount):
        self.health -= amount
        print(f"{self.name} took {amount} damage, health: {self.health}")

p = Player("Hero", 100)
p.take_damage(20)
```

```csharp
// C# (Unity)
using UnityEngine;

public class Player : MonoBehaviour
{
    public string playerName;  // 在 Inspector 里可以赋值
    public int health = 100;

    void Start()  // 游戏开始时调用一次
    {
        Debug.Log($"{playerName} spawned with {health} HP");
    }

    public void TakeDamage(int amount)
    {
        health -= amount;
        Debug.Log($"{playerName} took {amount} damage, health: {health}");
    }
}
```

  **它证明了什么**：C# 的语法更严格（类型声明、分号、大括号），但逻辑和 Python 类似。MonoBehaviour 的生命周期钩子（Start）是 Unity 特有的。

**代码演示 ② MonoBehaviour 生命周期**：

```csharp
// Assets/Scripts/LifecycleDemo.cs
using UnityEngine;

public class LifecycleDemo : MonoBehaviour
{
    void Awake() { Debug.Log("1. Awake - 对象创建时，最早"); }
    void OnEnable() { Debug.Log("2. OnEnable - 对象激活时"); }
    void Start() { Debug.Log("3. Start - 第一帧之前，只调用一次"); }
    void FixedUpdate() { Debug.Log("4. FixedUpdate - 固定时间间隔（物理用）"); }
    void Update() { Debug.Log("5. Update - 每帧调用（逻辑用）"); }
    void LateUpdate() { Debug.Log("6. LateUpdate - 所有 Update 之后（摄像机用）"); }
    void OnDisable() { Debug.Log("7. OnDisable - 对象禁用时"); }
    void OnDestroy() { Debug.Log("8. OnDestroy - 对象销毁时"); }
}
```

  预期输出（运行后 Console 里）：

```text
1. Awake - 对象创建时，最早
2. OnEnable - 对象激活时
3. Start - 第一帧之前，只调用一次
4. FixedUpdate - 固定时间间隔（物理用）
5. Update - 每帧调用（逻辑用）
6. LateUpdate - 所有 Update 之后（摄像机用）
... (4, 5, 6 重复多次)
```

  **它证明了什么**：Unity 脚本有明确的生命周期，不同的钩子在不同的时机调用。Update 是最常用的（每帧），FixedUpdate 用于物理（固定时间间隔），LateUpdate 用于摄像机（在所有移动之后）。

**坑**：
- C# 文件名必须和 public class 名一致（`Player.cs` 里必须是 `public class Player`）
- 编译错误会在 Console 里显示，必须修完才能运行
- 在 Inspector 里能看到的变量必须是 public 或标记 `[SerializeField] private`

**验收**：
- 能解释静态类型和动态类型的区别
- 能说出 MonoBehaviour 的主要生命周期钩子
- 能把一个简单的 Python 类翻译成 C#

---

## 课 7：变量、函数、属性

**课程目的**：掌握 C# 的变量、函数、属性（Property），能在 Unity 脚本里使用。

**历史动机**：C# 的变量系统和 Python 不同——有类型声明、有访问修饰符（public / private）、有属性（getter / setter）。这些是 C# 的核心特性。

**原理**：

- 变量：存储数据，必须声明类型（`int x = 5;`）
- 常用类型：int、float、bool、string、Vector3、GameObject、数组、List
- 函数：可重复调用的代码块（`void DoSomething() { ... }`）
- 属性：带 getter / setter 的"智能变量"，可以控制读写

**代码演示 ① 变量类型与 Inspector**：

```csharp
// Assets/Scripts/VariableDemo.cs
using UnityEngine;

public class VariableDemo : MonoBehaviour
{
    // 基础类型
    public int health = 100;           // Inspector 里可见可改
    public float speed = 5.0f;         // f 表示 float，不是 double
    public bool isAlive = true;
    public string playerName = "Hero";

    // 私有变量，Inspector 里不可见
    private int score = 0;

    // 私有变量，但 Inspector 里可见（只读）
    [SerializeField] private int maxHealth = 100;

    // Vector3（3D 坐标）
    public Vector3 spawnPoint = new Vector3(0, 1, 0);

    // GameObject 引用（Inspector 里拖一个对象进来）
    public GameObject target;

    void Start()
    {
        Debug.Log($"Player: {playerName}, HP: {health}, Speed: {speed}");
        if (target != null)
            Debug.Log($"Target: {target.name}");
    }
}
```

  预期输出：

```text
Player: Hero, HP: 100, Speed: 5
Target: Cube  (如果拖了一个 Cube 进来)
```

  **它证明了什么**：public 变量在 Inspector 里可见可改，private 不可见。`[SerializeField]` 可以让 private 变量在 Inspector 里可见（只读）。

**代码演示 ② 属性（Property）**：

```csharp
// Assets/Scripts/PropertyDemo.cs
using UnityEngine;

public class PropertyDemo : MonoBehaviour
{
    private int health = 100;

    // 属性：带 getter 和 setter
    public int Health
    {
        get { return health; }
        private set  // 只能在类内部修改
        {
            health = Mathf.Clamp(value, 0, 100);  // 限制在 0-100
            if (health <= 0)
                Debug.Log("Player died!");
        }
    }

    // 简写形式（C# 7+）
    public int Score { get; private set; } = 0;

    public void TakeDamage(int amount)
    {
        Health -= amount;  // 调用 setter，会自动 clamp
    }

    public void AddScore(int amount)
    {
        Score += amount;
    }

    void Start()
    {
        TakeDamage(30);
        Debug.Log($"Health: {Health}, Score: {Score}");

        // Health = 200;  // 编译错误：setter 是 private
    }
}
```

  预期输出：

```text
Health: 70, Score: 0
```

  **它证明了什么**：属性可以控制变量的读写权限，setter 里可以加逻辑（如 clamp）。这是 C# 的惯用写法，比 Python 的 getter / setter 方法更优雅。

**坑**：
- float 字面量要加 `f`（`5.0f`），不然会被当成 double，赋值给 float 会编译错误
- Vector3 的运算：`Vector3 a + Vector3 b`、`Vector3 a * 2` 都是合法的
- 属性可以只写 getter（只读属性），如 `public int MaxHealth => 100;`

**验收**：
- 能声明和使用各种变量类型
- 能解释 public / private / [SerializeField] 的区别
- 能写带 getter / setter 的属性

---

## 课 8：协程与调试

**课程目的**：理解协程（Coroutine）的概念和用法，掌握 Unity 的调试技巧。

**历史动机**：Unity 的脚本模型是"每帧调用 Update"，但很多操作需要"等待"（如延迟、动画、网络请求）。协程提供了一种"暂停执行，稍后继续"的机制，不需要写复杂的状态机。

**原理**：

- 协程（Coroutine）：可以暂停执行的函数，用 `yield return` 暂停，下一帧或指定时间后继续
- 和普通函数的区别：普通函数一次执行完，协程可以分多帧执行
- 常用 yield 指令：
  - `yield return null;` —— 等一帧
  - `yield return new WaitForSeconds(2f);` —— 等 2 秒
  - `yield return new WaitForSecondsRealtime(2f);` —— 等 2 秒（不受 Time.timeScale 影响）
- 调试技巧：Debug.Log / Debug.LogWarning / Debug.LogError；断点调试（Visual Studio）；Profiler（性能分析）

**代码演示 ① 协程基础**：

```csharp
// Assets/Scripts/CoroutineDemo.cs
using UnityEngine;
using System.Collections;

public class CoroutineDemo : MonoBehaviour
{
    void Start()
    {
        Debug.Log("Starting countdown...");
        StartCoroutine(Countdown());
    }

    IEnumerator Countdown()
    {
        for (int i = 3; i > 0; i--)
        {
            Debug.Log(i);
            yield return new WaitForSeconds(1f);  // 等 1 秒
        }
        Debug.Log("Go!");
    }
}
```

  预期输出（间隔 1 秒）：

```text
Starting countdown...
3
(1 秒后)
2
(1 秒后)
1
(1 秒后)
Go!
```

  **它证明了什么**：协程可以暂停执行，稍后继续。这是做延迟、倒计时、序列动画的标准做法。

**代码演示 ② 调试技巧**：

```csharp
// Assets/Scripts/DebugDemo.cs
using UnityEngine;

public class DebugDemo : MonoBehaviour
{
    public int health = 100;

    void Update()
    {
        // 日志
        if (Input.GetKeyDown(KeyCode.Space))
        {
            health -= 10;
            Debug.Log($"Space pressed, health: {health}");

            if (health <= 0)
                Debug.LogError("Health is zero or below!");
            else if (health <= 30)
                Debug.LogWarning("Health is low!");
        }

        // 在 Scene 视图里画线（调试用）
        Debug.DrawLine(transform.position, transform.position + Vector3.up * 2, Color.red);
    }
}
```

  预期：按空格键，Console 里输出日志；Scene 视图里看到红色向上线。

  **它证明了什么**：Debug.Log 是最基础的调试工具；Debug.DrawLine 可以在 Scene 视图里画线，方便调试物理、路径。

**坑**：
- 协程必须在 MonoBehaviour 里，用 `StartCoroutine()` 启动
- 协程的返回类型是 `IEnumerator`，不是 `void`
- 游戏暂停时（Time.timeScale = 0），`WaitForSeconds` 也会暂停，用 `WaitForSecondsRealtime` 可以避免
- 断点调试需要 Visual Studio（不是 VS Code），在 Unity Hub 安装时勾选

**验收**：
- 能写一个简单的协程（如倒计时）
- 能解释协程和普通函数的区别
- 能使用 Debug.Log + Debug.DrawLine 调试

---

# 阶段 3 · 物理与输入（L1 基础层）

**阶段目的**：掌握物理系统（Rigidbody / Collider / 射线检测）和输入系统（New Input System），做一个能跑能跳的角色。

## 课 9：Rigidbody 与 Collider

**课程目的**：理解物理组件的工作原理，能让对象受重力、碰撞、施加力。

**历史动机**：Unity 的物理引擎用 NVIDIA PhysX（3D）和 Box2D（2D），是业界标准。2024 Unity 6 升级了物理系统，支持更好的碰撞检测和关节约束。

**原理**：

- Rigidbody：给 GameObject 添加物理属性（质量、速度、受力）
- Collider：定义物体的碰撞形状（Box / Sphere / Capsule / Mesh）
- 物理材质（Physic Material）：定义表面属性（摩擦力、弹性）
- 触发器（Trigger）：Collider 标记为 Is Trigger，不产生物理碰撞，但能检测进入/离开

**代码演示 ① 让对象受重力**：

```csharp
// 在 Unity Editor 里：
// 1. 创建一个 Cube
// 2. Add Component → Rigidbody
// 3. 运行，Cube 会掉下来
```

  预期：Cube 受重力掉落，撞到地面（Plane）停下。

  **它证明了什么**：只要给 GameObject 加 Rigidbody，它就自动受物理影响。不需要写代码。

**代码演示 ② 施加力**：

```csharp
// Assets/Scripts/ForceDemo.cs
using UnityEngine;

public class ForceDemo : MonoBehaviour
{
    public Rigidbody rb;
    public float force = 10f;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            rb.AddForce(Vector3.up * force, ForceMode.Impulse);  // 瞬间施加向上的力
        }
    }

    void OnCollisionEnter(Collision collision)
    {
        Debug.Log($"Collided with {collision.gameObject.name}");
    }
}
```

  预期：按空格，Cube 向上跳；撞到地面时 Console 输出日志。

  **它证明了什么**：`AddForce` 可以施加力，`ForceMode.Impulse` 是瞬间力（如跳跃），`ForceMode.Force` 是持续力（如重力）。`OnCollisionEnter` 在碰撞发生时调用。

**坑**：
- 移动 Rigidbody 不要用 `transform.position`，用 `rb.MovePosition()` 或 `rb.AddForce()`
- Collider 必须有至少一个是 Rigidbody，否则碰撞检测不到
- Trigger 用 `OnTriggerEnter`，Collision 用 `OnCollisionEnter`，不要混

**验收**：
- 能解释 Rigidbody 和 Collider 的关系
- 能施加力让对象跳跃
- 能检测碰撞并输出日志

---

## 课 10：射线检测（Raycast）

**课程目的**：掌握射线检测，能做"点击地面移动"、"射击检测"等交互。

**历史动机**：射线检测是游戏里最常用的交互机制——从某个点发出一条"看不见的线"，检测碰到什么。FPS 的射击、RTS 的点击移动、RPG 的交互检测，都用它。

**原理**：

- Raycast：从原点发出一条射线，检测碰到的第一个 Collider
- 参数：原点（Vector3）、方向（Vector3）、最大距离（float）
- 返回：RaycastHit（碰到的对象、碰到的点、法线方向……）
- 常用场景：射击检测、地面检测（角色是否在地面上）、点击选取

**代码演示 ① 从摄像机发射线（FPS 射击）**：

```csharp
// Assets/Scripts/ShootRaycast.cs
using UnityEngine;

public class ShootRaycast : MonoBehaviour
{
    public float range = 100f;

    void Update()
    {
        if (Input.GetMouseButtonDown(0))  // 鼠标左键点击
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            if (Physics.Raycast(ray, out RaycastHit hit, range))
            {
                Debug.Log($"Hit {hit.gameObject.name} at {hit.point}");
                // 在碰到的点生成一个火花特效
                GameObject spark = GameObject.CreatePrimitive(PrimitiveType.Sphere);
                spark.transform.position = hit.point;
                spark.transform.localScale = Vector3.one * 0.1f;
                Destroy(spark, 0.5f);  // 0.5 秒后销毁
            }
        }
    }
}
```

  预期：鼠标左键点击，射线从摄像机发出，碰到对象后 Console 输出日志，碰到的点出现一个小球。

  **它证明了什么**：`Physics.Raycast` 是射线检测的核心 API，可以把屏幕坐标转成世界坐标的射线。

**代码演示 ② 地面检测（角色是否在地面上）**：

```csharp
// Assets/Scripts/GroundCheck.cs
using UnityEngine;

public class GroundCheck : MonoBehaviour
{
    public float groundDistance = 0.2f;
    public LayerMask groundMask;  // Inspector 里选 "Ground" 层

    bool IsGrounded()
    {
        // 从角色脚下向下发射线
        return Physics.Raycast(transform.position, Vector3.down, groundDistance, groundMask);
    }

    void Update()
    {
        if (IsGrounded())
            Debug.Log("Grounded");
    }
}
```

  预期：角色在地面上时，Console 输出 "Grounded"。

  **它证明了什么**：射线检测可以做地面检测，判断角色是否可以跳跃。LayerMask 可以过滤只检测特定层的对象。

**坑**：
- 射线检测每帧调用很贵，不要在不需要的对象上调用
- LayerMask 需要先在 Inspector 里设置 Layer，然后在脚本里选择
- 射线检测不到 Trigger，只能检测 Collision

**验收**：
- 能写 FPS 射击的射线检测
- 能写地面检测
- 能解释 LayerMask 的作用

---

## 课 11：New Input System + 能跑能跳的角色

**课程目的**：掌握 New Input System，做一个能跑能跳的角色（阶段 3 的最小闭环）。

**历史动机**：Unity 的旧输入系统（`Input.GetKey`）只支持键鼠，不支持手柄、触屏。2019 年引入 New Input System，支持所有设备，基于事件驱动。

**原理**：

- New Input System：基于 Action 的输入系统，一个 Action 可以绑定多个输入（键盘、手柄、触屏）
- Input Action Asset：`.inputactions` 文件，定义所有输入 Action
- Player Input 组件：挂在玩家对象上，自动调用 Input Action
- 常用 Action 类型：Button（按下）、Value（轴，如移动方向）

**代码演示 ① 配置 Input Action Asset**：

```text
# Unity Editor:
# 1. Project 窗口右键 → Create → Input Actions
# 2. 命名为 "PlayerInput"
# 3. 双击打开，添加：
#    - Move (Value, Vector2) → 绑定 WASD / 左摇杆
#    - Jump (Button) → 绑定 Space / A 按钮
# 4. Save，勾选 "Generate C# Class"
```

  **它证明了什么**：Input Action Asset 是 New Input System 的核心，可以可视化配置输入绑定。

**代码演示 ② 角色控制器**：

```csharp
// Assets/Scripts/PlayerController.cs
using UnityEngine;

[RequireComponent(typeof(CharacterController))]
public class PlayerController : MonoBehaviour
{
    public PlayerInputActions inputActions;
    public float moveSpeed = 5f;
    public float jumpForce = 5f;
    public float gravity = -9.81f;

    private CharacterController controller;
    private Vector3 velocity;

    void Awake()
    {
        controller = GetComponent<CharacterController>();
        inputActions = new PlayerInputActions();
    }

    void OnEnable()
    {
        inputActions.Enable();
    }

    void OnDisable()
    {
        inputActions.Disable();
    }

    void Update()
    {
        // 移动
        Vector2 moveInput = inputActions.Player.Move.ReadValue<Vector2>();
        Vector3 move = new Vector3(moveInput.x, 0, moveInput.y) * moveSpeed;
        controller.Move(move * Time.deltaTime);

        // 跳跃
        if (controller.isGrounded && inputActions.Player.Jump.wasPressedThisFrame)
        {
            velocity.y = jumpForce;
        }

        // 重力
        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity * Time.deltaTime);
    }
}
```

  预期：WASD 移动，空格跳跃，角色受重力影响。

  **它证明了什么**：New Input System + CharacterController 可以做一个完整的角色控制器。这是阶段 3 的最小闭环。

**坑**：
- CharacterController 和 Rigidbody 不能同时用，选一个
- 跳跃需要自己实现重力，CharacterController 不自动受重力影响
- Input Action 必须在 OnEnable 里 Enable，OnDisable 里 Disable

**验收**：
- 能配置 Input Action Asset
- 能做一个能跑能跳的角色（无 bug 运行 1 分钟）
- 能解释 New Input System 和旧输入系统的区别

---

# 阶段 4 · 动画与 UI（L1 基础层）

**阶段目的**：掌握 Mecanim 动画系统和 UGUI，做动画 + 输入 + UI 的完整闭环。

## 课 12：Mecanim 动画系统

**课程目的**：理解 Mecanim 动画状态机，能让角色有跑 / 跳动画。

**历史动机**：Unity 早期的动画系统（Animation）很简单，但不够灵活。2012 年引入 Mecanim，用状态机驱动动画，支持动画混合、过渡、参数控制。

**原理**：

- Animator Controller：状态机，定义动画之间的过渡条件
- Animation Clip：单个动画（如 run、jump）
- Animator Component：挂在 GameObject 上，控制 Animator Controller
- State：一个动画状态
- Transition：状态之间的过渡，有条件（如 isGrounded = false）
- Blend Tree：多个动画混合（如 walk → run 平滑过渡）

**代码演示 ① 配置 Animator Controller**：

```text
# Unity Editor:
# 1. Project 窗口右键 → Create → Animator Controller
# 2. 命名为 "PlayerAnimator"
# 3. 双击打开 Animator 窗口
# 4. 拖入动画 Clip：Idle、Run、Jump
# 5. 右键 Idle → Make Transition → Run
# 6. 选中 Transition，Inspector 里：
#    - Condition: speed > 0.1
# 7. 类似配置 Run → Idle (speed < 0.1)、Any State → Jump (isJumping = true)、Jump → Idle (isGrounded = true)
# 8. Parameters 窗口添加：speed (Float)、isJumping (Bool)
```

  **它证明了什么**：Animator Controller 用状态机控制动画过渡，参数是过渡的条件。

**代码演示 ② 通过脚本控制动画**：

```csharp
// Assets/Scripts/AnimationController.cs
using UnityEngine;

[RequireComponent(typeof(Animator))]
public class AnimationController : MonoBehaviour
{
    private Animator animator;
    private PlayerController playerController;

    void Start()
    {
        animator = GetComponent<Animator>();
        playerController = GetComponent<PlayerController>();
    }

    void Update()
    {
        // 获取玩家速度
        float speed = playerController.GetCurrentSpeed();
        animator.SetFloat("speed", speed);

        // 跳跃
        if (playerController.IsJumping())
            animator.SetBool("isJumping", true);

        // 着地
        if (playerController.IsGrounded())
            animator.SetBool("isJumping", false);
    }
}
```

  预期：角色移动时播放 run 动画，跳跃时播放 jump 动画。

  **它证明了什么**：Animator 的参数可以控制状态过渡，脚本通过 SetFloat / SetBool 控制动画。

**坑**：
- 动画 Clip 的命名要和 Animator Controller 里的一致
- Transition 的 Has Exit Time 取消勾选，不然过渡会等动画播完
- Blend Tree 可以做 walk → run 平滑过渡，但需要两个动画

**验收**：
- 能配置 Animator Controller
- 能通过脚本控制动画参数
- 能解释状态机和 Blend Tree 的区别

---

## 课 13：UGUI 菜单与 HUD

**课程目的**：掌握 UGUI，能做菜单、按钮、HUD（血条、分数）。

**历史动机**：Unity 早期没有内置 UI 系统，大家用 NGUI 插件。2014 年 Unity 4.6 引入 UGUI，成为标准 UI 系统。

**原理**：

- Canvas：UI 的容器，所有 UI 元素都在 Canvas 里
- RectTransform：UI 元素的 Transform，用锚点（Anchor）控制位置
- 常用 UI 组件：Text（文字）、Image（图片）、Button（按钮）、Slider（滑条）、Panel（面板）
- 事件系统：Button.onClick、Slider.onValueChanged

**代码演示 ① 创建 HUD（血条 + 分数）**：

```text
# Unity Editor:
# 1. Hierarchy 右键 → UI → Canvas（自动创建 Canvas + EventSystem）
# 2. Canvas 下右键 → UI → Slider（血条）
# 3. Canvas 下右键 → UI → Text（分数）
# 4. 调整锚点，血条放左上，分数放右上
```

```csharp
// Assets/Scripts/HUDController.cs
using UnityEngine;
using UnityEngine.UI;

public class HUDController : MonoBehaviour
{
    public Slider healthBar;
    public Text scoreText;

    private PlayerHealth playerHealth;

    void Start()
    {
        playerHealth = FindObjectOfType<PlayerHealth>();
        healthBar.maxValue = playerHealth.maxHealth;
        healthBar.value = playerHealth.health;
    }

    void Update()
    {
        healthBar.value = playerHealth.health;
        scoreText.text = $"Score: {playerHealth.score}";
    }
}
```

  预期：屏幕左上显示血条（随玩家血量变化），右上显示分数。

  **它证明了什么**：UGUI 可以创建 HUD，通过脚本更新 UI 的值。

**代码演示 ② 主菜单**：

```csharp
// Assets/Scripts/MainMenu.cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class MainMenu : MonoBehaviour
{
    public void StartGame()
    {
        SceneManager.LoadScene("GameScene");
    }

    public void QuitGame()
    {
        Application.Quit();
    }
}
```

```text
# Unity Editor:
# 1. 创建 Canvas，添加两个 Button：Start、Quit
# 2. 选中 Start 按钮，Inspector 里 On Click() → +
# 3. 拖入 MainMenu 对象，选择 MainMenu.StartGame
# 4. 类似配置 Quit 按钮
```

  预期：点击 Start 加载游戏场景，点击 Quit 退出。

  **它证明了什么**：Button.onClick 事件可以绑定脚本函数，实现菜单交互。

**坑**：
- Canvas 的 Render Mode：Screen Space - Overlay（默认，覆盖在场景上）、Screen Space - Camera（跟随摄像机）、World Space（在 3D 空间里）
- UI 元素的锚点很重要，不同分辨率下表现不同
- Text 组件已过时，用 TextMeshPro（TMP_Text）更好

**验收**：
- 能创建 HUD（血条 + 分数）
- 能创建主菜单（Start + Quit）
- 能解释 Canvas 的 Render Mode

---

## 课 14：动画 + 输入 + UI 闭环

**课程目的**：把阶段 3–4 的内容串起来，做一个完整的闭环：角色能跑能跳 + 动画 + HUD + 菜单。

**历史动机**：游戏开发的核心是"闭环"——输入 → 逻辑 → 表现 → 反馈。阶段 3 做了输入 + 物理，阶段 4 做了动画 + UI，现在把它们串起来。

**原理**：

- 闭环：玩家输入 → 角色移动/跳跃 → 动画播放 → HUD 更新 → 玩家看到反馈
- 模块化解耦：PlayerController（输入 + 物理）、AnimationController（动画）、HUDController（UI）各司其职

**代码演示 ① 整合所有模块**：

```csharp
// Assets/Scripts/GameManager.cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public static GameManager instance;

    public int score = 0;

    void Awake()
    {
        if (instance == null)
            instance = this;
        else
            Destroy(gameObject);
    }

    public void AddScore(int amount)
    {
        score += amount;
    }

    public void RestartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}
```

```text
# 场景里：
# 1. Player（有 PlayerController + AnimationController + CharacterController）
# 2. Canvas（有 HUDController，显示血条和分数）
# 3. GameManager（空 GameObject，挂 GameManager 脚本）
# 4. 场景里放一些可以收集的对象（碰到加分数）
```

  预期：完整的闭环——玩家移动/跳跃，动画播放，HUD 更新，收集对象加分。

  **它证明了什么**：把输入、物理、动画、UI 串起来，就是一个完整的游戏原型。这是 L1 基础层的毕业作品。

**坑**：
- GameManager 用单例模式（instance），其他脚本可以直接 `GameManager.instance.AddScore(10)`
- 场景切换时，单例会被销毁，需要 `DontDestroyOnLoad(gameObject)` 如果跨场景保留
- 模块化解耦很重要，不要把逻辑都堆在一个脚本里

**验收**：
- 能做一个完整的闭环（输入 → 物理 → 动画 → UI）
- 能解释模块化解耦的意义
- 能用单例模式做 GameManager

---

# 阶段 5 · 渲染管线与材质（L2 进阶层）

**阶段目的**：深入 URP 渲染管线，掌握 Shader Graph 进阶、光照系统、性能分析。

## 课 15：URP 深入与渲染流程

**课程目的**：理解 URP 的工作原理，能配置和切换渲染管线。

**历史动机**：Unity 的 Built-in RP 是十年前的产物，扩展性差。2017 年引入 SRP（Scriptable Render Pipeline），允许用 C# 写渲染逻辑。URP 是 SRP 的通用版本，HDRP 是高清版本。

**原理**：

- URP 的渲染流程：Camera → Render Passes（Opaque → Transparent → Post Processing）
- Render Pipeline Asset：配置 URP 的全局设置（质量、阴影、后处理）
- Renderer Data：配置具体的渲染器（Forward Renderer / 2D Renderer）
- 与 Built-in 的区别：
  - URP：可编程、性能好、跨平台一致
  - Built-in：固定功能、兼容性最好、但不再更新

**代码演示 ① 切换渲染管线**：

```text
# Unity Editor:
# 1. 安装 URP：Window → Package Manager → Unity Registry → Universal RP → Install
# 2. Package Manager 里 Samples → Import "URP Package Samples"
# 3. Project 窗口找到 Samples 里的 UniversalRenderPipelineAsset
# 4. Edit → Project Settings → Graphics → Scriptable Render Pipeline Settings
#    → 拖入 URP Asset
# 5. Edit → Project Settings → Quality → 所有 Quality Level 的 Renderer 改成 URP
# 6. 运行，应该看到渲染效果变化（更现代）
```

  预期：场景渲染效果变化，URP 的后处理（如 Bloom、Color Grading）生效。

  **它证明了什么**：URP 是 Unity 的推荐渲染管线，切换后性能更好、效果更现代。

**代码演示 ② URP Render Feature（自定义渲染通道）**：

```csharp
// Assets/Scripts/SimpleRenderFeature.cs
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

public class SimpleRenderFeature : ScriptableRendererFeature
{
    class SimpleRenderPass : ScriptableRenderPass
    {
        public override void Execute(ScriptableRenderContext context, ref RenderingData renderingData)
        {
            // 在这里加自定义渲染逻辑
            Debug.Log("Custom render pass executed");
        }
    }

    private SimpleRenderPass pass;

    public override void Create()
    {
        pass = new SimpleRenderPass
        {
            renderPassEvent = RenderPassEvent.AfterRenderingOpaques
        };
    }

    public override void AddRenderPasses(ScriptableRenderer renderer, ref RenderingData renderingData)
    {
        renderer.EnqueuePass(pass);
    }
}
```

```text
# Unity Editor:
# 1. 创建 Render Pipeline Asset：Project 右键 → Create → Rendering → URP Asset
# 2. 创建 Renderer Data：Project 右键 → Create → Rendering → URP Renderer
# 3. 选中 Renderer Data，Inspector 里 Add Render Feature → SimpleRenderFeature
# 4. 运行，Console 里看到 "Custom render pass executed"
```

  预期：自定义渲染通道执行。

  **它证明了什么**：URP 允许用 C# 写自定义渲染通道，扩展性极强。这是做特殊效果（如描边、溶解）的基础。

**坑**：
- URP 和 Built-in 的 Shader 不兼容，切换管线后材质需要重新配置
- Render Feature 的执行顺序很重要，`renderPassEvent` 决定在哪个阶段执行
- URP 的后处理（Bloom、Color Grading）需要在 Volume 组件里配置

**验收**：
- 能解释 URP 和 Built-in 的区别
- 能切换渲染管线
- 能写一个简单的 Render Feature

---

## 课 16：Shader Graph 进阶

**课程目的**：掌握 Shader Graph 的高级节点，能做溶解、描边、流动等效果。

**历史动机**：Shader Graph 让不写代码的人也能做复杂材质。2024 Unity 6 的 Shader Graph 支持了更多节点和 Sub Graph（子图），可以复用效果。

**原理**：

- Sub Graph：把一组节点打包成一个可复用的单元
- 自定义函数节点：用 HLSL 写自定义函数
- 常用效果：
  - 溶解（Dissolve）：用 Noise 纹理 + 阈值，让物体逐渐消失
  - 描边（Outline）：反转法线 + 第二遍渲染，给物体加描边
  - 流动（Flow）：用 UV 偏移 + 时间，让贴图流动

**代码演示 ① 溶解效果**：

```text
# Unity Editor:
# 1. Project 右键 → Create → Shader Graph → URP → Unlit Shader Graph
# 2. 命名为 "Dissolve"
# 3. 添加节点：
#    - Sample Texture 2D（噪声纹理）
#    - Property（Float，命名 "DissolveAmount"，默认 0）
#    - Step（Noise.R < DissolveAmount）
#    - Branch（Step 输出 → Alpha 输入）
#    - 连接到 Master Stack 的 Alpha
# 4. Master Stack 的 Surface Type 改成 Transparent
# 5. 保存，创建材质，应用到对象
# 6. Inspector 里调整 DissolveAmount，对象逐渐溶解
```

  预期：调整 DissolveAmount（0-1），对象从完整到逐渐消失，边缘有噪声效果。

  **它证明了什么**：Shader Graph 用节点连线就能做复杂的溶解效果，不需要写代码。

**代码演示 ② 流动效果（水 / 岩浆）**：

```text
# Shader Graph:
# 1. Tiling And Offset 节点
# 2. Time 节点 → 连接到 Offset 的 Y
# 3. Sample Texture 2D（水 / 岩浆贴图）
# 4. 连接到 Master Stack 的 Base Color
# 5. 保存，应用材质
```

  预期：贴图沿 Y 轴流动，像水或岩浆。

  **它证明了什么**：UV 偏移 + 时间可以做流动效果，是水面、岩浆、云的标准做法。

**坑**：
- Shader Graph 的节点类型必须匹配（Vector3 不能连到 Float）
- 溶解效果的 Transparency 需要在材质里开启
- 复杂 Shader 会降性能，手机上尤其要注意

**验收**：
- 能做溶解效果
- 能做流动效果
- 能解释 Sub Graph 的作用

---

## 课 17：光照系统与性能分析

**课程目的**：掌握 URP 的光照系统，能用 Profiler 分析性能。

**历史动机**：光照是 3D 渲染的核心，也是最耗性能的部分。Unity 的光照系统经历了多次演进，2024 Unity 6 的 URP 支持了完整的光照系统（实时光 + 烘焙光 + 环境光）。

**原理**：

- 实时光（Realtime）：每帧计算，慢但动态（如角色手电筒）
- 烘焙光（Baked）：预先计算，快但不能动（如太阳光）
- 混合光（Mixed）：静态部分烘焙，动态部分实时
- 光照探针（Light Probes）：在空间里采样光照，动态对象用
- 光照贴图（Lightmap）：预先计算的光照纹理，静态对象用
- Profiler：Unity 的性能分析工具，看每帧耗时

**代码演示 ① 烘焙光照**：

```text
# Unity Editor:
# 1. 静态对象标记为 Static（Inspector 右上角 Static 复选框）
# 2. 创建 Directional Light，Mode 改成 Mixed
# 3. Window → Rendering → Lighting → Scene
# 4. Lightmapper 选 Progressive GPU（如果有独显）或 Progressive CPU
# 5. Generate Lighting
# 6. 等待烘焙完成（几分钟）
```

  预期：场景光照更真实，阴影更柔和，性能更好（静态部分不需要每帧计算）。

  **它证明了什么**：烘焙光照可以大幅提升性能，是静态场景的标准做法。

**代码演示 ② Profiler 分析**：

```text
# Unity Editor:
# 1. Window → Analysis → Profiler
# 2. 运行场景
# 3. Profiler 窗口显示每帧耗时、CPU / GPU / 内存占用
# 4. 点击某一帧，看详细调用栈
# 5. 找到耗时的部分（如渲染、物理、脚本）
```

  预期：Profiler 显示每帧耗时、各模块占用。

  **它证明了什么**：Profiler 是性能优化的必备工具，可以定位瓶颈。

**坑**：
- 烘焙光照只对 Static 对象生效，动态对象用光照探针
- 光照贴图占用内存，手机上要控制分辨率
- Profiler 的 Overhead 会影响性能，发布版本不要开

**验收**：
- 能烘焙光照
- 能用 Profiler 分析性能
- 能解释实时光 / 烘焙光 / 混合光的区别

---

# 阶段 6 · 资源管线与外部协同（L2 进阶层）

**阶段目的**：掌握 Blender → Unity 的资产导入流程，ComfyUI → Unity 的贴图生成，以及音频 / 粒子 / 特效基础。

## 课 18：Blender → Unity 导入规范

**课程目的**：掌握 Blender 模型导出到 Unity 的最佳实践，避免常见问题。

**历史动机**：Blender 和 Unity 的坐标系不同（Blender 右手 Z 上，Unity 左手 Y 上），直接导入会出问题。2024 年之后，glTF 格式成为标准，但 FBX 仍然是主流。

**原理**：

- FBX：Autodesk 的格式，支持网格、骨骼、动画，Unity 默认支持
- glTF：开源格式，Web 友好，Unity 需要插件支持
- 坐标转换：Blender（右手 Z 上）→ Unity（左手 Y 上），导出时自动转换
- 缩放：Blender 默认 1 米 = 1 单位，Unity 也是，但导出时要确认 Scale 是 1.0

**代码演示 ① Blender 导出设置**：

```text
# Blender:
# 1. 选择要导出的对象
# 2. File → Export → FBX (.fbx)
# 3. 右侧设置：
#    - Path Mode: Copy（贴图嵌入）
#    - Forward: -Z Forward（Unity 坐标）
#    - Up: Y Up
#    - Apply Scalings: FBX Units Scale
#    - Object Types: Mesh（只导出网格）
#    - Armature: 如果角色有骨骼，勾选 Armature
#    - Animation: 如果有动画，勾选 Animation
# 4. 导出
```

  **它证明了什么**：Blender 导出时要配置坐标和缩放，不然 Unity 里会出问题。

**代码演示 ② Unity 导入设置**：

```text
# Unity Editor:
# 1. 把 FBX 文件拖到 Project 窗口
# 2. 选中 FBX，Inspector 里设置：
#    - Model：
#      - Scale Factor: 1（如果 Blender 里是 1 米）
#      - Mesh Compression: Off
#      - Read/Write: 勾选（如果要在脚本里访问顶点数据）
#    - Rig：
#      - Animation Type: Humanoid（人形角色）/ Generic（非人形）/ None（没动画）
#    - Animation：
#      - 如果有动画，勾选 Import Animation
#    - Materials：
#      - Location: Use External Materials（.mat）（如果想自己调材质）
# 3. Apply
```

  预期：模型正确导入，材质、动画都能用。

  **它证明了什么**：Unity 的 FBX 导入设置决定了模型的表现，需要仔细配置。

**坑**：
- Blender 的缩放要在导出前 Apply（Ctrl+A → Scale），不然 Unity 里会变形
- 骨骼动画导出时，Blender 的骨骼命名要和 Unity 的 Humanoid 配置匹配
- 材质导出时，贴图路径要是相对路径，不然 Unity 里找不到

**验收**：
- 能从 Blender 导出 FBX 到 Unity
- 能正确配置导入设置
- 能解释 Blender 和 Unity 的坐标系区别

---

## 课 19：ComfyUI 生成贴图 → Unity 材质

**课程目的**：掌握用 ComfyUI 生成贴图（漫反射、法线、粗糙度），导入 Unity 做材质。

**历史动机**：传统的贴图制作需要画师手绘或用 Substance Designer 生成，耗时长。2024 年之后，AI 生成贴图（ComfyUI + ControlNet）成为新选择，速度提升 10 倍以上。

**原理**：

- PBR 贴图：Physically Based Rendering，包括：
  - Albedo（漫反射）：基础颜色
  - Normal（法线）：表面凹凸细节
  - Roughness（粗糙度）：表面粗糙程度
  - Metallic（金属度）：金属/非金属
- ComfyUI 可以生成这些贴图：
  - Albedo：直接生成
  - Normal：用 Depth → Normal 转换
  - Roughness / Metallic：用灰度图生成

**代码演示 ① ComfyUI 生成 PBR 贴图**：

```text
# ComfyUI 工作流（简化版）：
# 1. 加载 Base Model（如 SD 1.5）
# 2. 加载 ControlNet（Depth 或 Normal）
# 3. 输入提示词："wooden texture, seamless, PBR"
# 4. 生成 Albedo 贴图（1024x1024）
# 5. 用另一个工作流生成 Normal 贴图（Depth → Normal 节点）
# 6. 生成 Roughness 贴图（灰度图，白色 = 粗糙，黑色 = 光滑）
```

  预期：生成 3 张贴图（Albedo、Normal、Roughness）。

  **它证明了什么**：ComfyUI 可以快速生成 PBR 贴图，比手绘快 10 倍。

**代码演示 ② Unity 里配置 PBR 材质**：

```text
# Unity Editor:
# 1. 把 3 张贴图拖到 Project 窗口
# 2. 选中贴图，Inspector 里设置 Texture Type：
#    - Albedo: Default
#    - Normal: Normal Map（勾选 "Create from grayscale" 如果是灰度图）
#    - Roughness: Default（后面在材质里分配）
# 3. Project 右键 → Create → Material
# 4. 选中材质，Inspector 里：
#    - Shader: Universal Render Pipeline/Lit
#    - Albedo: 拖入 Albedo 贴图
#    - Normal Map: 拖入 Normal 贴图
#    - Smoothness: 拖入 Roughness 贴图（反转，因为 Unity 用 Smoothness 不是 Roughness）
# 5. 把材质拖到对象上
```

  预期：对象显示 PBR 效果，有颜色、凹凸、粗糙度。

  **它证明了什么**：ComfyUI 生成的贴图可以直接用在 Unity 的 PBR 材质里，形成完整的工作流。

**坑**：
- Unity 的 Normal Map 是绿色通道翻转的（OpenGL 格式），ComfyUI 生成的是 DirectX 格式，需要转换
- Roughness 和 Smoothness 是反的（Roughness = 1 - Smoothness），Unity 用 Smoothness
- 贴图分辨率要 2 的幂（512、1024、2048），不然会警告

**验收**：
- 能用 ComfyUI 生成 PBR 贴图
- 能在 Unity 里配置 PBR 材质
- 能解释 PBR 贴图的 4 种类型

---

## 课 20：音频、粒子、特效基础

**课程目的**：掌握 Unity 的音频系统、粒子系统，能做基本的音效和特效。

**历史动机**：游戏的沉浸感 50% 来自画面，50% 来自声音和特效。Unity 的音频系统和粒子系统都是内置的，不需要额外插件。

**原理**：

- AudioSource：播放音频的组件，挂在 GameObject 上
- AudioClip：音频文件（.wav / .mp3 / .ogg）
- 3D 音效：AudioSource 在 3D 空间里，声音随距离衰减
- Particle System：粒子系统，用大量小粒子模拟火焰、烟雾、爆炸等
- 关键参数：
  - Emission：每秒发射多少粒子
  - Shape：粒子发射形状（球、圆锥、盒子）
  - Velocity：粒子速度
  - Color over Lifetime：粒子颜色随时间变化
  - Size over Lifetime：粒子大小随时间变化

**代码演示 ① 播放 3D 音效**：

```csharp
// Assets/Scripts/AudioPlayer.cs
using UnityEngine;

[RequireComponent(typeof(AudioSource))]
public class AudioPlayer : MonoBehaviour
{
    public AudioClip clip;
    private AudioSource audioSource;

    void Start()
    {
        audioSource = GetComponent<AudioSource>();
        audioSource.spatialBlend = 1f;  // 1 = 3D 音效，0 = 2D 音效
        audioSource.maxDistance = 20f;
    }

    public void Play()
    {
        audioSource.PlayOneShot(clip);
    }

    void OnCollisionEnter(Collision collision)
    {
        Play();  // 碰撞时播放音效
    }
}
```

  预期：对象碰撞时播放音效，声音随距离衰减。

  **它证明了什么**：AudioSource 可以播放 3D 音效，spatialBlend 控制 2D/3D 混合。

**代码演示 ② 粒子系统（火焰）**：

```text
# Unity Editor:
# 1. Hierarchy 右键 → Effects → Particle System
# 2. 选中 Particle System，Inspector 里配置：
#    - Duration: 5
#    - Start Lifetime: 1
#    - Start Speed: 2
#    - Start Size: 0.5
#    - Shape: Cone（圆锥）
#    - Color over Lifetime: 橙 → 红 → 透明
#    - Size over Lifetime: 大 → 小
# 3. 运行，看到火焰效果
```

  预期：圆锥形火焰效果，粒子从底部向上，逐渐变小变透明。

  **它证明了什么**：粒子系统可以模拟火焰、烟雾、爆炸等效果，不需要写代码。

**坑**：
- 音频文件要导入为 Streaming（大文件）或 Decompress on Load（小文件）
- 粒子系统的 Emission 太高会降性能，手机上控制在 1000 粒子以内
- 粒子可以用自定义 Mesh（如树叶、雪花），不只是小球

**验收**：
- 能播放 3D 音效
- 能配置粒子系统做火焰效果
- 能解释 spatialBlend 的作用

---

# 阶段 7 · Unity CLI 自动化（L2 进阶层）

**阶段目的**：掌握 Unity CLI 的全流程自动化，能做 CI/CD 集成。

## 课 21：CLI 全流程（create / build / package / cloud-build）

**课程目的**：掌握 Unity CLI 的核心命令，能从终端完成项目全生命周期。

**历史动机**：2026 年之前，Unity 的所有操作都要在 Editor GUI 里点，自动化几乎不可能。2026.7 Unity CLI 发布后，构建、打包、资源管理全部可以终端驱动，CI/CD 成为一等公民。

**原理**：

- `unity project create`：创建项目
- `unity build`：构建项目
- `unity package`：打包资产或项目
- `unity cloud-build`：触发云端构建
- 参数：`--platform`（目标平台）、`--scene`（场景列表）、`--output`（输出路径）

**代码演示 ① 创建项目**：

```bash
unity project create --name MyGame --template "Universal 3D" --path "C:/Projects/"
```

  预期输出：

```text
Project created at C:/Projects/MyGame
```

  **它证明了什么**：项目从终端创建，不需要打开 Editor。

**代码演示 ② 构建项目**：

```bash
unity build --project-path "C:/Projects/MyGame" --platform Win64 --scenes "Assets/Scenes/Main.unity" --output "C:/Builds/MyGame.exe"
```

  预期输出：

```text
Build succeeded: C:/Builds/MyGame.exe (120 MB)
```

  **它证明了什么**：构建从终端完成，可以集成到 CI/CD 管线。

**代码演示 ③ 多平台构建**：

```bash
# Windows
unity build --project-path "C:/Projects/MyGame" --platform Win64 --output "C:/Builds/MyGame.exe"

# WebGL
unity build --project-path "C:/Projects/MyGame" --platform WebGL --output "C:/Builds/WebGL/"

# Android
unity build --project-path "C:/Projects/MyGame" --platform Android --output "C:/Builds/MyGame.apk"
```

  预期：3 个平台的构建产物生成。

  **它证明了什么**：CLI 支持多平台构建，一次配置，多平台输出。

**坑**：
- CLI 第一次构建很慢（缓存冷），第二次就正常了
- 构建前需要确保平台模块已安装（Hub → Installs → Add Module）
- WebGL 构建需要安装 Emscripten（Unity Hub 自动处理）

**验收**：
- 能用 CLI 创建项目
- 能用 CLI 构建 Win64 / WebGL / Android
- 能解释 CLI 和 Editor 构建的区别

---

## 课 22：CI/CD 集成（GitHub Actions + CLI）

**课程目的**：掌握用 GitHub Actions + Unity CLI 做自动化构建。

**历史动机**：手动构建容易出错（忘记改版本号、选错平台），CI/CD 让每次提交自动构建，保证质量。

**原理**：

- GitHub Actions：GitHub 的 CI/CD 服务，每次 push / PR 自动运行工作流
- Unity CLI on CI：在 CI 环境里调用 Unity CLI 构建
- Unity License：CI 环境需要 Unity 许可证（个人版 / 专业版）
- Artifact：构建产物上传到 GitHub，可以下载测试

**代码演示 ① GitHub Actions 工作流**：

```yaml
# .github/workflows/build.yml
name: Unity Build

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Unity CLI
        run: |
          # 安装 Unity Hub（无界面）
          choco install unity-hub
          # 安装 Unity Editor
          UnityHub.exe -- --headless install -v 2026.2.0f1 -m win-il2cpp
          # 安装 Unity CLI
          UnityHub.exe -- --headless install-cli

      - name: Activate License
        run: |
          unity license activate --serial ${{ secrets.UNITY_SERIAL }}

      - name: Build
        run: |
          unity build --platform Win64 --output "build/MyGame.exe"

      - name: Upload Artifact
        uses: actions/upload-artifact@v4
        with:
          name: MyGame-Win64
          path: build/MyGame.exe
```

  预期：每次 push 到 main，自动构建 Win64 版本，上传 Artifact。

  **它证明了什么**：GitHub Actions + Unity CLI 可以实现全自动化构建，每次提交都测试。

**代码演示 ② 多平台构建矩阵**：

```yaml
# .github/workflows/build-multi.yml
name: Unity Multi-Platform Build

on:
  push:
    branches: [ main ]

jobs:
  build:
    strategy:
      matrix:
        platform: [Win64, WebGL, Android]
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build
        run: |
          unity build --platform ${{ matrix.platform }} --output "build/${{ matrix.platform }}/"

      - name: Upload
        uses: actions/upload-artifact@v4
        with:
          name: MyGame-${{ matrix.platform }}
          path: build/${{ matrix.platform }}/
```

  预期：每次 push 自动构建 3 个平台，分别上传 Artifact。

  **它证明了什么**：GitHub Actions 的矩阵策略可以一次配置，多平台并行构建。

**坑**：
- CI 环境的 Unity 许可证需要用 Serial（专业版）或 ActivationToken（个人版）
- Windows CI 用 `windows-latest`，Linux CI 用 `ubuntu-latest`（但 Unity 在 Linux 上支持有限）
- 构建产物很大（几百 MB），上传 Artifact 会占 GitHub 存储

**验收**：
- 能写 GitHub Actions 工作流
- 能做多平台构建矩阵
- 能解释 CI/CD 的意义

---

## 课 23：云端构建（Unity Cloud Build）

**课程目的**：掌握 Unity Cloud Build，能在云端构建而不占用本地资源。

**历史动机**：本地构建需要高性能机器，团队里每个人都要装 Unity。Unity Cloud Build 让构建在云端完成，本地只做开发。

**原理**：

- Unity Cloud Build：Unity 官方的云端构建服务
- 配置：在 dashboard.unity3d.com 配置项目、平台、分支
- 触发：push 到 Git 自动触发，或 CLI 手动触发
- 产物：构建完成后下载，或自动发布到 TestFlight / Google Play

**代码演示 ① 配置 Unity Cloud Build**：

```text
# 浏览器打开：
https://dashboard.unity3d.com/cloud-build
# 1. 创建项目（或关联已有项目）
# 2. Add Build Target：
#    - Source: GitHub / GitLab / Bitbucket
#    - Repository: 你的仓库
#    - Branch: main
#    - Platform: Windows / WebGL / Android
# 3. 配置构建设置：
#    - Unity Version: 2026.2.0f1
#    - Scripting Backend: IL2CPP
#    - Scenes: Assets/Scenes/Main.unity
# 4. Save & Build
```

  预期：云端开始构建，完成后可以下载产物。

  **它证明了什么**：Unity Cloud Build 可以在云端构建，不需要本地机器。

**代码演示 ② CLI 触发云端构建**：

```bash
unity cloud-build trigger --project-id "your-project-id" --build-target-id "windows-main"
```

  预期输出：

```text
Build triggered: https://build-api.cloud.unity3d.com/build/12345
```

  **它证明了什么**：CLI 可以触发云端构建，适合 CI/CD 集成。

**坑**：
- Unity Cloud Build 需要 Unity Pro 订阅（个人版不能用）
- 云端构建的机器配置固定，不能自定义
- 构建失败时，日志在 dashboard 里看，不在本地

**验收**：
- 能配置 Unity Cloud Build
- 能用 CLI 触发云端构建
- 能解释 Cloud Build 和本地构建的区别

---

# 阶段 8 · MCP Mode 与 AI 驱动（L2 进阶层）

**阶段目的**：掌握 Unity MCP Mode，能用 AI 驱动场景编辑，能注册自定义 MCP Tool。

## 课 24：Unity 内置 MCP Server 原理

**课程目的**：理解 Unity MCP Server 的工作原理，知道它暴露了哪些 Tool。

**历史动机**：2026 年，MCP（Model Context Protocol）成为 AI 驱动工具的标准协议。Unity 在 Editor 里内置 MCP Server，让 Claude、Cursor 这类 AI 客户端可以直接操作 Unity。

**原理**：

- MCP Server：HTTP Server，接受 JSON-RPC 请求
- Tool：MCP 的功能单元，如 `create_object`、`move_object`、`build_project`
- Resource：MCP 的数据单元，如场景列表、对象属性
- Prompt：MCP 的模板单元，预定义的提示词

**代码演示 ① 查看 MCP Server 状态**：

```bash
curl http://localhost:7123/health
```

  预期输出：

```text
{"status": "ok", "version": "1.0.0"}
```

  **它证明了什么**：MCP Server 在 Editor 里运行，可以接受外部请求。

**代码演示 ② 列出可用的 Tool**：

```bash
curl -X POST http://localhost:7123/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "method": "tools/list", "id": 1}'
```

  预期输出：

```json
{
  "jsonrpc": "2.0",
  "result": {
    "tools": [
      {"name": "create_object", "description": "Create a GameObject"},
      {"name": "move_object", "description": "Move a GameObject"},
      {"name": "delete_object", "description": "Delete a GameObject"},
      {"name": "build_project", "description": "Build the project"},
      ...
    ]
  }
}
```

  **它证明了什么**：MCP Server 暴露了一系列 Tool，AI 客户端可以调用它们操作 Unity。

**坑**：
- MCP Server 默认只监听 localhost，远程访问需要 SSH tunnel
- 权限模型比较粗，生产环境建议关掉写操作的 Tool
- MCP 连接断开重连有时丢状态，长任务做好断点续跑

**验收**：
- 能解释 MCP Server 的工作原理
- 能列出可用的 Tool
- 能用 curl 调用 MCP Tool

---

## 课 25：Claude Code 连 Unity 做场景编辑

**课程目的**：掌握用 Claude Code 通过 MCP 操作 Unity 场景。

**历史动机**：AI 驱动场景编辑是 2026 的新能力——不再是人手动拖拽，而是 AI 根据自然语言指令自动搭建场景。

**原理**：

- Claude Code 配置：在 settings.json 里加 Unity MCP Server
- 自然语言指令：Claude 把自然语言转成 MCP Tool 调用
- 场景编辑：创建对象、移动对象、调整材质、搭建关卡

**代码演示 ① 配置 Claude Code**：

```json
// ~/.claude/settings.json
{
  "mcpServers": {
    "unity": {
      "url": "http://localhost:7123/mcp"
    }
  }
}
```

  **它证明了什么**：Claude Code 可以连接 Unity MCP Server。

**代码演示 ② 自然语言搭建场景**：

```text
# 在 Claude Code 里输入：
请通过 Unity MCP 在当前场景里搭建一个简单的关卡：
- 地面是一个 20x20 的 Plane
- 中间有一个 Cube 作为玩家出生点
- 周围有 4 个 Cylinder 作为障碍物
- 有一个 Directional Light 照亮场景
```

  预期：Unity Editor 里自动创建这些对象，搭建好场景。

  **它证明了什么**：AI 可以根据自然语言指令自动搭建场景，大幅提升效率。

**代码演示 ③ 调整场景参数**：

```text
# 在 Claude Code 里输入：
请把场景里的 Directional Light 的强度调到 0.8，颜色改成暖黄色（RGB: 1.0, 0.9, 0.7）
```

  预期：光照参数改变，场景氛围变化。

  **它证明了什么**：AI 可以精确调整场景参数，不需要人手动找 Inspector。

**坑**：
- 自然语言指令要具体，不然 AI 会猜
- AI 操作前最好备份场景（Ctrl+S 之前先 Ctrl+Shift+S 另存为）
- 复杂的场景搭建分多步，不要一次性给太复杂的指令

**验收**：
- 能配置 Claude Code 连 Unity MCP
- 能用自然语言搭建简单场景
- 能调整场景参数

---

## 课 26：自定义 MCP Tool 注册

**课程目的**：掌握在 Unity 侧用 C# 注册自定义 MCP Tool。

**历史动机**：Unity 内置的 MCP Tool 覆盖基础操作，但团队的高频操作（如"一键生成关卡"、"自动配置光照"）需要自定义 Tool。

**原理**：

- 自定义 Tool：用 C# 写，继承 `McpTool` 基类
- 注册：用 `[McpTool]` 特性标记，Unity 自动注册
- 参数：Tool 可以接受参数（JSON 格式）
- 返回值：Tool 返回 JSON 结果

**代码演示 ① 自定义 MCP Tool**：

```csharp
// Assets/Editor/McpTools/GenerateLevelTool.cs
using UnityEngine;
using Unity.Mcp;

[McpTool("generate_level", "Generate a simple level with obstacles")]
public class GenerateLevelTool : McpTool
{
    [McpParameter("int", Description = "Number of obstacles")]
    public int obstacleCount = 10;

    public override McpResult Execute()
    {
        // 创建地面
        GameObject ground = GameObject.CreatePrimitive(PrimitiveType.Plane);
        ground.transform.localScale = new Vector3(2, 1, 2);
        ground.name = "Ground";

        // 创建障碍物
        for (int i = 0; i < obstacleCount; i++)
        {
            GameObject obstacle = GameObject.CreatePrimitive(PrimitiveType.Cylinder);
            obstacle.transform.position = new Vector3(
                Random.Range(-8f, 8f),
                0.5f,
                Random.Range(-8f, 8f)
            );
            obstacle.name = $"Obstacle_{i}";
        }

        return new McpResult($"Generated level with {obstacleCount} obstacles");
    }
}
```

  **它证明了什么**：可以用 C# 写自定义 MCP Tool，扩展 Unity 的 AI 能力。

**代码演示 ② 调用自定义 Tool**：

```text
# 在 Claude Code 里输入：
请通过 Unity MCP 调用 generate_level 工具，生成一个有 20 个障碍物的关卡
```

  预期：Unity Editor 里自动生成关卡。

  **它证明了什么**：自定义 Tool 可以被 AI 调用，实现团队特定的工作流。

**坑**：
- 自定义 Tool 必须放在 `Editor/` 文件夹里
- Tool 的参数必须是 JSON 兼容的类型（int / float / string / bool）
- Tool 执行会阻塞 Editor，耗时操作要用异步

**验收**：
- 能写自定义 MCP Tool
- 能注册并被 AI 调用
- 能解释 MCP Tool 的工作原理

---

# 阶段 9 · 发布与部署（L3 精通层）

**阶段目的**：掌握 WebGL 发布、AWS 云端构建、桌面 / 移动端发布。

## 课 27：WebGL 发布 + 浏览器跑

**课程目的**：掌握 Unity WebGL 发布，让项目在浏览器里运行。

**历史动机**：Unity 从 5.0 开始支持 WebGL，让游戏可以在浏览器里跑，不需要插件。2024 Unity 6 的 WebGL 性能大幅提升，接近原生。

**原理**：

- WebGL：JavaScript API，让浏览器渲染 3D 图形
- Unity WebGL 发布：把 Unity 项目编译成 WebAssembly + JavaScript
- 限制：
  - 不支持多线程（Web Worker 有限）
  - 不支持文件 IO（用 IndexedDB 代替）
  - 性能比原生差 20-30%

**代码演示 ① WebGL 发布**：

```bash
unity build --project-path "C:/Projects/MyGame" --platform WebGL --output "C:/Builds/WebGL/"
```

  预期输出：

```text
Build succeeded: C:/Builds/WebGL/
```

  产物结构：

```text
C:/Builds/WebGL/
├── index.html          # 入口页面
├── Build/
│   ├── WebAssembly.wasm  # 游戏代码
│   ├── JavaScript.js     # 加载器
│   └── Data.data         # 资源数据
└── TemplateData/          # HTML 模板
```

  **它证明了什么**：Unity 项目可以编译成 WebGL，在浏览器里运行。

**代码演示 ② 本地测试**：

```bash
# 需要 HTTP Server（WebGL 不能直接打开 index.html）
cd "C:/Builds/WebGL/"
python -m http.server 8080
```

  然后浏览器打开 `http://localhost:8080/`。

  预期：浏览器里看到游戏，可以玩。

  **它证明了什么**：WebGL 项目需要 HTTP Server 测试，不能直接打开文件。

**坑**：
- WebGL 构建很慢（第一次 10-20 分钟）
- 产物很大（50-200 MB），需要 CDN 加速
- 浏览器缓存很重要，更新时要改版本号

**验收**：
- 能发布 WebGL 版本
- 能在浏览器里运行
- 能解释 WebGL 的限制

---

## 课 28：AWS EC2 + Unity CLI 云端构建

**课程目的**：掌握用 AWS EC2 做 Unity 云端开发 / 构建环境。

**历史动机**：本地机器可能不够强，或者团队需要共享构建环境。AWS EC2 可以按需开高性能机器，用完就停。

**原理**：

- EC2 g4dn.xlarge：4 vCPU + 16 GB RAM + T4 GPU，约 $0.5/小时
- Windows Server：Unity Editor 在 Windows 上体验最好
- Parsec / Moonlight：低延迟远程桌面，体验接近本地
- Unity CLI on EC2：和课 21 一样，只是机器在云端

**代码演示 ① 启动 EC2 实例**：

```bash
# 使用 AWS CLI（在 WSL2 里）
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \  # Windows Server 2022
  --instance-type g4dn.xlarge \
  --key-name MyKeyPair \
  --security-group-ids sg-0abcdef1234567890 \
  --block-device-mappings '[{"DeviceName":"/dev/sda1","Ebs":{"VolumeSize":100}}]'
```

  预期：EC2 实例启动，2-3 分钟后可连接。

  **它证明了什么**：AWS EC2 可以按需开高性能机器。

**代码演示 ② 连接并安装 Unity**：

```text
# Windows 本机，用 Parsec 连接 EC2：
# 1. 安装 Parsec：https://parsec.app/
# 2. 登录，看到 EC2 实例
# 3. 连接，进入 Windows 桌面
# 4. 安装 Unity Hub + Editor + CLI（和课 1 一样）
```

  预期：远程桌面里可以运行 Unity Editor，体验接近本地。

  **它证明了什么**：Parsec 提供低延迟远程桌面，云端开发体验接近本地。

**坑**：
- EC2 按小时计费，用完要停（`aws ec2 stop-instances`）
- EBS 存储也要钱（100 GB 约 $10/月）
- Parsec 需要 GPU 驱动，Windows Server 要安装 NVIDIA 驱动

**验收**：
- 能启动 EC2 实例
- 能用 Parsec 连接
- 能在云端运行 Unity

---

# 阶段 10 · 进阶主题（L3 精通层）

**阶段目的**：了解自定义渲染效果、ECS 概念、多人联网入门。

## 课 29：自定义渲染效果（Shader Graph 进阶）

**课程目的**：掌握 Shader Graph 的高级技巧，能做描边、溶解、全息效果。

**历史动机**：Shader Graph 让不写 HLSL 的人也能做复杂效果。2024 Unity 6 的 Shader Graph 支持了更多节点，可以做 90% 的常见效果。

**原理**：

- 描边（Outline）：第二遍渲染，顶点沿法线外扩，背面剔除反转
- 全息（Hologram）：Fresnel 效果 + 透明度 + 扫描线
- 溶解（Dissolve）：Noise 纹理 + 阈值 + 边缘颜色

**代码演示 ① 全息效果**：

```text
# Shader Graph:
# 1. Fresnel 节点（视角相关效果）
# 2. Power 节点（Fresnel ^ 3）
# 3. Color 节点（青色，RGB: 0, 1, 1）
# 4. Multiply 节点（Fresnel × Color）
# 5. Time 节点 → Sine → Remap（0-1）
# 6. Step 节点（UV.y < Remap 输出）
# 7. Multiply 节点（Multiply × Step）→ 扫描线
# 8. 连接到 Master Stack 的 Emission 和 Alpha
```

  预期：对象显示全息效果，有扫描线，边缘发光。

  **它证明了什么**：Shader Graph 可以做复杂的全息效果，不需要写 HLSL。

**坑**：
- 复杂 Shader 会降性能，手机上慎用
- Fresnel 效果依赖视角，摄像机动的时候效果会变
- 扫描线用 UV.y，对象旋转时扫描线方向会变

**验收**：
- 能做全息效果
- 能解释 Fresnel 的原理
- 能做描边效果

---

## 课 30：ECS 概念 + 多人联网入门

**课程目的**：了解 ECS（Entity Component System）架构，了解 Unity Gaming Services 多人联网。

**历史动机**：Unity 的传统架构是 GameObject + Component（OOP），但大规模场景（10000+ 对象）性能差。2019 年引入 ECS（面向数据），性能提升 10 倍以上。多人联网一直是 Unity 的弱项，2022 年推出 Unity Gaming Services（UGS），提供官方解决方案。

**原理**：

- ECS：
  - Entity：纯 ID，没有数据
  - Component：纯数据，没有逻辑
  - System：纯逻辑，没有数据
  - 优势：数据连续内存，CPU 缓存友好，性能好
- UGS：
  - Relay：NAT 穿透，P2P 联网
  - Lobby：房间管理
  - Game Server Hosting：专用服务器

**代码演示 ① ECS 概念演示**：

```csharp
// Assets/Scripts/EcsDemo.cs
using Unity.Entities;
using Unity.Transforms;
using Unity.Mathematics;

// Component：纯数据
public struct Health : IComponentData
{
    public int value;
}

// System：纯逻辑
public partial class HealthSystem : SystemBase
{
    protected override void OnUpdate()
    {
        foreach (var health in SystemAPI.Query<RefRW<Health>>())
        {
            if (health.ValueRO.value <= 0)
            {
                // 处理死亡逻辑
            }
        }
    }
}
```

  **它证明了什么**：ECS 的架构和传统 OOP 完全不同，Component 是纯数据，System 是纯逻辑。

**代码演示 ② 多人联网概念**：

```text
# Unity Gaming Services 流程：
# 1. 玩家 A 创建 Lobby
# 2. 玩家 B 加入 Lobby
# 3. 玩家 A 启动 Relay，获取 Join Code
# 4. 玩家 B 用 Join Code 连接
# 5. 开始游戏，同步状态
```

  **它证明了什么**：UGS 提供完整的多人联网解决方案，不需要自己搭服务器。

**坑**：
- ECS 学习曲线陡峭，和传统 OOP 思维完全不同
- ECS 还在预览阶段，API 经常变
- UGS 部分服务收费（Lobby 免费，Game Server Hosting 收费）

**验收**：
- 能解释 ECS 和 OOP 的区别
- 能解释 UGS 的联网流程
- 能写出简单的 ECS Component 和 System

---

# 阶段 11 · 毕业项目

**项目目的**：结合 AI 短剧方向，做一个完整的可交互体验。

**项目内容**：
- 用 ComfyUI 生成资产（角色、场景贴图）
- 用 Unity 组装成可交互体验（如：观众可以走进 AI 生成的场景，触发剧情）
- WebGL 发布，部署到 AWS
- 最终交付：一个可以在线访问的链接

**项目流程**：
1. 需求分析：确定交互方式和故事线
2. 资产生成：ComfyUI 生成角色、场景贴图
3. Unity 开发：搭建场景、编写交互逻辑
4. 测试优化：性能优化、Bug 修复
5. 发布部署：WebGL 构建 + AWS 部署
6. 验收答辩：展示作品，讲解技术方案

**验收标准**：
- 可交互：观众可以控制角色移动、触发事件
- 资产完整：角色、场景、UI 都齐全
- 性能达标：WebGL 在浏览器里流畅运行（30 FPS+）
- 在线可访问：AWS 部署，链接可以公开访问

