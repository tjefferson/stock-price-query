# Changelog

## v1.1.4 (2026-03-29)

### Added
- **市盈率与市值数据**：查询结果新增 `pe_ratio`（市盈率）和 `market_cap`（总市值）字段，在数据源可用时自动返回
- **版本号查询**：支持 `--version` / `-V` 参数查看脚本版本

### Changed
- SKILL.md 输出格式说明新增市盈率和市值展示
- 恢复 CHANGELOG.md 和 README.md 文档

## v1.1.3 (2026-03-23)

### Changed
- Removed documentation files: CHANGELOG.md and README.md

## v1.1.2 (2026-03-23)

### Changed
- Minor metadata updates

## v1.1.1 (2026-03-23)

### Fixed
- **美股指数代码支持**：修复 `.IXIC`（纳斯达克）、`.DJI`（道琼斯）、`.INX`（标普500）等以 `.` 开头的指数代码被输入校验正则拦截的问题，正则从 `^[A-Za-z0-9]{1,10}$` 改为 `^\.?[A-Za-z0-9]{1,10}$`
- **港股指数市场识别**：`HSI`（恒生指数）、`HSCEI`（国企指数）等纯字母港股指数代码之前被 `detect_market` 误判为美股，新增 `HK_INDEX_CODES` 白名单优先匹配港股指数
- **港股指数代码格式**：`build_tencent_symbol` 和 `parse_stock` 中港股指数不再做 `zfill(5)` 补零，避免生成无效代码如 `0HSI`

## v1.1.0 (2026-03-23)

### Added
- **批量查询**：支持一次查询多只股票（逗号分隔），单次 HTTP 请求获取全部数据，最多 20 只
- **大盘指数映射**：新增上证指数、深证成指、创业板指、恒生指数、纳斯达克、道琼斯、标普500 等指数代码映射
- **扩大股票映射表**：从 10 只扩展至 21 只热门个股，新增招商银行、工商银行、五粮液、美的、美团、小米、京东、谷歌、亚马逊、Meta 等

### Changed
- SKILL.md description 优化，新增批量查询、大盘指数等高频搜索关键词
- SKILL.md tags 从 17 个扩展至 30 个，覆盖更多搜索场景
- 输出格式新增批量查询的紧凑对比展示格式，涨跌使用 🟢/🔴 颜色标识

## v1.0.4 (2026-02-26)

### Changed
- SKILL.md 输出格式从 Markdown 表格改为紧凑多行格式，避免飞书消息卡片分页
- 价格与涨跌幅合并为一行展示
- 成交量与成交额合并为一行展示
- 成交额增加人性化单位（亿/万）显示要求
- README.md 新增最终显示格式示例

## v1.0.3 (2026-02-25)

### Security
- 新增输入安全校验，stock_code 仅允许字母和数字（`^[A-Za-z0-9]{1,10}$`），market 使用白名单（sh/sz/hk/us），防止 shell 命令注入
- SKILL.md 增加输入安全校验说明
- 解决 ClawHub VirusTotal 扫描报告中的 Suspicious 标记

## v1.0.2 (2026-02-24)

### Reverted
- 移除 stock_query.sh wrapper 脚本，保持纯 .py 脚本方案
- SKILL.md 调用方式还原为直接 python3 调用

## v1.0.1 (2026-02-24)

### Changed
- 数据源从新浪财经 API (hq.sinajs.cn) 切换为腾讯财经 API (qt.gtimg.cn)，解决 403 Forbidden 问题
- 腾讯 API 无需 Referer Header，稳定性更好
- 统一了各市场（A 股/港股/美股）的数据解析逻辑，代码更简洁
- 更新 references/api-docs.md 为腾讯 API 文档

## v1.0.0 (2026-02-24)

### Added
- 初版发布，支持查询股票实时价格
- 支持 A 股沪市（sh）、A 股深市（sz）、港股（hk）、美股（us）四个市场
- 自动识别股票代码所属市场
- 零外部依赖，纯 Python 标准库实现
- 返回结构化 JSON 数据（当前价、涨跌幅、开高低收、成交量等）
- API 限流自动重试机制
- 提供 API 参考文档（references/api-docs.md）
- SKILL.md 元数据格式对齐 OpenClaw 官方示例规范
