# Blender 游戏资产学习路线图（5.2 LTS · macOS）

> 生成时间：2026-09-26　|　目标主线：**游戏资产全流程**　|　验收标准：**能独立做出可导入引擎的成品资产**
> 当前定位：已完成「模式 + 四大工具 + 快捷键」阶段，处于**会用操作、但没形成资产闭环**的临界点

---

## 0. 先定位：你现在在哪、下一站在哪

从你已有的四篇笔记（`对象模式与编辑模式`、`基础练习`、`环切详解`、`Blender快捷键`）判断：

| 维度 | 状态 | 说明 |
| --- | --- | --- |
| Object / Edit 模式 | ✅ 已通 | 知道 Object 是容器、Mesh 是几何；知道 `Ctrl+A → Scale` 的意义 |
| Vertex / Edge / Face | ✅ 已通 | 三种选择模式的适用场景清楚 |
| E / I / Ctrl+R / Ctrl+B | ✅ 已通 | 四大工具的原理（不只是按键）都理解到位 |
| 快捷键体系 | ✅ 已通 | 已建立「动作 + 轴 + 数值」的语法感 |
| **拓扑思维** | 🟡 半成品 | 知道 Quad Flow 重要，但还没用 edge loop 主动规划过模型 |
| **Loop/Ring + GG 滑动 + 支撑线** | ❌ 缺口 | 你笔记末尾自己点名的「下一站」，还没走 |
| **修改器 / 非破坏性流程** | ❌ 缺口 | 目前是纯破坏性建模 |
| **UV** | ❌ 缺口 | 只知道 `U` 键，没做过完整展开与排布 |
| **材质 / PBR / 烘焙** | ❌ 缺口 | 未开始 |
| **引擎导出闭环** | ❌ 缺口 | 没验证过任何一次完整导出 |

**结论：你不需要再补基础操作，缺的是「从几何到资产」的后半程。**
所以这份路线图不从 Cube 讲起，而是**从你笔记的最后一句话接上**。

---

## 1. 版本与环境决策（先定死，别中途换）

### 用哪个版本

- **装 Blender 5.2 LTS**。2026-07-14 发布，官方支持到 **2028-07**，当前补丁版 **5.2.2（2026-09-15）**。
- LTS 的意义：两年内只收 bug 修复、不改功能。做长周期学习计划最怕「学到一半界面变了」，LTS 就是为这个准备的。
- **不要**为了追新去装 5.3/6.0 的每日构建版。

### 5.x 相对 4.x 的变化，会直接影响你找教程

网上大量教程还是 3.x / 4.x，看的时候要注意这几处差异：

| 变化 | 影响 |
| --- | --- |
| 色彩管理管线重写（ACES 1.3/2.0、HDR、宽色域） | 老教程的「颜色发灰」解法可能已过时；你现在能直接用 ACES 视图变换 |
| UI/主题系统重做（移除 300+ 主题选项） | 老教程里的界面截图、自定义主题可能对不上 |
| UV 编辑器同步机制重构 | 4.x 那些「展完 UV 拓扑错位」的抱怨，5.x 好很多 |
| 新增 Array 修改器的 **Scatter on Surface** | 表面散布物体（碎石、植被）不必全靠几何节点 |
| Cycles：Multiresolution 烘焙大幅增强（支持矢量位移、n-gon、只烘焙到选中图像） | 高模→低模烘焙链路更顺 |
| 几何节点新增 SDF / Volume 节点 | 布尔和有机融合有新解法，但**入门阶段先别碰** |

> 老教程依然能看，建模的底层逻辑（拓扑、倒角、挤出）没变。**快捷键和菜单位置有出入时，按 F3 搜命令名**，不用死记。

### macOS 特别注意

1. `Ctrl` 大多可用 `Cmd` 替代，但**不是全部**——和系统快捷键冲突的会失效。请按你笔记第 38 节的说明处理。
2. 没小键盘 → `Edit → Preferences → Input → Emulate Numpad`，用主键盘数字键模拟视角切换。
3. 触控板用户：MMB（中键）很难按。**强烈建议弄个三键鼠标**，或者把 Orbit/ Pan 改成 `Alt+LMB` 之类的组合。视图导航每天要用几百次，这里卡手会非常劝退。

