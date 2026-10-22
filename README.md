# DSH REA MCP Toggle

An installable DeepSeek Harness bundle that adds the [REA](https://github.com/morluto/rea) MCP server. Switch the bundle off on the Plugins page when reverse engineering tools are not needed. By default it starts `rea-agents@6.5.0` through `npx`; the first start downloads the package. Requires Node.js compatible with REA and `npx` on `PATH`.

To install, clone this repository, then use **Plugins → Add plugin → Install third-party plugin** and enter the clone's directory. If another REA entry is present in the home patch, back it up and remove that entry first; otherwise REA stays active when this bundle is off. Local REA installations can override the `rea-mcp` command in the active profile patch. See the Chinese instructions below for the exact behavior.

把 [REA](https://github.com/morluto/rea) MCP 注册为 DeepSeek Harness 的组合插件，在 **插件 → 已安装插件** 中随时开关。关闭后，DSH Desktop 的新会话不再加载 REA 工具定义，可减少不需要逆向分析时的上下文开销。

## 安装

1. 安装 Node.js 22.19+ 或 24.11+，并确认终端可运行 `npx`。执行 `git clone https://github.com/seventeenlyc/dsh-rea-mcp-toggle.git`。
2. 在 DeepSeek Harness 的 **插件 → 添加插件 → 安装第三方插件** 中输入本地仓库目录路径，安装后打开 `dsh-rea-mcp-toggle` 开关。默认配置通过 `npx -y rea-agents@6.5.0 mcp` 启动 REA；首次启动需要下载 npm 包。
3. 新建会话检查 REA 工具是否出现。关闭开关后再新建会话检查工具是否消失。

这个包只声明一个 `dsh.bundle.patch`。DSH 开关修改当前配置档的 `package.json` 中 `dsh.profile.bundles` 选择列表，按选择结果组合 YAML；它不会每次直接改写 `~/.dsh/cordis.patch.yml`。

如果已有全局 `rea-mcp` 条目，先备份 `~/.dsh/cordis.patch.yml`，再把该条目从全局 patch 移走。全局 patch 优先级较高；留下它会导致插件关闭后 REA 仍然加载。其他配置档若仍需 REA，可把原条目分别放入它们自己的 `cordis.patch.yml`。

已有本地 REA 安装、希望避免 `npx` 启动开销时，可以在 DSH 当前配置档的 `cordis.patch.yml` 中为 `rea-mcp` 添加更高优先级的配置覆盖，把 `command` 指向本机 Node.js，把 `args` 指向本机的 `rea.mjs` 与 `mcp`。这个覆盖仅在组合插件开启时生效；关闭组合插件仍会移除 REA 组件。修改前请备份配置档。

```yaml
- id: rea-mcp
  name: "@deepseek-ai/dsh-mcp-client"
  config:
    serverName: rea
    transport: stdio
    command: node
    args:
      - /path/to/rea/scripts/rea.mjs
      - mcp
```

这个开关只作用于安装它的 DSH 配置档。它不会修改 Claude CLI 或 Codex 的 MCP 配置。已打开的会话可能保留旧工具目录，切换后请新建会话验证。

## License

MIT
