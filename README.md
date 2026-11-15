# Ventoy

[![License](https://img.shields.io/badge/License-Apache%202.0--Commons%20Clause-blue)](LICENSE)

这是一个 Ventoy 配置仓库，包含用于自动化 Windows 10/11 安装的配置文件和脚本。代理开发指南见 [AGENTS.md](AGENTS.md)。

本项目提供了一个完整的 Ventoy 配置方案，可以通过 USB 启动盘自动安装各种版本的 Windows 10/11，无需手动干预。

## 特性

- 自动识别并显示 Windows ISO 文件
- 提供所有主要 Windows 版本的无应答安装选项
- 绕过 Windows 11 系统要求检查
- 自定义 Ventoy 启动菜单行为
- 支持 Windows 各版本的产品密钥预配置

## 快速开始

### 准备工作

1. 下载并安装 [Ventoy](https://www.ventoy.net/cn/index.html)
2. 准备一个 USB 闪存驱动器（建议容量至少 8GB）
3. 下载所需的 Windows ISO 文件

### 配置步骤

1. 将本仓库的 `ventoy` 文件夹内容复制到 Ventoy USB 驱动器的根目录
2. 在 USB 驱动器中创建 `ISO/Windows` 文件夹
3. 将 Windows ISO 文件放入 `ISO/Windows` 文件夹

### 使用方法

1. 使用 Ventoy 制作启动 USB 盘
2. 复制配置文件和 Windows ISO 到 USB 盘
3. 从 USB 盘启动计算机
4. 在 Ventoy 菜单中选择要安装的 Windows 版本
5. 选择自动安装选项
6. 系统将自动完成安装过程

## 配置说明

### ventoy.json 配置

主要配置项：

- `auto_install`: 自动安装配置
  - `parent`: ISO 文件搜索路径
  - `template`: 无应答安装模板文件列表
  - `timeout`: 自动选择等待时间（秒）

- `control`: Ventoy 行为控制
  - `VTOY_DEFAULT_IMAGE`: 默认启动镜像
  - `VTOY_MENU_TIMEOUT`: 菜单超时时间
  - `VTOY_WIN11_BYPASS_CHECK`: 绕过 Windows 11 系统检查

### 无应答安装文件

每个 XML 文件对应一个 Windows 版本，包含：
- 产品密钥预配置
- EULA 自动接受
- 安装参数优化

## 文件结构

```text
Ventoy/
├── LICENSE                         # Apache 2.0 许可证
├── AGENTS.md                       # 代理开发指南
└── ventoy/
    ├── ventoy.json                 # Ventoy 主配置文件
    └── script/
        └── Windows_10_11/           # Windows 10/11 无应答安装脚本
            ├── education.xml        # 教育版
            ├── education_n.xml      # 教育版 N
            ├── enterprise.xml       # 企业版
            ├── enterprise_g.xml     # 企业版 G
            ├── enterprise_g_n.xml   # 企业版 G N
            ├── enterprise_n.xml     # 企业版 N
            ├── home.xml             # 家庭版
            ├── home_n.xml           # 家庭版 N
            ├── pro.xml              # 专业版
            ├── pro_4_workstations.xml    # 专业版工作站
            ├── pro_4_workstations_n.xml  # 专业版工作站 N
            ├── pro_education.xml         # 专业教育版
            ├── pro_education_n.xml       # 专业教育版 N
            └── pro_n.xml                # 专业版 N
```

## 支持的 Windows 版本

### Windows 10/11 版本
- 家庭版 (Home / Home N)
- 专业版 (Pro / Pro N / Pro for Workstations / Pro N for Workstations)
- 企业版 (Enterprise / Enterprise N / Enterprise G / Enterprise G N)
- 教育版 (Education / Education N / Pro Education / Pro Education N)

## 注意事项

1. **产品密钥**: 所有配置文件中使用的都是通用产品密钥，用于安装激活
2. **系统要求**: 已配置绕过 Windows 11 的 TPM 2.0 和安全启动检查
3. **数据备份**: 安装前请备份重要数据，安装过程会格式化系统盘
4. **硬件兼容**: 建议在支持的硬件上测试安装

## 故障排除

### 常见问题

1. **启动菜单未显示自动安装选项**
   - 检查 ventoy.json 是否正确复制到 USB 根目录
   - 确认 ISO 文件路径正确

2. **安装过程中出现错误**
   - 验证 Windows ISO 文件完整性
   - 检查硬件兼容性

3. **产品密钥无效**
   - 更新至最新 Windows 版本
   - 使用有效的许可证密钥

### 验证配置

```bash
# 验证 JSON 语法
python -m json.tool ventoy.json

# 验证 XML 语法
xmllint --noout ventoy/script/Windows_10_11/*.xml
```

## 资源链接

- [Ventoy 官方文档](https://www.ventoy.net/cn/doc_start.html)
- [Windows 应答文件技术参考](https://docs.microsoft.com/zh-cn/windows-hardware/customize/desktop/unattend/)
- [Windows ADK 下载](https://docs.microsoft.com/zh-cn/windows-hardware/get-started/adk-install)

## 贡献指南

欢迎提交问题报告和改进建议：

1. Fork 本仓库
2. 创建特性分支
3. 提交更改
4. 发起 Pull Request

## 免责声明

本配置仅用于合法的 Windows 系统安装。使用者需确保：
- 拥有有效的 Windows 许可证
- 遵守微软软件许可条款
- 仅在授权设备上使用

作者不对因使用本配置造成的任何损失承担责任。

## License

[Apache License 2.0 with Commons Clause v1.0](LICENSE)