### 必开的内置插件

`Edit → Preferences → Add-ons`，搜名字勾选：

| 插件 | 用途 | 什么时候用上 |
| --- | --- | --- |
| **Node Wrangler** | 材质节点效率神器（`Ctrl+Shift+LMB` 预览节点） | Stage 4 |
| **LoopTools** | 顶点/边的 Circle、Space、Flatten、Gstretch | Stage 0 起 |
| **Bool Tool** | 布尔运算的批量与可视化管理 | Stage 1 |
| **F2** | 快速补面，修洞必备 | Stage 0 |
| **Auto Mirror** | 对称建模自动加 Mirror 修改器 | Stage 1 |
| **Extra Objects** | 追加一批基础几何体 | 随时 |
| **Import Images as Planes** | 导入参考图 | Stage 1 起 |
| **Rigify** | 骨骼绑定生成器 | 支线（做角色才开） |

> glTF 2.0 导入导出是**内置**的，不用装插件。

---

## 2. 路线总览：8 个阶段

| # | 阶段 | 核心能力 | 阶段产出物 | 预估工时 |
| --- | --- | --- | --- | --- |
| **0** | 拓扑与建模思维补齐 | Edge Loop/Ring、GG 滑动、支撑线、SubD 控制 | 一个「圆角方块变音箱」练习组 | 10–14h |
| **1** | 硬表面建模工作流 | 修改器栈、布尔、倒角策略、参考图对型 | **游戏道具 #1：木箱 / 油桶 / 工具箱** | 15–20h |
| **2** | 非破坏性流程与资产管理 | Mirror/Array/Solidify/SubD 栈、集合与命名、单位 | **游戏道具 #2：科幻储物柜（含对称与阵列）** | 10–14h |
| **3** | UV 与贴图坐标 | 缝合边、展开、排布、UDIM 取舍、UV 检查 | 上面两个道具的完整 UV | 12–16h |
| **4** | 材质 / PBR / 烘焙 | Principled BSDF、贴图通道、烘焙 AO/Normal、贴图绘制 | 带完整 PBR 贴图的道具 | 15–20h |
| **5** | 有机资产：雕刻 → 重拓扑 → 烘焙 | Sculpt、Retopo、High→Low 烘焙 | **岩石 / 树桩 / 简单生物道具** | 15–20h |
| **6** | 资产规范与引擎导出 | 命名、原点、面数预算、GLB/FBX 预设、导入验证 | **3 个资产成功进引擎** | 8–10h |
| **7** | 场景组装 + 几何节点 + 作品集 | 资产库、散布、程序化辅助、出图 | **一个完整场景 + 作品集页面** | 20–30h |

**总计约 105–144 小时**（不含支线）。

> 支线（按需选）：**绑定与动画** 8–12h ｜ **几何节点深入** 15–25h ｜ **程序化材质** 10–15h

---

## 3. 各阶段详解

### Stage 0 · 拓扑与建模思维补齐（10–14h）

**一句话目标**：把「会按 Ctrl+R」升级成「知道该在哪加线、加几条、为什么」。

**必学清单**
- [ ] Edge Loop vs Edge Ring：`Alt+LMB` 选环 / `Ctrl+Alt+LMB` 选圈
- [ ] **Edge Slide `G G`**：拓扑不变，只重新分配空间（你笔记第 28 节已有）
- [ ] Loop Cut 的两次左键语义：第一次锁环、第二次定位，中间可右键居中
- [ ] **支撑线（Support Loop）**：`Ctrl+R` 越靠近边缘 → SubD 后越硬；越远 → 越软
- [ ] Subdivision Surface 修改器 + `Shade Auto Smooth`（对象右键）
- [ ] 加权/普通法线、`Mark Sharp`、`Bevel Weight`（`Ctrl+Shift+E` 无关，用边属性面板）
- [ ] 拓扑体检：三角面、N-gon、极点（pole）识别；`Mesh → Clean Up` 相关操作
- [ ] LoopTools：Space / Circle / Flatten 解决「线不均匀」「圆不圆」

