# FileFlow 开发需求文档

C# + WPF MVP 需求文档(v0.1)
目标:30 天交付首个可下载的单文件 EXE

──────────────────

## 项目定位

• 只做 Windows 10/11 桌面
• 轻量(< 30 MB),原生外观,毫秒级搜索,自然语言文件操作
• 后续版本再考虑跨平台,MVP 不预留抽象层

──────────────────

## 功能需求(MVP Only)

| 编号 | 功能              | 验收标准(可自动化测试)                                                                                                   |
| ---- | ----------------- | ------------------------------------------------------------------------------------------------------------------------ |
| F1   | 文件树 + 列表视图 | 启动 1 秒内显示当前目录 ≥ 10 000 文件,滚动 60 FPS                                                                        |
| F2   | 搜索              | 全盘文件名关键字 ≤ 500 ms 返回首屏结果(调用 Everything SDK)                                                              |
| F3   | 批量重命名        | 选中 1000 文件 → 添加后缀"\_bak"→Undo 可恢复                                                                             |
| F4   | 压缩/解压         | 选中文件夹 → 右键"压缩为 zip"生成同名 zip,双击 zip 可解压,若系统未检测到 7-Zip,弹窗提示用户下载或手动指定路径            |
| F5   | AI 指令           | 输入框输入"把今天修改的 pdf 移到 Docs"→ 弹出确认列表 → 一键执行,本地 Ollama 或云端 OpenAI 超时 8 s 后提示“AI 指令不可用” |
| F6   | 脚本扩展          | 菜单中选择并运行指定脚本                                                                                                 |
| F7   | 配置导入/导出     | 支持配置文件本地 JSON 导入/导出                                                                                          |

──────────────────

## 后续版本功能规划

| 编号 | 功能         | 说明                                    |
| ---- | ------------ | --------------------------------------- |
| F8   | 云同步       | 配置文件云同步(OneDrive/Google Drive)   |
| F9   | 压缩密码保护 | ZIP 压缩包密码保护功能                  |
| F10  | 脚本自动扫描 | 自动扫描 Scripts 目录并动态生成执行按钮 |

──────────────────

## 功能需求详细说明

### F1 文件树 + 列表视图

1. 文件树功能

   - 显示系统所有磁盘驱动器
   - 支持展开/折叠文件夹结构
   - 支持单击选择目录并在文件列表中显示内容
   - 支持右键菜单:刷新,展开所有,折叠所有

2. 文件列表功能
   - 显示名称,大小,类型,修改时间等列
   - 支持按列标题排序(升序/降序切换)
   - 支持大图标,小图标,列表,详细信息等多种视图模式
   - 支持键盘快捷键操作(方向键,回车,删除等)

### F2 搜索功能

1. 实时搜索

   - 输入关键字后自动触发搜索
   - 搜索结果分页显示,每页最多 100 条
   - 支持模糊匹配和精确匹配切换

2. 搜索选项
   - 支持按文件名,扩展名,大小,修改时间等条件过滤
   - 支持搜索特定目录或整个磁盘
   - 支持正则表达式匹配模式

### F3 批量重命名

1. 重命名模式

   - 支持添加前缀/后缀
   - 支持替换文本
   - 支持编号序列(001, 002, ...)
   - 支持大小写转换

2. 操作安全
   - 提供预览功能,显示重命名结果
   - 支持撤销操作
   - 冲突检测和处理(发现冲突即终止并提示)

### F4 压缩/解压

1. 压缩功能

   - 调用系统已安装的 7-Zip(或用户自选 exe)进行压缩
   - 支持 ZIP 格式压缩
   - 不支持密码保护(MVP 版本)

2. 解压功能
   - 调用系统已安装的 7-Zip(或用户自选 exe)进行解压
   - 双击 ZIP 文件调用外部程序解压
   - 不支持密码保护(MVP 版本)

### F5 AI 指令

1. 指令解析

   - 支持自然语言文件操作指令
   - 支持文件筛选条件(今天,本周,文件类型等)
   - 支持移动,复制,删除等操作
   - 通过 LLM Provider Adapter 统一接口,兼容 Ollama,OpenAI 等

2. 安全机制
   - 执行前显示操作确认列表
   - 支持取消操作
   - 支持撤销操作

### F6 脚本扩展

1. 脚本执行

   - 通过菜单选择并运行指定脚本
   - 支持 IronPython 脚本执行
   - 支持访问选中文件列表
   - 显示脚本执行输出

### F7 配置导入/导出

1. 配置导出/导入

   - 支持导出所有用户配置到 JSON 文件
   - 支持从 JSON 文件导入配置
   - 导入时提供配置预览

──────────────────

## 非功能需求详细说明

### 性能要求

1. 启动性能

   - 冷启动时间不超过 800ms(从双击图标到主界面完全显示)
   - 热启动时间不超过 500ms(从任务栏或开始菜单启动)

