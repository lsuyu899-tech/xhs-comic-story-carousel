---
name: xhs-comic-story-carousel
description: Create Xiaohongshu/Rednote hand-drawn story carousel comics from a theme, pasted real-life content, comments, diaries, or reference-note examples. Use when the user wants to replicate minimalist black-line emotional comic notes, design the plot through a fixed Q&A workflow, lock protagonist consistency with a reference image, freeze confirmed storyboards, decide page count, and generate complete pages directly with Image 2/image generation.
---

# Xiaohongshu Comic Story Carousel

Use this skill to turn a raw topic or long personal story into a Xiaohongshu-style hand-drawn comic carousel. Stability is the first priority: lock the story, lock the character, then generate.

## Non-Negotiables

- Follow the fixed workflow unless the user explicitly overrides it.
- Stability beats speed and brevity. Do not skip character locking to save time.
- Ask exactly one decision question per assistant turn. Do not ask the user to decide protagonist, ending, page count, style, and generation mode in the same turn.
- Do not make all major choices for the user at once. Guide one step, wait for the user, then move to the next step.
- Treat "可以" as approval of the current gate only, not blanket approval for all later gates.
- Before final story pages, create or choose a character reference image, called `图0角色参考图`, unless the user already provides an accepted reference.
- Once the user confirms a storyboard, freeze it. During image generation, do not rewrite page text, change scenes, change page count, compress metaphors, or "improve" the story.
- Treat image generation as execution of the confirmed storyboard, not a second creative-writing pass.
- Keep cover, story, page text, protagonist setting, style lock, reference image, and prompts in one shared context.
- Use direct Image 2 generation for finished pages when the user asks for "直接生图" or "完整版"; do not add text afterward unless the user asks.
- Inspect generated pages and regenerate flawed pages, especially wrong Chinese text, extra hands, duplicated limbs, inconsistent protagonist, or weak continuity.
- Do not log in, scrape, publish, read cookies, or operate a Xiaohongshu account.

## One-Decision Rule

This skill must feel like guided collaboration, not an automatic one-shot generator.

Use this rule in every planning turn:

```text
First: summarize what has been decided.
Then: show only the next useful decision.
Finally: ask exactly one question.
```

Good question pattern:

```text
我建议这一组停在“被迫撑住，不鸡汤”的结尾。你确认这个结尾方向吗？
```

Bad question pattern:

```text
你想要什么结尾、主角、页数、风格，要不要直接生图？
```

Decision order:

1. mainline and ending direction
2. protagonist
3. style lock
4. reference image plan
5. page count
6. storyboard
7. generation

Do not write the full storyboard before page count is approved. Do not generate images before the reference plan and storyboard are approved. If the user explicitly says "你直接定，不用问", make conservative choices, then show the locked summary before generating.

## Fixed Workflow

Always drive the work through these gates.

### Gate 1: Story Mainline

Receive the user's material. Extract one proposed mainline and one proposed ending direction.

If the input is too thin to infer the mainline, ask exactly one question about the missing piece. Do not ask multiple setup questions at once.

Output this format:

```text
主线:
起点 -> 压力/失败累积 -> 触发物/触发动作 -> 情绪爆点 -> 结尾余味
建议结尾方向:
<collapse / relief / twist / regret / hope / dark humor, with one short reason>
确认问题:
我建议这组停在「<ending direction>」。你确认这个结尾方向吗？
```

Keep one mainline. Side details are supporting beats, not separate stories.

### Gate 2: Character Lock

After the mainline is approved, write a `角色锁定卡`. This is mandatory before any page count, storyboard, or final image generation.

Default character lock:

```text
角色锁定卡:
主角: 中性小人
脸: 圆脸，黑点眼，小鼻子，小嘴
头发: 凌乱中短发，发尾外翘，长度到脖子附近
身体: 瘦小，头稍大，松垮短袖，简单长裤
气质: 有点缩着、疲惫、敏感，不夸张卖萌
固定识别点: 发尾外翘 + 松垮短袖 + 小圆脸 + 黑点眼
禁止变化: 不换发型，不换年龄感，不变成写实人物，不变成另一个性别特征明显的人
```

If the user gives a different protagonist, still produce the same card structure.

End this gate with exactly one question:

```text
我建议用这个中性小人作为固定主角。你确认这个主角设定吗？
```

### Gate 3: Style Lock

After the protagonist is approved, lock the visual style before page count or storyboard.

Default style lock:

```text
风格锁定卡:
画幅: 竖版 3:4，小红书连续图
底色: 纯白大留白
线条: 黑色粗糙手绘马克笔线条，线稿感，不要写实
人物: 同一个中性小人，头发保持白底线稿，不要整块涂黑
文字: 粗黑手写中文，短句，大字，优先放上方
颜色: 默认黑白，只允许少量浅蓝点缀眼泪、雨、热气、杯子或高光
构图: 每页一个核心画面，不做复杂多格漫画，不堆背景
禁止: 写实阴影、彩色插画、精致漫画上色、复杂场景、换画风、logo、水印、二维码
```

End this gate with exactly one question:

```text
我建议固定成这种黑白大留白手绘风格。你确认这个风格吗？
```

### Gate 4: Reference Image Lock

Default to stability mode. Do one of these before final story pages:

- If the user provides a reference protagonist image, ask whether it is the locked character reference.
- If no reference exists, generate `图0角色参考图` first. This image is not part of the Xiaohongshu carousel and is not saved as `图1`.
- If the image tool supports visual references, use the accepted reference image for every final page.
- If visual references are not available, paste the full `角色锁定卡` and the fixed recognition points into every page prompt.

`图0角色参考图` prompt:

```text
Use case: character-reference
Asset type: reference sheet for a Xiaohongshu hand-drawn diary comic protagonist, not a final carousel page
Primary request: Generate a clean character reference image for one consistent protagonist.
Character: gender-neutral small person; round face; black dot eyes; tiny nose; small mouth; messy medium-short hair with outward-flipped ends near the neck; small thin body; loose short-sleeve T-shirt; simple long pants; tired, slightly withdrawn posture.
Views: show one front view and two small expression/pose variations of the same character on a white background.
Style: minimalist black hand-drawn marker line art, rough casual diary comic feeling, no color except optional tiny pale-blue accent.
Constraints: no Chinese text, no labels, no logo, no watermark, no QR code, no extra characters, no detailed background.
```

After `图0角色参考图`, inspect it. If it changes the character concept, regenerate before continuing. Character consistency is more important than saving one generation.

End this gate with exactly one question before generating or accepting the reference:

```text
我建议先做一张图0角色参考图来稳住后面所有页面。你确认先做参考图吗？
```

After generating `图0角色参考图`, ask exactly one acceptance question:

```text
这张图0的角色能作为后续所有页面的固定参考吗？
```

### Gate 5: Page Count

After the mainline, protagonist, style, and reference plan are approved, propose one page count.

Choose by complexity:

- 6 pages: one simple incident with one emotional turn
- 8 pages: complete emotional arc with clear trigger and ending
- 9-10 pages: long real-life content, multiple failure beats, or needed continuity bridge

Add a page when two pages do not emotionally connect. Common bridge pages:

- expectation -> disappointing reality
- object breaks -> tears start falling
- public failure -> private self-blame
- daily detail -> larger life realization
- "I thought I changed" -> "but the old fear is still there"

Output:

```text
建议页数:
<N> 页
原因:
<one short reason tied to story complexity>
确认问题:
我建议做 <N> 页。你确认这个页数吗？
```

Do not write the full storyboard until page count is approved.

### Gate 6: Storyboard

After page count is approved, create the full storyboard.

Storyboard every page using exactly this format:

```text
图1
图上文字:
画面:
情绪作用:
衔接检查:
角色一致性提示:
```

Storyboard rules:

- Page text should usually be 1-3 short lines.
- Decide any wording simplification before asking for storyboard confirmation, not during image generation.
- Do not compress multiple time jumps into one page.
- Do not let the image show the next page's event too early.
- The `角色一致性提示` must repeat the fixed recognition points, for example: `同一个中性小人: 外翘中短发、圆脸黑点眼、松垮短袖`.
- The `风格一致性提示` should be included in the `画面` or `角色一致性提示` when useful: black line art, white background, large blank space, no filled-black hair unless approved.
- After the storyboard, ask exactly one confirmation question unless the user already said "不用确认，直接按这个生图" or equivalent.

### Gate 7: Storyboard Freeze

After the user confirms the storyboard, freeze this exact contract:

- page count
- page order
- each page's `图上文字`
- each page's `画面`
- the protagonist lock
- the style lock
- the accepted reference image

During final image generation:

- Do not shorten text silently.
- Do not rewrite metaphors silently.
- Do not change the scene to make prompting easier.
- Do not merge or remove pages.
- Do not add new story beats.
- Do not say "I made a small adjustment" after generation unless the user approved that adjustment before generation.

If a confirmed line is too long or repeatedly fails Chinese rendering, stop and say:

```text
这一页如果继续按原文生图，文字稳定性可能不够。我建议改成：
原文:
建议:
原因:
你确认后我再重生这一页。
```

No unapproved rewrite is allowed.

Before generation, show a short frozen summary and ask exactly one final execution question:

```text
冻结内容: 页数、每页文字、每页画面、主角、风格、参考图都已锁定。
确认问题:
现在按冻结分镜逐页生图吗？
```

### Gate 8: Image 2 Generation

Generate one page per image call. Do not batch unrelated pages into one prompt.

Each prompt must include:

- page number and total pages
- exact confirmed Chinese text to render verbatim
- exact confirmed scene from the storyboard
- the locked protagonist description
- the locked style description
- the accepted reference image if the image tool supports it
- exact visible pose and limb count
- style constraints
- no extra text, logo, watermark, QR code, or dense background

Use this prompt pattern:

```text
Use case: illustration-story
Asset type: completed Xiaohongshu carousel comic page X of N, vertical 3:4 portrait
Primary request: Generate a finished hand-drawn Chinese diary comic page with readable handwritten Chinese text included in the image. Execute the confirmed storyboard exactly; do not reinterpret or rewrite it.

Reference:
Use the accepted 图0角色参考图 as the visual reference for the protagonist if visual references are supported. The character must match the reference image.

Exact confirmed Chinese text to render, verbatim:
<confirmed page text>

Confirmed scene to draw:
<confirmed storyboard scene>

Locked protagonist:
gender-neutral small person; round face; black dot eyes; tiny nose; small mouth; messy medium-short hair with outward-flipped ends near the neck; small thin body; loose short-sleeve T-shirt; simple long pants; tired, slightly withdrawn posture. Fixed recognition points: outward-flipped medium-short hair, round face with black dot eyes, loose T-shirt, small body. Do not change hairstyle, age impression, realism level, or gender expression.

Locked style:
vertical 3:4 Xiaohongshu carousel page; pure white background; lots of blank space; minimalist black rough marker line art; handwritten black Chinese text; no realistic rendering; no detailed shading; no full-color illustration; no complex multi-panel comic layout; hair remains line-art on white, not a filled black mass, unless the confirmed reference image uses filled hair.

Pose/anatomy:
<describe exact pose>. Exactly one protagonist unless specified. Exactly two arms, two hands, two legs. No extra hands, no duplicated limbs.

Style:
minimalist black hand-drawn line art, rough marker strokes, casual Xiaohongshu diary comic feeling, white background, large blank space, emotionally restrained, optional tiny pale-blue accent only.

Composition:
vertical 3:4, text top center unless the storyboard says bottom text, drawing in lower-middle area, lots of white space.

Constraints:
Chinese text must be exactly the confirmed text. No extra words. No English unless included in the confirmed text. No logo, watermark, QR code, speech bubbles, dense background, or realistic shading.
```

For pages with multiple characters, keep the protagonist distinct and describe background characters as vague silhouettes with no detailed faces.

## Quality Control

After generation, inspect every page before calling the set done.

Check:

- protagonist consistency: same hair, face, clothes, age impression, and emotional style as `图0角色参考图`
- style consistency: same black-line white-background hand-drawn style, same text rhythm, same sparse composition
- storyboard fidelity: text and scene match the confirmed storyboard exactly
- text accuracy: no wrong Chinese characters, missing lines, or extra text
- anatomy: no extra hands, merged arms, duplicated limbs, or three-handed poses
- continuity: page X naturally leads to page X+1
- composition: enough white space, vertical 3:4, no crowded panel layout

Regenerate only flawed pages unless the user approves a storyboard change. If a character drifts by page 2 or 3, regenerate from that page with stronger reference-image wording and the full locked protagonist block.

## Save The Final Set

When the user asks to save outputs locally:

- Create a folder under the current workspace.
- Name it `<主题>-YYYY-MM-DD`.
- Copy final carousel images into it.
- Name final carousel files in reading order: `图1.png`, `图2.png`, ..., `图N.png`.
- Save `图0角色参考图` as `图0-角色参考.png` only if the user wants to keep it; do not count it in the carousel.
- If new pages are inserted before storyboard confirmation, renumber following pages.
- After storyboard confirmation, do not insert pages unless the user approves a revised storyboard.
- Leave original generated images in Codex's generated-images directory.

Report the final folder path and page count.

## Output Style

During planning, show only the current gate and one next decision question. Do not dump the whole workflow, all choices, and full storyboard in one response unless the user explicitly asks for a one-shot draft. During generation, keep updates short and mention which pages were regenerated and why. Final responses should point to the folder and summarize only approved structural changes.