**练习项目（3 个，从易到难）**
1. **圆角立方体**：Cube → 全选边 Bevel（3–4 segments）→ 观察高光 → 对比加支撑线后的 SubD 版本
2. **桌腿**：长方体 → `Ctrl+R` x2 → `Alt+LMB` 选中段 → `S` 收腰 → 理解「线控制形状」
3. **音箱**：Cube → Inset 出喇叭面 → Extrude 内凹 → Bevel 边缘 → 加 SubD → 加支撑线保住硬边

**验收标准**（可判定，不是感觉）
- 不看教程，能在 3 分钟内把一个 Cube 做成**边缘锐利但整体平滑**的圆角方块（SubD + 支撑线）
- 能说出「为什么这个角糊了」并指出该在哪加线
- 做完后用 `Mesh → Clean Up` 检查：无孤立顶点、无内部面

**推荐资源**
- 你自己的 `环切详解.md` 第 27–31 节（先把 5 种 Loop Cut 用法逐个走一遍）
- CG Cookie · *Mesh Modeling Bootcamp*（Kent Trammell，10h，付费）——拓扑这块讲得最系统
- YouTube · Josh Gambrell / Cédric Lepiller（硬表面拓扑思路，免费）

**常见坑**
- ❌ 为了「模型更精细」疯狂加线 → 拓扑越密越难改。**先加最少的线把形状定住**
- ❌ 在 Object Scale ≠ 1 的状态下建模 → 每次都要 `Ctrl+A → Scale`（你笔记第 29 节已强调）
- ❌ SubD 后形状崩了就删线重来 → 应该先检查支撑线距离

---

### Stage 1 · 硬表面建模工作流（15–20h）

**一句话目标**：拿到一张参考图，能按流程做出比例正确、拓扑干净、可继续加工的道具。

**必学清单**
- [ ] **参考图设置**：`Import Images as Planes` 导入三视图 → 对齐 → 开 X-Ray
- [ ] **单位与真实尺寸**：`Scene Properties → Units`，用米；给物体设真实尺寸（油桶 0.6m 直径之类）
- [ ] 修改器基础：Mirror、Solidify、Bevel（作为修改器而非 `Ctrl+B`）、Subdivision
- [ ] Boolean（Bool Tool）+ **布尔后的清理**：这是硬表面最痛的一步
- [ ] Bevel 策略：Width 与 Segments 分开想（你笔记第 33 节已懂），再补「按镜头距离定宽度」
- [ ] 分离件 vs 一体件：什么时候该 `P → Separate`
- [ ] 硬边处理：`Shade Auto Smooth` + `Mark Sharp` vs 加倒角

**练习项目**
- **道具 #1：木箱**（入门）：Cube → Inset 木条面 → Extrude → Bevel → 加铁角件
- **道具 #2：油桶**：Cylinder → 加环做箍 → Inset+Extrude 做顶盖 → 支撑线 + SubD
- **道具 #3：工具箱 / 弹药箱**（带把手、锁扣、倒角）：**这是本阶段的验收项目**

**验收标准**
- 按参考图做的道具，比例误差肉眼可接受（对三视图检查）
- 全部 Quad 为主，无 N-gon（允许极少数）
- 加 SubD 2 级后形状不崩
- 面数落在该道具的预算内（见 Stage 6 预算表）
- 文件有命名规范（见 Stage 6）

**推荐资源**
- CG Cookie · *Press Start: Learn Blender by Making a Game-Ready Asset*（Jonathan Lampel，5h，付费）——**最贴这条主线**
- YouTube · **Grant Abbitt**「Detailed Game Assets」系列（免费，偏中高级，硬啃很值）
- YouTube · **Imphenzia**（低模游戏资产，免费）
- YouTube · **Blender Secrets**「Easy hole modeling」（开洞技巧，免费，1 分钟一条）

**常见坑**
- ❌ 布尔完不清理 → 一堆三角面和 n-gon → UV 和烘焙全崩。**布尔只用来开洞，结构靠挤出**
- ❌ Bevel 用在修改器上但没设 Limit Method → 该硬的地方被倒圆了（用 **Weight** 或 **Angle**）
- ❌ 不做真实尺寸 → 进引擎后比例全乱

---

### Stage 2 · 非破坏性流程与资产管理（10–14h）