2. 内存使用

   - 空闲状态下内存占用不超过 50MB
   - 显示 10 万文件列表时内存占用不超过 200MB
   - 运行 24 小时后内存泄漏不超过 10MB

3. 响应性能
   - UI 操作响应时间不超过 100ms
   - 文件操作(复制,移动,删除)进度更新频率不低于 10Hz

### 安装部署

1. 安装方式

   - 提供单文件 EXE 安装包
   - 双击 EXE 文件直接运行,无需安装步骤
   - 不需要管理员权限即可运行
   - 不在注册表中写入任何信息

2. 兼容性
   - 支持 Windows 10 1909 及以上版本
   - 支持 Windows 11 所有版本
   - 支持 x64 和 ARM64 架构

### 用户界面

1. 外观设计

   - 默认跟随 Windows 系统主题(浅色/深色模式)
   - 提供 Win11 风格开关选项
   - 界面元素符合 Fluent Design 设计语言

2. 交互体验
   - 所有操作都有明确的视觉反馈
   - 支持鼠标和键盘操作
   - 支持高 DPI 显示器

### 稳定性

1. 错误处理

   - 文件操作失败时显示友好的错误信息
   - 程序异常时自动生成错误报告
   - 关键操作前进行权限检查

2. 撤销机制
   - 支持文件操作撤销(移动,删除,重命名等)
   - 最多支持 10 步撤销操作
   - 撤销操作成功率 100%

### 安全性

1. 数据安全

   - 不会上传任何用户文件到云端
   - AI 模型完全本地运行(可选)
   - 用户配置文件可选加密存储

2. 系统安全
   - 不修改系统关键设置
   - 不安装驱动程序
   - 不添加系统服务

──────────────────

## 技术约束详细说明

### 编程语言和框架

1. C# 12 + .NET 8

   - 使用最新语言特性提升开发效率
   - 利用 .NET 8 的 AOT 编译优化性能
   - 保证长期支持和安全性更新

2. WPF + ModernWpfUI
   - 使用 WPF 的数据绑定和 MVVM 模式
   - ModernWpfUI 提供现代化 Fluent Design 外观
   - 支持 Windows 10/11 原生视觉效果

### 核心功能组件

1. Everything SDK

   - 通过 DllImport 调用本地 DLL
   - 实现毫秒级全盘文件名搜索
   - 提供高性能搜索接口

2. 7-Zip 外部调用

   - 通过进程调用系统已安装的 7-Zip
   - 支持用户自定义压缩工具路径
   - 无需内嵌压缩库,减少程序体积

3. LLM Provider Adapter

   - 统一 AI 接口适配器
   - 兼容 Ollama,OpenAI 以及后续任意实现
   - 接口保持零改动即可热插拔

4. IronPython 3.4
   - 嵌入 Python 脚本执行环境
   - 允许用户扩展功能
   - 无需用户安装额外 Python 环境

### 打包和部署

1. dotnet publish
   - 使用 PublishProfile 配置单文件打包
   - 支持 win-x64 和 win-arm64 平台
   - 自动生成独立可执行文件

──────────────────

## 数据结构 & 关键接口

```csharp
public record FileItem(string FullPath, long Size, DateTime Modified);

public interface IFileManager
{
    Task<IReadOnlyList<FileItem>> SearchAsync(string keyword);
    Task RenameBatchAsync(IEnumerable<FileItem> files, string pattern);
    Task CompressAsync(IEnumerable<string> paths, string destZip);
    Task MoveAsync(IEnumerable<string> sources, string destDir);
    Task UndoAsync();               // 最近 10 步操作栈
    Task<bool> ExportConfigAsync(string path);  // 导出配置
    Task<bool> ImportConfigAsync(string path);  // 导入配置
}

public interface ILlmProvider
{
    Task<string> GenerateAsync(string prompt);
}

public class OllamaProvider : ILlmProvider { }
public class OpenAiProvider : ILlmProvider { }
```

──────────────────

## 用户界面草图(XAML 伪代码)

```XAML
<modern:MetroWindow>
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="250"/> <!-- 目录树 -->
            <ColumnDefinition Width="*"/>   <!-- 文件列表 -->
            <ColumnDefinition Width="300"/> <!-- 预览/脚本面板 -->
        </Grid.ColumnDefinitions>

        <TreeView x:Name="FolderTree" ItemsSource="{Binding Roots}"/>
        <ListView x:Name="FileList" ItemsSource="{Binding Files}" GridViewColumnHeaderClick="Sort"/>
        <DockPanel Grid.Column="2">
            <Menu DockPanel.Dock="Top">
                <MenuItem Header="工具">
                    <MenuItem Header="导出配置" Command="{Binding ExportConfigCmd}"/>
                    <MenuItem Header="导入配置" Command="{Binding ImportConfigCmd}"/>
                    <MenuItem Header="运行脚本" Command="{Binding RunScriptCmd}"/>
                </MenuItem>
            </Menu>
            <TextBox DockPanel.Dock="Top" Placeholder="AI 指令…" KeyDown="OnAiInput"/>
            <ContentControl Content="{Binding Preview}"/>
        </DockPanel>
    </Grid>
</modern:MetroWindow>
```

