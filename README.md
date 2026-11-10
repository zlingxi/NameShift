<div align="center">

<img src="assets/banner.jpg" alt="NameShift — 一款轻量的批量重命名工具" width="720"/>

# NameShift

**一款免费的跨平台批量重命名工具 —— 轻至 1.5 MB。改之前先看到结果，改错了随时回头。**

基于 Tauri 2 + Vue 3 + TypeScript + Rust 构建，支持 **Windows 与 macOS**。
项目以"学习规范的桌面应用分层架构"为首要目标，前后端职责严格分离。

[![Tauri](https://img.shields.io/badge/Tauri-2.x-24C8DB?logo=tauri&logoColor=white)](https://v2.tauri.app/)
[![Vue](https://img.shields.io/badge/Vue-3.5-4FC08D?logo=vuedotjs&logoColor=white)](https://vuejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.x-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Rust](https://img.shields.io/badge/Rust-2021-DEA584?logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![Pinia](https://img.shields.io/badge/Pinia-4.x-F7D336?logo=pinia&logoColor=white)](https://pinia.vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-8.x-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Vitest](https://img.shields.io/badge/Vitest-5.x-6E9F18?logo=vitest&logoColor=white)](https://vitest.dev/)

[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS-blue)](#)
[![Version](https://img.shields.io/badge/Version-1.1.0-green)](#)
[![Tests](https://img.shields.io/badge/Tests-93%20passing-brightgreen)](#测试)
[![License](https://img.shields.io/badge/License-学习与个人使用-orange)](#许可)

**🚧 源码整理中，即将开源**

</div>

---

## 🌟 亮点速览

- **改前先看到，改错能回头** —— 任何参数改动实时刷新"修改前 → 修改后"；两阶段事务执行，失败自动逆序回滚；撤销日志持久化到磁盘，重启应用后仍可一键撤销
- **字符级改动高亮** —— 红 = 删除、绿 = 新增、橙 = 替换、紫点线 = 仅大小写（GitHub 式 diff），一眼看清这次改了哪几个字
- **扩展名徽章 9 色系** —— 图片 / 设计 / 视频 / 音频 / 文档 / 表格 / 压缩包 / 代码 / 字体按类型着色，深浅分档区分具体格式，混合目录一眼归堆
- **可串联的规则链** —— 六大模式按固定顺序一次执行（查找替换 → 删减 → 新增 → 大小写 → 序号），也可单独生效；参数互不覆盖、自动记忆
- **CSV 往返** —— 把「新文件名」列丢进 Excel / WPS 批量编辑再导回，按「目录 + 当前文件名」精确匹配，匹配不到的行绝不猜测对位
- **Windows 右键菜单呼出** —— 不用先打开软件：资源管理器里选中 → 右键改名，文件夹自动递归展开，进来就是实时预览
- **轻至 1.5 MB** —— Tauri 2 原生壳，没有浏览器内核；免费、无广告、永久可用
- **Windows / macOS 双平台** —— 同一套交互与规则记忆；中英双语、暗色主题、内置帮助中心

---

## ✨ 功能总览

### 六大重命名模式 · 可串联的规则链

每个模式底部都有独立的「启用此规则」滑动开关：启用多个规则时按固定顺序
（查找替换 → 删减 → 新增 → 大小写 → 序号）**一次执行串联生效**；
**全部未启用时由当前标签单独生效**（单模式行为）；手动编辑模式独占。

| 模式 | 能力 |
|---|---|
| **查找替换** | 纯文本替换（全部命中）；正则表达式模式（支持 `$1` 捕获组，语法错误即时提示）；区分大小写开关；扩展名默认保护，可选一并替换 |
| **删减内容** | 四种子操作——**按位置**（删开头/结尾 N 个字符）、**按范围**（删第 M~N 个字符）、**按关键词**（全部 / 仅第一处 / 仅最后一处，可区分大小写）、**按类型**（删数字 / 英文字母 / 空格） |
| **新增内容** | 五种插入位置——名称前 / 名称后 / 指定字符位置 / 关键词前 / 关键词后（忽略大小写取第一处）；始终插在扩展名之前 |
| **大小写** | 全部大写 / 全部小写 / 每词首字母大写；可选包含扩展名 |
| **序号格式** | 模板占位符 `{index}`（序号）、`{name}`（当前主名）、`{date}`/`{time}`（文件修改时间）、`{parent}`（父目录名）、`{ext}`（原扩展名）；起始编号 / 步长 / 位数补零（0–6 位）；"每个文件夹重新计数" |
| **手动编辑** | 在预览表"修改后"列直接逐行输入，支持单行还原与一键全部还原 |

> 标签从左到右即规则链执行顺序（先整形内容 → 再拼接 → 再统一大小写 → 最后编号）；
> 排序依据：「文件名」按目录分组自然排序；「修改时间」为全局时间线排序（跨目录，照片按时间编号；
> 勾选「每个文件夹重新计数」时自动退回目录分组保证编号连续）；各模式参数独立保存、互不覆盖。

### 🌈 改动可视化（字符级 diff + 类型徽章）

- **字符级 diff 高亮**：预览表按字符标注每一处变化，多规则串联时也能逐字区分——
  🔴 红底删除线 = 被删除的内容（标在「修改前」列）｜🟢 绿底 = 新增的内容（标在「修改后」列）｜
  🟠 橙底 = 替换产生的新内容（旧内容在左列红标对照）｜🟣 紫底点线 = 仅大小写变化（内容未变，放心改）
- **扩展名徽章 9 色系**：徽章按文件类型着色——图片 / 设计 / 视频 / 音频 / 文档 / 表格 / 压缩包 / 代码 / 字体；
  同类同色系、深浅分档区分具体格式（JPG / PNG / GIF 由深到浅）；表外扩展名 FNV-1a 哈希自动取色，无扩展名灰色占位
- **内置帮助中心**：右上角「?」随时查看上述图例与全部策略说明（规则链顺序、排序过滤、导入选项），
  首次启动红点引导，看完即散

### 🎯 参与范围

- **扩展名过滤**：如输入 `jpg, png`，仅命中的扩展名参与改名，其余行原地标注跳过
- **文件夹改名**：导入选项勾选「仅文件夹」进入专用模式——只能添加文件夹、文件自动跳过，
  拖/选的文件夹本身成为条目（配合「含子目录」递归纳入子层级文件夹）；
  所在文件夹本批次将改名的条目自动跳过并说明，防止路径失效
- **列表勾选**：仅用于标记待移除（底栏「移除选中」批量删），不影响改名范围
- 行级快捷操作：右键菜单支持从清单移除 / 打开所在位置 / 复制文件名 / 复制完整路径
- 清单工具条：排序切换、扩展名过滤、**导出/导入 CSV**、**刷新清单**（与磁盘同步，剔除已不存在的条目）、参与统计

### 📄 CSV 往返（Excel 批量编辑手动命名）

清单工具条的「导出 CSV / 导入 CSV」让手动改名走出应用窗口：

1. **导出**：当前清单导出为 CSV（`序号 / 目录 / 当前文件名 / 新文件名` 四列，
   新文件名列 = 当前生效的新名），UTF-8 带 BOM + CRLF，Excel / WPS 双击打开不乱码
2. **编辑**：在表格软件里修改「新文件名」列（可用公式、粘贴外部数据源、条件格式辅助核对）；
   新名留空 = 该行不改动
3. **导入**：按「目录 + 当前文件名」精确匹配（大小写不敏感），全量回填**手动编辑**模式，
   自动切换到该模式；未匹配的行跳过并在确认框与结果提示中说明；
   撞名 / 非法字符照常由实时预览标红

> 导入要求 UTF-8 编码（Excel 请用「CSV UTF-8」格式另存）；表头中英文均可识别，
> 用户自行追加的备注列会被忽略；纯数字文件名注意表格软件可能吞掉前导零。
> 匹配不到的行不会被猜测对位，保证改名对象永远准确。

### 🔒 安全与效率

- **实时预览**：任何参数改动立刻刷新"修改前 → 修改后"，逐行标注状态（可改名 / 未变化 / 冲突 / 非法）
- **冲突检测**：批次内撞名、与目录内现有文件重名（统一忽略大小写，兼容 Win/mac 默认文件系统）
- **非法名校验**：按当前系统校验非法字符（Windows `\ / : * ? " < > |`、macOS `/ :` 等）；Windows 保留设备名（CON、NUL、COM1-9…）与尾随点/空格；在非 Windows 平台上对含 Windows 非法字符的名字给出**跨平台可移植性警告**
- **扩展名保护**：文本类模式默认只处理主文件名，勾选后才允许改动扩展名
- **两阶段事务执行**：目标名冲突的操作先挪临时名再落最终名（支持 `A↔B` 互换、仅大小写改名）；任一步失败自动**逆序回滚**，目录保证回到初始状态
- **一键撤销**：撤销日志**持久化到磁盘**，重启应用后仍可撤销最近批次；部分失败自动保留待重试
- **可选执行前确认**：设置开启后，按钮与 `Ctrl+Enter` 均先弹确认框

### 📂 文件导入

- 全窗口**拖拽**导入，或按钮选择文件 / 文件夹
- 文件夹支持**递归子目录**与**包含隐藏文件**开关；**仅文件夹**模式专用于批量重命名文件夹
- 重复路径自动去重；文件按目录分组、目录内按当前排序方式展示
- 快捷键：`Ctrl/Cmd+O` 添加文件、`Ctrl/Cmd+Shift+O` 添加文件夹、`Ctrl/Cmd+Enter` 重命名、`Ctrl/Cmd+Z` 撤销、`Ctrl/Cmd+A` 全选/全不选

### 🖱 不用先打开软件 —— Windows 右键菜单

安装后在设置里一键把 NameShift 注册进资源管理器右键菜单（可随时移除）：

1. **选中即达**：选中文件或文件夹 → 右键「用 NameShift 重命名」，软件就位，不用先开软件、也不用拖拽
2. **文件夹自动展开**：选中的是文件夹时，递归收集全部子文件——包括子文件夹里的那些，按目录分组陈列、序号各自计数
3. **进来就能预览**：导入后直接进入实时预览表，规则没下之前磁盘上一个字节都不动，冲突与非法名提前拦下

### 🎨 体验细节

- **暗色 / 亮色主题**：首次启动跟随系统偏好，一键切换并记忆；原生标题栏与应用主题同步（Windows 11 DWM 精确着色 / macOS 原生标题栏）
- **中英双语界面**：设置中切换（跟随系统 / 简体中文 / English），macOS 应用菜单栏同步本地化
- **内置帮助中心**：右上角「?」打开，目录式图例（改动颜色 / 扩展名徽章 / 行状态 / 规则策略 / 排序过滤 / 导入范围），首次启动红点引导
- **工具栏语义分组**：右上角图标按「求助信息 ｜ 应用偏好 ｜ 支持作者」分组排列，分隔线隔开
- **界面图标全矢量**：SVG 图标（`AppIcon.vue`），不依赖系统字体字形，两平台渲染一致
- 版本号显示在窗口标题与"关于"弹窗
- 全部参数、导入选项与清单偏好自动持久化
- 长清单渲染优化（`content-visibility`），千级文件流畅滚动

## 🖼 界面预览

启动后即见：顶部为导入操作与主题/更多菜单，模式标签页切换六种规则，中间大表为"修改前 / 修改后"实时预览，底部按作用域分组的操作区（清单操作 │ 撤销 / 重命名）。

| 亮色主题 | 暗色主题 |
|:---:|:---:|
| ![查找替换](assets/screenshot-light-find-replace.png) | ![新增内容-暗色](assets/screenshot-dark-insert.png) |

**实时预览与状态标注**——逐行显示"修改前 → 修改后"，右列标注该行是否可改名：

![实时预览](assets/screenshot-light-preview.png)

## 🛠 技术栈

| 层次 | 技术 | 说明 |
|---|---|---|
| 桌面框架 | [Tauri 2](https://v2.tauri.app/) | 轻量壳 + 系统 WebView，产物体积远小于 Electron |
| 前端框架 | [Vue 3.5](https://vuejs.org/) `<script setup>` | 组合式 API |
| 状态管理 | [Pinia 4](https://pinia.vuejs.org/) | files / rules / theme / app / toast 五个 store |
| 语言 | TypeScript 6 + Rust | 前端类型完备，后端安全系统级执行 |
| 构建 | [Vite 8](https://vitejs.dev/) | 开发热更新 + 生产构建 |
| 测试 | [Vitest 5](https://vitest.dev/) / `cargo test` | 前端 44 例 + 后端 49 例 |
| 插件 | `@tauri-apps/plugin-dialog` / `plugin-opener` | 系统对话框与打开文件/目录 |

## 🚀 开发

```bash
npm install          # 安装前端依赖
npm run tauri dev    # 启动桌面应用（自动起 Vite + cargo run）
npm run test         # 前端核心层单测（Vitest）
npm run build        # 类型检查 + 生产构建
cd src-tauri && cargo test   # Rust 后端单测
```

> 只想起浏览器里调 UI？直接 `npm run dev`，应用会自动使用 mock 后端（`src/services/mock.ts`），
> 内置一组示例文件，无需 Rust 环境。

> **编译产物位置**：Rust 产物与前端构建输出统一在源码树外的 `G:\My_Develop\NameShift\build\`
> （`target\` 与 `dist\`），由父级 `.cargo/config.toml` 的 `target-dir` 和 `vite.config.ts` 的
> `outDir` 指定；删除父级 `.cargo` 目录即回退到默认的 `src-tauri/target`。

## 🏗 架构（学习重点）

```
┌─ 前端 src/ ─────────────────────────────────────┐
│ components/   纯展示组件（工具栏/标签页/清单条/表格/底栏）│
│ stores/       Pinia：files(清单) rules(参数+链开关)  │
│               theme(主题) app(元信息) settings toast
│ composables/  组合逻辑：usePreview useRename useDragDrop│
│ services/     IPC 唯一边界：ipc.ts(契约) tauri.ts(真实现) mock.ts(浏览器)
│ core/         纯函数领域层，不依赖 Vue/Tauri        │
│   rules/      六个模式各一个纯函数，统一签名         │
│   preview.ts  参与范围过滤 + 规则链编排 + 校验        │
│   validate.ts 校验；version.ts 版本比较              │
│ i18n/         vue-i18n：zh-CN / en 语言包            │
│ types/        领域与 DTO 类型                       │
└──────────────────────────────────────────────────┘
                 ↕ invoke（camelCase DTO）
┌─ 后端 src-tauri/src/ ────────────────────────────┐
│ commands.rs  Tauri 命令薄壳（只转发）               │
│ fs_ops.rs    目录扫描（递归/隐藏/仅文件夹模式/mtime）  │
│ rename.rs    两阶段执行器 + 失败回滚（文件与目录）    │
│ journal.rs   撤销日志栈 + 磁盘持久化（跨会话撤销）    │
│ titlebar.rs  标题栏主题化（深浅 + Win11 精确色）     │
│ menu.rs      macOS 应用菜单栏（中/英双语重建）        │
│ validate.rs  合法性二次校验（真正的安全边界）         │
│ error.rs     可序列化统一错误                      │
└──────────────────────────────────────────────────┘
```

### 关键设计决策

1. **前端算预览、Rust 执行改名**。预览走纯函数（`core/`），敲键盘零延迟刷新；
   Rust 在执行前用同一套规则**二次校验**——前端校验只管体验，不是安全边界。
2. **文件名拆成 `stem + ext` 处理**，规则函数统一签名
   `(ctx: {stem, ext, dirIndex, globalIndex}, params) => {stem, ext}`。
   新增模式 = 新增一个纯函数 + `RuleParams` 加字段 + `rules/index.ts` 注册三步
   （"删减内容"模式即是按此三步接入的，后端零改动）。
3. **两阶段重命名**（`rename.rs`）：先给"目标位被占用"（磁盘已有文件，或目标是批次内
   其他操作的源名）的操作改临时名 `~ns_tmp_<uuid>`，第二阶段统一落到最终名；
   每一步都记入回滚日志，失败时逆序还原，目录保证回到初始状态。
4. **IPC 契约化**（`services/ipc.ts`）：store 只依赖接口。浏览器里自动切到 mock 实现，
   UI 可以脱离 Rust 独立调试 / 自动化测试。
5. **占用名动态腾挪**：预览时"本批次即将改走"的名字会从占用集合中扣除，
   所以 A→B、B→A 互换和仅大小写改名都能正确放行。

### 🧪 测试

- 前端：**44** 个 Vitest 用例覆盖 CSV 往返（编解码 / 表头中英识别 / 匹配回填）、
  扩展名徽章配色（语义类别查表 + 哈希兜底 + 别名归并 + 档位分布）、
  字符级 diff（LCS 对齐 / 大小写感知 / 片段合并 / 超长名护栏 / 两列渲染视图）。
- 后端：**49** 个 cargo test 覆盖扫描（mtime/仅文件夹模式）、执行器（成功/互换/仅大小写/
  目录改名/冲突回滚/批次重名/撤销）、撤销日志持久化往返与合法性校验。

## 📦 打包

### 本机一键：`npm run dist`

统一发布入口（三平台同名命令）。**在哪台机器运行，就产出该平台全部可得的架构产物**，
默认自动检测已安装的 Rust target 组成构建矩阵：

| 平台 | 默认矩阵 | 产物（统一命名 NameShift-{版本}-{平台}-{架构}） |
|---|---|---|
| Windows | x64（宿主）+ arm64（已装 target 时） | `NameShift-1.1.0-Windows-x64-setup.exe` / `…-Windows-arm64-setup.exe` |
| macOS | universal 单包（需双 target，未装回退宿主架构） | `NameShift-1.1.0-MacOS-universal.dmg` |
| Linux | 宿主架构（跨架构请用 CI arm runner） | `NameShift-1.1.0-Linux-x64.deb` / `…-Linux-x64.AppImage` |

- 打包目标按平台配置文件声明：`tauri.windows.conf.json`（NSIS）、
  `tauri.linux.conf.json`（deb + AppImage）、`tauri.macos.conf.json`（`["app", "dmg"]`），
  与 `tauri.conf.json` 按 RFC 7396 自动合并
- 显式指定架构：`npm run dist -- --target aarch64-pc-windows-msvc`（单 target 模式，跳过矩阵）
- tauri build 统一经 `npm run tauri --` 调用，与 `npm run tauri build` 同一入口
  （pretauri 端口清理钩子同样生效）
- 产物重命名：`scripts/rename-artifacts.mjs`（Tauri v2 无构建后钩子、无命名模板）

### 云端全平台：GitHub Actions（`.github/workflows/release.yml`）

Rust GUI 无法跨操作系统交叉编译（GTK / WebView2 系统依赖），"一个命令出全平台"
由 CI 完成：**push `v*` 标签（或手动 workflow_dispatch）** 即在四个 runner 并行构建并
自动挂到 GitHub Release：

| Runner | 产物 |
|---|---|
| windows-latest | Windows x64 + arm64（MSVC 交叉，单 runner 双架构） |
| macos-15 | macOS universal（Intel + Apple Silicon 单包） |
| ubuntu-latest | Linux x64（deb + AppImage） |
| ubuntu-24.04-arm | Linux arm64（官方免费 arm runner） |

### 跨平台构建迁移清单

把源码放到对应平台上构建（Windows / macOS 各自构建各自的包，无法交叉编译）：

1. 只拷源码目录；**不要拷** `node_modules\`（平台相关，到新平台重新 `npm install`）与 `build\`（旧平台产物）
2. **父目录的 `.cargo\config.toml` 记得一起带**，放到项目目录的上一级——它负责把编译产物迁出源码树；
   不带也能构建（自动回退到项目内 `src-tauri/target`），只是源码树会再次出现 `target` 目录
3. macOS 前置一次性安装：Node.js、Rust（rustup）、`xcode-select --install`；macOS 打包用系统内置
   hdiutil，无需下载 WiX/NSIS
4. Windows 首次打**安装包**需联网下载 NSIS 工具集（GitHub）；仅编译 exe 可用
   `npm run tauri build -- --no-bundle` 跳过
5. 分发给他人的 macOS 应用需要 Apple 开发者证书签名与公证；本机使用未签名的 .app 右键打开即可

## 📄 许可

供学习与个人使用，永久免费。