**一句话目标**：模型改需求时不推倒重来。

**必学清单**
- [ ] 修改器栈的顺序语义（Mirror 在 SubD 前还是后？）
- [ ] Mirror 的正确用法（ clipping、merge、原点位置）
- [ ] Array + **Scatter on Surface**（5.0 新增）做阵列与散布
- [ ] Collection / Outliner 组织：一个资产一个 Collection
- [ ] Linked Duplicate `Alt+D` vs Duplicate `Shift+D` 的数据块共享语义
- [ ] 实例（Instance）与实例化集合：重复摆放的道具用实例省内存
- [ ] 原点管理：`Set Origin`，道具原点放底部中心 / 角色放脚下

**练习项目**
- **道具 #4：科幻储物柜**：对称建模（Mirror）+ 阵列散热孔（Array）+ 面板缝隙（Inset+Extrude）+ 独立门板（可单独 `P` 分开）

**验收标准**
- 关掉某个修改器，能看到「改之前的样子」——证明流程是可逆的
- 改一个参数（比如柜子宽度）能整体跟着变，不用手动重做

**常见坑**
- ❌ Mirror 的原点不在对称轴上 → 模型撕裂，开 Clipping 能缓解但别依赖
- ❌ 修改器顺序错：SubD 在 Mirror 之后会在接缝处产生硬边

---

### Stage 3 · UV 与贴图坐标（12–16h）

**一句话目标**：每个模型都有不重叠、不变形、排布合理的 UV。这是**游戏资产和「好看的渲染图」最大的分水岭**。

**必学清单**
- [ ] **标记缝合边（Seam）**：`Edit Mode → 2 边模式 → 选边 → Ctrl+E → Mark Seam`
- [ ] `U → Unwrap` vs `Smart UV Project` vs `Follow Active Quads` 的适用场景
- [ ] UV 编辑器：缝合选择同步、Pin（`P`）、Pack Islands
- [ ] **UV 检查三件套**：UV Checker 贴图（拉伸检测）+ Overlap 检测 + 孤岛间距
- [ ] 纹素密度（Texel Density）一致：同一场景里的物件 UV 密度要统一
- [ ] 第二套 UV（光照贴图 UV）：做法是把现有 UV 复制一份，重排成不重叠
- [ ] UVPackmaster（免费版可用，支持 Blender 2.93–5.2）做高效排布
- [ ] 5.x 的 UV 同步机制（比 4.x 少踩很多坑）

**练习项目**
- 给 Stage 1/2 的 4 个道具逐个做 UV
- 加一张棋盘格贴图，检查每个面格子是否方正

**验收标准**
- 所有 UV 岛无重叠（烘焙光照贴图的前提下）
- 棋盘格检查：无明显拉伸（格子基本是正方形）
- 纹理空间利用率 80%+（用 Pack Islands 后目测）
- 硬边/倒角边与 UV 缝合边对齐（避免光照接缝）

**推荐资源**
- Blender 官方手册 UV 章节（免费，权威）
- UVPackmaster 官方文档（免费）
- TexTools（免费）——**注意：装之前先确认对 Blender 5.2 的兼容性**，老版本可能不支持

**常见坑**
- ❌ 不标缝合边直接 Unwrap → 得到一个乱七八糟的展开
- ❌ 为了「塞满 UV 空间」把岛排得太挤 → 烘焙时像素溢出（mipmap 一到就露馅）
- ❌ 忘了 UV 是有方向/手性的 → 贴图镜像（检查文字贴图方向）

---

### Stage 4 · 材质 / PBR / 烘焙（15–20h）

**一句话目标**：做出引擎能原样吃进去的 PBR 材质，而不是只有 Blender 里好看的节点。

**必学清单**
- [ ] **Principled BSDF 的每个输入对应什么物理属性**（Base Color / Metallic / Roughness / Normal / AO）
- [ ] 贴图色彩空间：**Color 贴图 = sRGB，Normal/Roughness/Metallic = Non-Color**（错了整个材质就废）
- [ ] Metallic 工作流 vs Specular 工作流（游戏业界默认 Metallic-Roughness）
- [ ] **烘焙**：高模 → 低模的 Normal / AO / Curvature；Cycles 下 Bake 设置（`Selected to Active`、Extrusion 0.02–0.05）
- [ ] 5.0 增强的 Multiresolution 烘焙（支持矢量位移、n-gon）
- [ ] Texture Paint 基础：手绘细节、蒙版
- [ ] 材质数量控制：**一个材质 = 一次 draw call**，道具尽量 1–2 个材质
- [ ] 贴图分辨率决策：小道具 512/1024，主角道具 2048，4096 只给 hero 资产

