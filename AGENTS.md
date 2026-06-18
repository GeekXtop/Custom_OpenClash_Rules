# 仓库指南

## 项目结构与模块组织

本仓库维护 OpenClash 配置模板、分流规则、维护脚本和文档。

- `cfg/` 存放 Subconverter/OpenClash `.ini` 模板；`cfg/yaml/` 存放 Clash YAML 变体。
- `rule/` 存放维护源文件 `.list`，以及由其生成的 `.yaml` 和 `.mrs` 规则集。
- `game_rule/` 存放游戏相关 YAML 规则。
- `overwrite/` 存放远程覆写示例。
- `py/` 存放 Python 维护脚本，目前主要是 `generate_game_cdn.py`。
- `shell/` 和 `script/` 存放路由器安装、检查和维护脚本。
- `doc/` 是文档源内容；`wiki/` 是 MkDocs/GitHub Pages 来源及 Wiki 备份镜像。
- `.github/workflows/` 存放规则生成、文档部署、Wiki 同步和 CDN 刷新等自动化流程。

## 构建、测试与开发命令

Python 使用 `uv` 管理。

- `uv run python py/generate_game_cdn.py`：从 v2fly 上游更新 `rule/Game_Download_CDN.list`。
- `uv run --with mkdocs-material mkdocs build --clean`：构建文档站点到 `site/`。
- `uv run --with mkdocs-material mkdocs serve`：本地预览 MkDocs 文档站点。
- `git diff --check`：提交前检查空白字符问题。
- `mihomo convert-ruleset domain yaml rule/Custom_Direct_Domain.yaml rule/Custom_Direct_Domain.mrs`：本地安装 `mihomo` 后，可验证 `.mrs` 转换。

## 代码风格与命名约定

文件保持 UTF-8 编码，优先使用 LF 换行。YAML `payload:` 列表使用 2 空格缩进，并保持现有生成文件格式。规则源文件使用 Clash 风格条目，例如 `DOMAIN-SUFFIX,example.com`、`DOMAIN,example.com`、`DOMAIN-KEYWORD,keyword`、`IP-CIDR,1.2.3.0/24` 或 `SRC-PORT,1234`。文件命名沿用 PascalCase 加下划线风格，例如 `Custom_Direct.list`、`Custom_Proxy_Domain.yaml`。

## 测试与验证指南

仓库没有独立测试套件。修改后按影响范围验证：文档变更需构建 MkDocs；规则生成逻辑变更需运行对应 Python 脚本；规则文件变更需检查 `git diff`，确认生成文件没有意外变化。修改 `.list` 后，应同步确认对应 `.yaml` 和 `.mrs` 与源文件一致，或明确交由 GitHub Actions 自动生成。

## 提交与 PR 规范

近期提交使用 Conventional Commit 风格，例如 `feat(rules): remove anyrouter.top from Custom_Direct.list`、`chore(rules): auto generate Custom_Direct_Domain.mrs from Custom_Direct_Domain.yaml`。推荐 scope 使用 `rules`、`wiki`、`cfg`、`shell` 或 `docs`。

PR 应说明影响的文件、变更原因和验证方式；如有关联 Issue，请一并链接。只有文档截图、页面渲染或图片资源变更时才需要附截图。