──────────────────

## 测试策略详细说明

### 单元测试

1. 核心接口测试

   - 使用 xUnit 框架覆盖 [IFileManager](file:///E:/ToDo/FileFlow/doc/IFileManager.cs#L35-L35) 全部接口
   - 每个方法至少包含 3 个测试用例(正常,边界,异常)
   - 测试覆盖率目标 90% 以上

2. 功能模块测试
   - 搜索功能测试:关键字匹配,性能基准
   - 重命名功能测试:各种模式,冲突处理
   - 压缩解压测试:调用外部程序
   - AI 指令测试:常见指令,错误输入
   - 配置导入导出测试

### UI 自动化测试

1. Appium-WindowsDriver

   - 覆盖主要用户操作流程
   - 验证搜索,重命名,撤销等关键功能
   - 测试不同 Windows 主题和 DPI 设置

2. 兼容性测试
   - Windows 10/11 不同版本
   - 不同屏幕分辨率和缩放比例

### 性能测试

1. BenchmarkDotNet

   - 对比 10 万条虚拟化 ListView 滚动 FPS
   - 测试搜索响应时间和内存占用
   - 评估外部程序调用性能

2. 压力测试
   - dotMemory 连续 1 小时压力测试
   - 监控内存使用和泄漏情况
   - 测试大量文件操作的稳定性

──────────────────

## 交付物详细说明(上线前必须全部 ✅)

### 软件交付物

1. 单文件 EXE(win-x64)

   - 使用 .NET 8 AOT 编译生成
   - 包含所有依赖项,无需额外安装
   - 支持 Windows 10/11 系统

2. 安装和使用文档
   - README.md(下载链接 + 动图演示)
   - 包含系统要求,安装步骤,基本使用说明
   - 常见问题解答

### 多媒体交付物

1. 演示视频
   - 3 分钟安装/使用视频
   - 展示主要功能和操作流程
   - 包含高清和压缩两个版本

### 开源交付物

1. GitHub 发布

   - GitHub Release Tag v0.1.0
   - 包含完整的源代码和构建脚本
   - 提供详细的 CHANGELOG

2. 社区支持
   - Issues 模板(Bug / Feature / AI 指令示例)
   - 提供贡献指南和代码规范
   - 建立社区讨论渠道

──────────────────

## 详细里程碑计划(4 周冲刺)

### Week 1: 项目骨架 + 文件树/列表

目标:完成基础框架和核心文件浏览功能

任务分解:

1. 项目初始化

   - 创建解决方案和项目结构
   - 配置 ModernWpfUI 和基本样式
   - 设置 MVVM 模式基础架构

2. 文件树实现

   - 实现磁盘驱动器检测和显示
   - 开发文件夹展开/折叠功能
   - 添加右键菜单支持

3. 文件列表实现
   - 开发多种视图模式(图标,列表,详细信息)
   - 实现列排序功能
   - 优化大数据量显示性能(虚拟化)

交付物:

- 可运行的基础应用
- 文件浏览功能
- 基本界面交互

### Week 2: Everything 搜索 + 批量重命名

目标:集成高性能搜索和批量操作功能

任务分解:

1. Everything SDK 集成

   - 集成 Everything SDK DLL
   - 开发搜索接口和结果显示
   - 优化搜索性能和用户体验

2. 批量重命名功能
   - 实现多种重命名模式
   - 开发预览和冲突检测功能(发现冲突即终止并提示)
   - 集成撤销机制

交付物:

- 高性能全文搜索功能
- 完整的批量重命名工具
- 相关单元测试

### Week 3: 外部压缩调用 + AI 指令原型

目标:实现外部压缩工具调用和 AI 指令解析

任务分解:

1. 外部压缩工具调用

   - 开发 7-Zip 等外部程序调用接口
   - 实现压缩和解压功能
   - 支持用户自定义工具路径

2. AI 指令原型
   - 开发 LLM Provider Adapter 接口
   - 实现 Ollama 和 OpenAI 适配器
   - 开发自然语言指令解析器

交付物:

- 外部压缩工具调用功能
- AI 指令解析原型
- 相关性能测试

### Week 4: IronPython 脚本 + AOT 打包 + 发布

目标:实现脚本扩展功能并完成产品发布

任务分解:

1. IronPython 脚本支持

   - 集成 IronPython 3.4
   - 实现菜单选择脚本执行
   - 开发脚本安全管理机制

2. 配置导入导出功能

   - 实现 JSON 配置导入导出
   - 开发配置预览功能

3. AOT 打包和发布
   - 配置 .NET 8 AOT 编译
   - 生成单文件 EXE
   - 完成所有测试和文档

交付物:

- 完整可发布的 v0.1.0 版本
- 所有文档和多媒体材料
- GitHub Release 发布