**练习项目**
- 给工具箱做完整 PBR：Base Color + Roughness + Normal + AO 四张图
- 烘焙一次高模 → 低模的 Normal，验证进引擎后高光正常

**验收标准**
- 材质只有 Principled BSDF + 贴图，**没有 Blender 专有节点**（Mix Shader 之类引擎不认）
- 导入引擎后视觉和 Blender 里基本一致
- 贴图已保存到磁盘（烘焙完不存会丢！）

**推荐资源**
- YouTube · **Grant Abbitt**「Node School: Materials」（免费）
- YouTube · **SouthernShotty**「How PROS Texture: 3 Easy Methods」（免费）
- 免费素材：Poly Haven（CC0 贴图/HDRI）、ambientCG（CC0 PBR 材质）
- 付费：GameDev.tv · *Blender Environment Artist*（Grant Abbitt + Rick Davidson，14.5h，适配 5.1，评分 4.7+）

**常见坑**
- ❌ Normal 贴图设成 sRGB → 光照全错。**Non-Color，再说一遍**
- ❌ 烘焙前没 Apply Scale / 没清重叠 UV → 烘焙出一堆黑块
- ❌ 一个道具 8 个材质 → 引擎里 8 次 draw call，性能直接崩

---

### Stage 5 · 有机资产：雕刻 → 重拓扑 → 烘焙（15–20h）

**一句话目标**：能处理非机械形状（岩石、地形、生物、破损道具）。

**必学清单**
- [ ] Sculpt Mode 基础笔刷：Draw / Clay / Grab / Smooth / Crease
- [ ] Dyntopo 的取舍（好用但会毁拓扑，只在中途用）
- [ ] Voxel Remesh 快速起型（5.2 的 voxel remesher 现在**保留顶点色等属性**）
- [ ] **重拓扑**：手工 Retopo（吸附 + F2 + Poly Build）或 RetopoFlow（付费）
- [ ] 高模 → 低模烘焙流程（接 Stage 4）
- [ ] 多分辨率（Multires）工作流的适用场景

**练习项目**
- **岩石组**（3 块）：Cube → Voxel Remesh 起型 → 雕刻 → 重拓扑到 300–800 tri → 烘焙
- **树桩 / 破损木箱**：练习有机 + 硬表面的混合

**验收标准**
- 低模面数控制在预算内，但轮廓和主要体积特征保留（远距离看不出差别）
- 烘焙的 Normal 图在引擎里能还原高模细节

**推荐资源**
- YouTube · **Grant Abbitt**「Sculpting In Blender: A Complete Beginner's Guide」（免费）
- YouTube · **Ryan King Art**「Sculpting for Complete Beginners」（免费）
- YouTube · **Grant Abbitt**「Realistic Low Poly Game Assets – Rocks」（免费，**最贴这条线**）
- 付费：CG Boost · *Master 3D Sculpting in Blender*

---

### Stage 6 · 资产规范与引擎导出（8–10h）⭐ 关键阶段

**一句话目标**：把「做得出来」变成「能进项目」。很多人卡在这一步，前面的功夫全白费。

#### 面数预算参考（三角形数）

| 资产类型 | 移动端 | PC / 主机 | UE5 Nanite |
| --- | --- | --- | --- |
| 手持道具（枪、刀） | 1,500 | 4,000 | 不适用（骨骼） |
| 环境道具（桶、箱） | 800 | 2,500 | 不限（但仍要 LOD0 兜底） |
| 载具车身 | 8,000 | 25,000 | 不限 |
| 主角角色 | 3,000 | 10,000 | 不适用（骨骼） |

> 数值是业界常见参考区间，具体以你项目的性能预算为准。**先做小道具**练手。

#### 导出前检查清单（每个资产都过一遍）

