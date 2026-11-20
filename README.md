# grounding-visual-prompts

通用 AI 图像 / 视频生成提示词「先核验画面物理与逻辑，再动笔写词」纪律技能（Agent Skill）。

专治 AI 在复刻参考视频、GIF、截图或换角二创时最常见的 6 类翻车：
- 凭低帧率缩略图或脑补加戏（把无声张嘴当怒吼、漏掉关键微表情）
- 动作方向与视线方向写反（主体左右与画面左右混淆，违背牛顿物理因果）
- 道具中途莫名其妙消失或闪现（未做道具守恒追踪）
- 参考图 / 参考视频与文字要求互相打架（带循环跳切的脏源片、一边标 `fully_preserved` 一边改首帧）
- 单镜头内硬塞微时间戳造成机械卡顿、或用 `without trembling` 等否定词反向诱发抖动（粉红大象效应）
- 失败返工时只针对用户吐槽的 2 个词“打补丁”，遗漏未上报的根本缺陷

## 核心机制

1. **4 道前置硬门禁（Pre-Prompt Gates）**：写词前必须先可见输出：
   - **Gate 1**：素材洁净度检查 & `≥8fps`（`≤3s` 全帧）逐帧日志 + 首尾 3 帧跳切检查 + 屏幕位置轨迹追踪
   - **Gate 2**：节拍、视线与物理向量必填表（统一 `viewer-left / viewer-right` 坐标系 + 牛顿受力与时长比检查）
   - **Gate 3**：道具守恒账本必填表（逐件追踪至末 3 帧）
   - **Gate 4**：参考素材与提示词冲突矩阵（状态冲突 / 陈旧上传 ID / 角色参数冲突；不通过直接 **STOP**，禁止在脚注藏雷的同时输出“可提交 JSON”）
2. **提示词转换规范**：连续因果叙事（零单镜微时间戳）+ 正向静止表达（零否定词反噬）+ 零无依据加戏。
3. **失败返工铁律 & 生成后逐拍验收**：双端抽帧全量重跑 Gates 1–4；成片后 `≥2fps` 抽帧对照 Gate 2/3 逐项核验。

> 模型无关：适用于 MiniMax-H3、Seedance 2.0/2.5、Veo、Kling、Wan、Google Flow / Nano Banana 等任意图/视频模型；具体模型语法与接口参数请配合对应模型技能或官方文档使用。

## 安装方式

```bash
git clone https://github.com/alexkwok213/grounding-visual-prompts.git
ln -s "$(pwd)/grounding-visual-prompts" ~/.agents/skills/grounding-visual-prompts
```

（适用于 Claude Code、Codex、Antigravity、Cursor 等支持 `~/.agents/skills/` 的 Agent 工具。）
