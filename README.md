# DSH REA MCP Toggle

把 [REA](https://github.com/morluto/rea) MCP 注册为 DeepSeek Harness 的组合插件，在 **插件 → 已安装插件** 中随时开关。关闭后，DSH Desktop 的新会话不再加载 REA 工具定义，可减少不需要逆向分析时的上下文开销。

## 安装

1. 先安装 REA 和 Node.js，再执行 `git clone https://github.com/seventeenlyc/dsh-rea-mcp-toggle.git`。编辑 [`cordis.patch.yml`](./cordis.patch.yml)，把 `command` 和 `args` 中的路径改为本机实际位置。仓库里的路径是作者本机示例。
2. 在 DeepSeek Harness 的 **插件 → 添加插件 → 安装第三方插件** 中输入本地仓库目录路径，安装后打开 `dsh-rea-mcp-toggle` 开关。
3. 新建会话检查 REA 工具是否出现。关闭开关后再新建会话检查工具是否消失。

这个包只声明一个 `dsh.bundle.patch`。DSH 开关修改当前配置档的 `package.json` 中 `dsh.profile.bundles` 选择列表，按选择结果组合 YAML；它不会每次直接改写 `~/.dsh/cordis.patch.yml`。

如果已有全局 `rea-mcp` 条目，先备份 `~/.dsh/cordis.patch.yml`，再把该条目从全局 patch 移走。全局 patch 优先级较高；留下它会导致插件关闭后 REA 仍然加载。其他配置档若仍需 REA，可把原条目分别放入它们自己的 `cordis.patch.yml`。

这个开关只作用于安装它的 DSH 配置档。它不会修改 Claude CLI 或 Codex 的 MCP 配置。已打开的会话可能保留旧工具目录，切换后请新建会话验证。

## License

MIT