```
[ ] Ctrl+A → Apply Rotation & Scale（Scale 必须是 1,1,1，Rotation 归零）
[ ] 原点位置正确（道具：底部中心；角色：脚下；门：铰链处）
[ ] 面数在预算内
[ ] UV 已展开、已排布、无重叠（若烘焙光照）
[ ] 材质只用 Principled BSDF + 贴图，无 Blender 专有节点
[ ] 修改器需要烘焙进网格的已 Apply（或导出时勾 Apply Modifiers）
[ ] 清理：M → By Distance 合并重叠顶点；Shift+N 重算法线；删掉所有空物体/参考图
[ ] 命名规范：类型_名称_变体_序号（如 Prop_Crate_Wood_01）
```

#### GLB（glTF 2.0）导出预设 —— **静态资产首选**

```
Format:            glTF Binary (.glb)     ← 单文件，贴图内嵌
Include → Limit to: Selected Objects      ← 只导出你选的
Include → Apply Modifiers: ON             ← 必须，否则 SubD/Bevel 全丢
Transform → +Y Up: ON                     ← 匹配 Unity / Godot / UE
Geometry → UVs: ON
Geometry → Normals: ON
Geometry → Tangents: OFF                  ← 引擎导入时会重算，导出来反而容易出接缝
Compression (Draco): ON, level 6          ← 体积降 40–70%（需确认目标引擎支持解码）
Animation: OFF（静态道具）
```

#### FBX 导出预设 —— **骨骼动画资产用这个**

静态道具优先 GLB；**带骨骼层级的角色/动画资产**用 FBX 更稳（5.0 重做了骨骼轴向处理）。
导出时按需设 Forward / Up 轴向与缩放，其余同检查清单。

#### 各引擎导入要点

| 引擎 | 单位/轴向 | 要点 |
| --- | --- | --- |
| **Godot 4** | 与 Blender 一致（米，Y-up） | GLB 是原生格式，拖进 FileSystem 即可；含多物体时 import mode 选 **Scene** 而非 Mesh，才能还原层级 |
| **Unity (URP / Unity 6)** | 米，与 Blender 一致 | 原生支持 GLB；颜色发灰去查 `Project Settings → Player → Color Space → Linear`；pbrMetallicRoughness 自动映射 URP Lit |
| **Unreal Engine 5** | **厘米**（GLB 是米） | 导入时 **Import Uniform Scale = 100**，或在 Blender 里把场景单位设 0.01 再导出；高模静态网格可开 Nanite 免做 LOD |
| **Bevy** | 米，Y-up | glTF 2.0 是官方推荐格式，用 GLB；材质映射到 `StandardMaterial`，复杂节点会丢——**保持 Principled 纯净** |
| **Cocos Creator** | 需实测 | 支持 glTF/FBX；轴向与单位建议先拿 1 个测试资产验证再批量做 |

> ⚠️ 通用铁律：**在做第 2 个资产之前，先拿第 1 个资产跑通导出→导入→检查的完整链路。**
> 管线没验证就量产，是游戏美术最常见的翻车方式。

---

### Stage 7 · 场景组装 + 几何节点 + 作品集（20–30h）

**一句话目标**：多个资产组合成一个可信的场景，并把它变成能展示的东西。

**必学清单**
- [ ] Asset Browser + 资产库：把做好的道具变成可复用资产（5.2 支持远程资产库托管）
- [ ] 实例化 vs 复制：大量重复道具用实例
- [ ] 几何节点入门：散布、随机化旋转缩放、地形散布植被（5.0 新增 Scatter on Surface 也能干一部分）
- [ ] 空间分组：按区域分组而不是按类型分组（利于视锥剔除）
- [ ] 灯光与出图：三点光、HDRI（Poly Haven）、EEVEE 快速出图、Cycles 出精图
- [ ] 作品集呈现：转台渲染 + 线框 + UV 检查图 + 面数标注

**练习项目**
- **场景 #1：废弃仓库角落**（4–6 个道具 + 灯光 + 一个相机视角的出图）
- **场景 #2：户外岩石地形**（几何节点散布 + 有机资产）

