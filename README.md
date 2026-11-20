# grounding-visual-prompts

复刻 GIF 或参考视频时，帮助 AI 先读懂表演，再写生成提示词。比如黑白猫挥拳、橘猫捂肚、杆子倒地，同样三个动作，可能是拳头打偏，也可能是杆子回弹。两种解释需要拍出不同的接触瞬间。

## 怎么用

把原片交给助手，试着问：

> 用 grounding-visual-prompts 看这段 GIF。先说清谁想做什么、实际发生了什么、观众在哪一拍明白。如果画面支持两种不同的拍法，请指出哪一帧有歧义。

- **读片**：先看原件，再对照旧提示词和失败样片；分别记录可见动作、解释依据、创作者已确认的意图和本次新增的改编。
- **准备生成**：为角色图、首帧和动作参考分配职责，检查机位、起手姿势、道具和视线；具体参数按目标模型与接入服务核实。
- **验片与返工**：先静音原速看包袱是否成立，再逐帧检查关键接触。按最早出现偏差的位置决定重读剧情、换图、改词或试剪。

完整流程和案例见 [SKILL.md](SKILL.md)。

## 安装方式

```bash
git clone https://github.com/alexkwok213/grounding-visual-prompts.git
ln -s "$(pwd)/grounding-visual-prompts" ~/.agents/skills/grounding-visual-prompts
```

（适用于 Claude Code、Codex、Antigravity、Cursor 等支持 `~/.agents/skills/` 的 Agent 工具。）
