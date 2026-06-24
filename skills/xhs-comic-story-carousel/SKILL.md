---
name: xhs-comic-story-carousel
description: Create Xiaohongshu/Rednote hand-drawn story carousel comics from a theme, pasted real-life content, comments, diaries, or reference-note examples. Use when the user wants to replicate minimalist black-line emotional comic notes, design the plot through a fixed Q&A workflow, lock protagonist consistency, decide page count, storyboard continuity, and then generate complete pages directly with Image 2/image generation.
---

# Xiaohongshu Comic Story Carousel

Use this skill to turn a raw topic or long personal story into a Xiaohongshu-style hand-drawn comic carousel. The skill has two jobs: first shape the material into a coherent page-by-page emotional story, then generate finished images directly with Image 2 when requested.

## Non-Negotiables

- Follow the fixed workflow below unless the user explicitly says to skip planning and directly generate.
- Keep cover, story, page text, protagonist setting, and image prompts in one shared context.
- Lock one protagonist before storyboarding and reuse the exact same protagonist description in every image prompt.
- Treat character consistency as a deliverable, not a style suggestion.
- Build a flip-through story: every page must set up, escalate, reveal, bridge, or resolve.
- Add bridge pages when adjacent pages skip a psychological step.
- Prefer short, spoken Chinese page text over explanatory paragraphs.
- Use direct Image 2 generation for finished pages when the user asks for "直接生图" or "完整版"; do not add text afterward unless the user asks.
- Inspect generated pages and regenerate flawed pages, especially wrong Chinese text, extra hands, duplicated limbs, inconsistent protagonist, or weak continuity.
- Do not log in, scrape, publish, or operate a Xiaohongshu account.

## Fixed Workflow

Always drive the work through these four gates.

### Gate 1: Story Mainline

Receive the user's material. If the input is only a theme, ask up to three short questions:

- ending feeling: collapse, relief, twist, regret, hope, or dark humor
- protagonist: gender-neutral small person, specific gender/person, mascot, or abstract figure
- ending style: stop at emotion, add reflection, or add reversal

If the user gives long material, extract the mainline directly and ask only if one of the three items above is impossible to infer.

Output this format:

```text
主线:
起点 -> 压力/失败累积 -> 触发物/触发动作 -> 情绪爆点 -> 结尾余味
```

Keep one mainline. Side details are supporting beats, not separate stories.

### Gate 2: Character Lock

Before any storyboard or image generation, write a `角色锁定卡`. This is mandatory.

Use this format:

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

When generating images, paste the full `角色锁定卡` meaning into every page prompt. Do not rely on "same character as before"; each Image 2 call is independent and may forget earlier pages.

### Gate 3: Page Count And Storyboard

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
- Do not compress multiple time jumps into one page.
- Do not let the image show the next page's event too early.
- The `角色一致性提示` must repeat the fixed recognition points, for example: `同一个中性小人: 外翘中短发、圆脸黑点眼、松垮短袖`.
- After the storyboard, briefly ask for confirmation unless the user has already said "直接生图", "不用问", or similar.

### Gate 4: Image 2 Generation

Generate one page per image call. Do not batch unrelated pages into one prompt.

Each prompt must include:

- page number and total pages
- exact Chinese text to render verbatim
- one simple scene
- the locked protagonist description
- exact visible pose and limb count
- style constraints
- no extra text, logo, watermark, QR code, or dense background

Use this prompt pattern:

```text
Use case: illustration-story
Asset type: completed Xiaohongshu carousel comic page X of N, vertical 3:4 portrait
Primary request: Generate a finished hand-drawn Chinese diary comic page with readable handwritten Chinese text included in the image.

Exact Chinese text to render, verbatim, large in rough black hand-lettering:
<page text>

Scene:
<one simple scene>

Locked protagonist, must stay visually consistent across the whole set:
gender-neutral small person; round face; black dot eyes; tiny nose; small mouth; messy medium-short hair with outward-flipped ends near the neck; small thin body; loose short-sleeve T-shirt; simple long pants; tired, slightly withdrawn posture. Fixed recognition points: outward-flipped medium-short hair, round face with black dot eyes, loose T-shirt, small body. Do not change hairstyle, age impression, realism level, or gender expression.

Pose/anatomy:
<describe exact pose>. Exactly one protagonist unless specified. Exactly two arms, two hands, two legs. No extra hands, no duplicated limbs.

Style:
minimalist black hand-drawn line art, rough marker strokes, casual Xiaohongshu diary comic feeling, white background, large blank space, emotionally restrained, optional tiny pale-blue accent only.

Composition:
vertical 3:4, text top center unless the storyboard says bottom text, drawing in lower-middle area, lots of white space.

Constraints:
Chinese text must be exactly the provided text. No extra words. No English unless included in the provided text. No logo, watermark, QR code, speech bubbles, dense background, or realistic shading.
```

For pages with multiple characters, still keep the protagonist distinct and describe background characters as vague silhouettes with no detailed faces.

## Quality Control

After generation, inspect every page before calling the set done.

Check:

- protagonist consistency: same hair, face, clothes, age impression, and emotional style
- text accuracy: no wrong Chinese characters, missing lines, or extra text
- anatomy: no extra hands, merged arms, duplicated limbs, or three-handed poses
- continuity: page X naturally leads to page X+1
- composition: enough white space, vertical 3:4, no crowded panel layout

Regenerate only flawed pages unless the story structure changed. If a character drifts by page 2 or 3, regenerate from that page with a stronger copied `Locked protagonist` block and mention the fixed recognition points near the top of the prompt.

If direct Chinese text repeatedly fails, simplify the line while preserving meaning and tell the user the wording was simplified for text stability.

## Save The Final Set

When the user asks to save outputs locally:

- Create a folder under the current workspace.
- Name it `<主题>-YYYY-MM-DD`.
- Copy final images into it.
- Name files in final reading order: `图1.png`, `图2.png`, ..., `图N.png`.
- If new pages are inserted, renumber following pages.
- Leave original generated images in Codex's generated-images directory.

Report the final folder path and page count.

## Output Style

During planning, show the mainline, character lock, page count, and storyboard. During generation, keep updates short and mention which pages were regenerated and why. Final responses should point to the folder and summarize structural changes, not restate every prompt.