**验收标准**
- 场景里 80% 的道具是你自己做的（少量用免费素材补）
- 能在引擎里跑起来且帧率正常
- 输出 3 张图：最终渲染 + 线框 + 贴图/材质展示

**推荐资源**
- 付费：GameDev.tv · *Blender Environment Artist*（Grant Abbitt / Rick Davidson，适配 5.1，4.7 分）
- 付费：GameDev.tv + Rob Tuytel（Poly Haven 作者）· *Blender Environment Artist* 32h 版
- 免费：YouTube · Erindale / Harry Blends / CG Matter（几何节点三巨头）
- 免费：YouTube · Kaizen「The Power of LIGHTING in Blender」

---

## 4. 节奏模板：按你能投入的时间选一档

| 档位 | 每周 | 走完 8 阶段 | 每周分配建议 |
| --- | --- | --- | --- |
| 🐢 宽松 | 5–8h | 约 5–6 个月 | 工作日 2 × 45min，周末 1 次 2–3h 整块 |
| 🚶 标准 | 10–15h | 约 3 个月 | 每天 1.5–2h，周末留一次 3h 做项目 |
| 🚀 冲刺 | 20h+ | 约 6–7 周 | 每天 3h+，但**每周至少留半天休息**，肌肉记忆需要睡眠巩固 |

**通用的每周配比（无论哪档）**

- **70% 做项目**：跟着练习项目实际动手。看教程 ≠ 学会
- **20% 学新知**：看课程 / 文档，学本周要用到的新概念
- **10% 复盘**：回看上周的模型，找出 3 个能改进的地方；把踩过的坑写进笔记（你已有的 4 篇笔记就是这个习惯的产物，继续保持）

**番茄节奏**：单次不超过 45 分钟就切一次；建模是高度依赖手眼协调的活，疲劳后做的东西第二天基本要返工。

---

## 5. 快捷键进阶路线（从你已经会的往下接）

你已经掌握：`G/R/S`、`Tab`、`1/2/3`、`E`、`I`、`Ctrl+R`、`Ctrl+B`、`Ctrl+A`、`F3`

| 阶段 | 新增快捷键 | 用途 |
| --- | --- | --- |
| Stage 0 | `G G` / `Alt+LMB` / `Ctrl+Alt+LMB` | 边滑动 / 选环 / 选圈 |
| Stage 0 | `Ctrl+E`（边菜单）→ Mark Sharp / Mark Seam | 标记硬边与缝合边 |
| Stage 1 | `Ctrl+Shift+B`（顶点倒角）/ `L`（选连通）/ `Shift+G`（选相似） | 批量选择与清理 |
| Stage 2 | `Alt+D`（链接复制）/ `Shift+Ctrl+Alt+C`（设原点） | 实例与原点管理 |
| Stage 3 | `U`（UV 菜单）/ `Ctrl+E → Mark Seam` | UV 全流程 |
| Stage 4 | `Ctrl+Shift+LMB`（Node Wrangler 预览节点） | 材质调试 |
| Stage 6 | `Shift+N`（重算法线）/ `M → By Distance` | 导出前清理 |

> 每周只新增 3–5 个，不要一次背一堆。

---

## 6. 资源总表

### 免费（够走完全程）

| 类型 | 资源 |
| --- | --- |
| 入门 | Blender Guru · Donut 系列（4.x 版，经典起点） |
| 硬表面 / 游戏资产 | YouTube · **Grant Abbitt**（含 Detailed Game Assets 系列）、**Imphenzia**、**Blender Secrets**、**Josh Gambrell** |
| 雕刻 | YouTube · Grant Abbitt、Ryan King Art、Keelan Jon |
| 贴图 | YouTube · Grant Abbitt「Node School: Materials」、SouthernShotty「How PROS Texture」 |
| 几何节点 | YouTube · Erindale、Harry Blends、CG Matter、Ducky 3D |
| 灯光 / 渲染 | YouTube · Kaizen、Creative Shrimp（EEVEE 写实） |
| 色彩管理 | YouTube · Christopher 3D（Blender 5.0 ACES 2.0） |
| 动画 / 绑定 | YouTube · Pierrick Picaut (P2DESIGN)、Dikko |
| 文档 | Blender 官方手册 · https://docs.blender.org/manual/en/latest/ |
| 资源聚合 | awesome-blender · https://github.com/agmmnn/awesome-blender |
| 免费素材 | Poly Haven（HDRI/贴图）、ambientCG（PBR 材质） |

