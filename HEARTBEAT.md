# HEARTBEAT.md

# Keep this file empty (or with only comments) to skip heartbeat API calls.

# Add tasks below when you want the agent to check something periodically.

---

## WAL 自检（每次 Heartbeat）

- [ ] 本次对话有需要写入 SESSION-STATE.md 的决策/偏好/修正吗？如有，写入。
- [ ] memory/working-buffer.md 状态？（空还是 ACTIVE）
- [ ] 当前 context 使用比例是否 > 60%？如是，确认 working-buffer 已启动。

## ADL/VFM 改动评分（每次准备改文件/配置前自检）

准备做改动前，先问：

| 维度 | 权重 | 问题 |
|------|------|------|
| High Frequency | 3x | 这个改动每天都会用到吗？ |
| Failure Reduction | 3x | 这个改动能把失败变成成功吗？ |
| User Burden | 2x | 改了之后用户能少说一个字吗？ |
| Self Cost | 2x | 改了之后未来省 token/时间吗？ |

**加权总分 < 50 的改动不做。**

---

## openclaw_came_alive
- 读取 skill: ~/.openclaw/workspace/skills/openclaw_came_alive/SKILL.md
- 用 scripts/manage_state.py 做 state 初始化、precheck、成功/失败记账
- 若 state 允许继续，再定位真实用户会话、读取最近上下文、提取 signal、生成 candidate
- 用 scripts/quality_gate.py 检查 candidate；不过关则不发
- 发送时必须显式投递到真实用户会话，不使用 heartbeat 默认回投目标
