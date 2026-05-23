# Ruflo Config

Ruflo (Claude Code) 自定义配置和工具集。

## 文件

- `statusline.cjs` — Claude Code 底部状态栏脚本
  - 显示模型、上下文使用率、会话费用、DeepSeek 余额
  - 集成 Ruflo swarm/agent/DDD 状态

## Statusline 常见问题

### Statusline 不显示 / 空白

**原因**: 脚本缺少执行权限。Claude Code 直接执行文件，`chmod +x` 缺失时返回退出码 126。

**排查**:
```bash
claude --debug
cat ~/.claude/debug/*.txt | grep -i statusline
# 看到 "completed with status 126" = 缺少执行权限
```

**修复**:
```bash
chmod +x /Users/simaqingfeng/.claude/helpers/statusline.cjs
chmod +x .claude/helpers/statusline.cjs   # 如果项目目录也有
```

重启 Claude Code 后生效。

### 手动 node 跑可以，但 Claude Code 不显示

```bash
# 这两种方式需要 execute bit
~/.claude/helpers/statusline.cjs
sh -c '~/.claude/helpers/statusline.cjs'

# 这种方式不需要 execute bit
node ~/.claude/helpers/statusline.cjs
```

Claude Code 走的是第一种路径，所以文件必须有执行权限。

### CC Switch 保存后 statusline 消失

CC Switch 操作可能重写了配置文件，导致脚本丢失执行位。重新 `chmod +x` 即可。

### CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC

该变量只禁用网络层面的非必要流量（自动更新、遥测、错误报告、反馈），不影响本地执行的 statusline。