### 付费（按性价比排序）

| 课程 | 时长 | 适合 | 备注 |
| --- | --- | --- | --- |
| CG Cookie · *Press Start: Making a Game-Ready Asset* | 5h | **最贴你的主线** | $21/月订阅内 |
| CG Cookie · *Mesh Modeling Bootcamp* | 10h | 拓扑系统补强 | Kent Trammell |
| GameDev.tv · *Complete Blender Creator* | 48h | 全面打底 | Grant Abbitt + Rick Davidson，35 万学员 |
| GameDev.tv · *Blender Environment Artist* | 14.5h | 场景 / 资产 | 已适配 Blender 5.1，4.7 分 |
| GameDev.tv · *Blender Character Creator for Video Games* | 34.5h | 角色支线 | 4.8 分 |
| CG Boost · *Master 3D Sculpting / Environments* | — | 雕刻 / 场景深化 | 质量高 |
| Blender Studio | — | 官方制作流程 | $11.50/月 |

> 订阅制（CG Cookie / Skillshare）比逐个买课划算：**先订一个月，把最需要的两门刷完就退订。**

---

## 7. 防弃坑：5 个最常见的失败模式

| 失败模式 | 表现 | 解法 |
| --- | --- | --- |
| **野心过载** | 第 2 周就想做赛博机甲，做不出来就放弃 | Grant Abbitt 原话：从低模开始。**先做丑的、做完的**，比做一半的好看的强 100 倍 |
| **教程仓鼠症** | 收藏 50 个教程，一个都没看完 | 一门课没做完不许开下一门。本路线每个 Stage 只允许 1–2 个主资源 |
| **只输入不输出** | 看了 100 小时，自己动手就卡住 | 按 70/20/10 配比，**每周必须有实际产物** |
| **跳过 UV/导出** | 模型做得挺好，进引擎全废 | Stage 6 提前做：**第 1 个道具就跑通导出闭环** |
| **不记笔记** | 一周不看就全忘 | 你已经在记了。每个 Stage 结束时补一篇笔记（像已有的 4 篇那样），这是你最有效的复利 |

---

## 8. 立刻可以做的第一步（今天，1 小时）

1. 装 Blender 5.2.2 LTS（如果还没装）
2. 打开 `Preferences → Add-ons`，勾上 Node Wrangler / LoopTools / Bool Tool / F2 / Auto Mirror / Extra Objects / Import Images as Planes
3. macOS 开 `Emulate Numpad`；确认视图导航（Orbit/Pan/Zoom）用得顺手
4. 打开你的 `环切详解.md`，把第 31 节的 **5 种 Loop Cut 用法**逐个实操一遍（约 40 分钟）
5. 做完把这篇笔记补一句：「已练过，卡点：______」

走完这 5 步，Stage 0 就算正式开工了。

---

## 下一步：要我接着做哪个？

- **A** — 把 Stage 0 拆成逐周任务清单（带每天具体做什么、做多久）
- **B** — 从 Stage 1 开始，给你设计 3 个具体的练习道具（附参考图搜集方向、尺寸、面数预算）
- **C** — 设置每日/每周学习提醒 + 打卡，让计划真正跑起来
- **D** — 直接给我一个能立刻开工的项目（比如你的游戏项目需要某个具体资产，我按它倒推学习路径）

---

### 附：这份计划的信息来源

- Blender 官方发布页与版本生命周期（5.2 LTS 发布 2026-07-14，支持至 2028-07，当前 5.2.2 / 2026-09-15）
- Blender 5.0 官方 Release Notes（色彩管理、UV 同步重构、Multires 烘焙增强、Scatter on Surface）
- Blender 官方手册（Keymap / Local View / 导航）
- glTF 导出与引擎导入实践（Godot / Unity / UE5 单位与轴向处理）
- 课程与创作者资源库：Udemy Blender 分类、learn-blender.org 2026 榜单、SouthernShotty 2026 路线图、awesome-blender
- 你自己的 4 篇 Blender 笔记（用于定位当前水平）
