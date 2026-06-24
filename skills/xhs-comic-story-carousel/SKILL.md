---
name: xhs-comic-story-carousel
description: Create Xiaohongshu/Rednote hand-drawn story carousel comics from a theme, pasted real-life content, comments, diaries, or reference-note examples. Use when the user wants to replicate minimalist black-line emotional comic notes, design the plot through a fixed Q&A workflow, lock protagonist consistency with a reference image, freeze confirmed storyboards, decide page count, and generate complete pages directly with Image 2/image generation.
---

# Xiaohongshu Comic Story Carousel

Use this skill to turn a raw topic or long personal story into a Xiaohongshu-style hand-drawn comic carousel. Stability is the first priority: lock the story, lock the character, then generate.

## Non-Negotiables

- Follow the fixed workflow unless the user explicitly overrides it.
- Stability beats speed and brevity. Do not skip character locking to save time.
- Before final story pages, create or choose a character reference image, called `图0角色参考图`, unless the user already provides an accepted reference.
- Once the user confirms a storyboard, freeze it. During image generation, do not rewrite page text, change scenes, change page count, compress metaphors, or "improve" the story.
- Treat image generation as execution of the confirmed storyboard, not a second creative-writing pass.
- Keep cover, story, page text, protagonist setting, reference image, and prompts in one shared context.
- Use direct Image 2 generation for finished pages when the user asks for "直接生图" or "完整版"; do not add text afterward unless the user asks.
- Inspect generated pages and regenerate flawed pages, especially wrong Chinese text, extra hands, duplicated limbs, inconsistent protagonist, or weak continuity.
- Do not log in, scrape, publish, read cookies, or operate a Xiaohongshu account.

## Fixed Workflow

Always drive the work through these gates.

### Gate 1: Story Mainline

Receive the user's material. If the input is only a theme, ask up to three short questions:

- ending feeling: collapse, relief, twist, regret, hope, or dark humor
- protagonist: gender-neutral small person, specific person, mascot, or abstract figure
- ending style: stop at emotion, add reflection, or add reversal

If the user gives long material, extract the mainline directly and ask only if one of the three items above is impossible to infer.

Output this format:

```text
主线:
起点 -> 压力/失败累积 -> 触发物/触发动作 -> 情绪爆点 -> 结尾余味
```

Keep one mainline. Side details are supporting beats, not separate stories.

### Gate 2: Character Lock

Before any storyboard or final image generation, write a `角色锁定卡`. This is mandatory.

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

### Gate 3: Reference Image Lock

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

### Gate 4: Page Count And Storyboard

Choose page count by complexity:

- 6 pages: one simple incident with one emotional turn
- 8 pages: complete emotional arc with clear trigger and ending
- 9-10 pages: long real-life content, multiple failure beats, or needed continuity bridge

Add a page when two pages do not emotionally connect. Common bridge pages:

- expectation -> disappointing reality
- object breaks -> tears start falling
- public failure -> private self-blame
- daily detail -> larger life realization
- "I thought I changed" -> "but the old fear is still there"

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
- Decide any wording simplification before asking for confirmation, not during image generation.
- Do not compress multiple time jumps into one page.
- Do not let the image show the next page's event too early.
- The `角色一致性提示` must repeat the fixed recognition points, for example: `同一个中性小人: 外翘中短发、圆脸黑点眼、松垮短袖`.
- After the storyboard, ask for confirmation unless the user already said "不用确认，直接按这个生图" or equivalent.

### Gate 5: Storyboard Freeze

After the user confirms the storyboard, freeze this exact contract:

- page count
- page order
- each page's `图上文字`
- each page's `画面`
- the protagonist lock
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

### Gate 6: Image 2 Generation

Generate one page per image call. Do not batch unrelated pages into one prompt.

Each prompt must include:

- page number and total pages
- exact confirmed Chinese text to render verbatim
- exact confirmed scene from the storyboard
- the locked protagonist description
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

During planning, show the mainline, character lock, reference plan, page count, and storyboard. During generation, keep updates short and mention which pages were regenerated and why. Final responses should point to the folder and summarize only approved structural changes.
