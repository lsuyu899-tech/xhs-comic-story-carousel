---
name: xhs-comic-story-carousel
description: Create Xiaohongshu/Rednote hand-drawn story carousel comics from a theme, pasted real-life content, comments, diaries, or reference-note examples. Use when the user wants to replicate minimalist black-line emotional comic notes, design the plot through step-by-step Q&A, decide protagonist setting, page count, storyboard, continuity, and then generate complete pages directly with Image 2/image generation.
---

# Xiaohongshu Comic Story Carousel

Use this skill to turn a raw topic or long personal story into a Xiaohongshu-style hand-drawn comic carousel. The core job is not only drawing; first shape the material into a coherent page-by-page emotional story, then generate finished images directly with Image 2 when requested.

## Operating Principles

- Keep cover, story, page text, character setting, and image prompts in one shared context.
- Do not jump straight to image generation unless the user explicitly asks to skip planning.
- Build a flip-through story: each page should set up, escalate, reveal, bridge, or resolve.
- Prefer short, spoken Chinese page text over explanatory paragraphs.
- Preserve the user's emotional truth, but avoid needlessly insulting or dehumanizing wording in on-image text.
- Use direct Image 2 generation for finished pages when the user asks for "直接生图" or "完整版"; do not default to post-adding text unless the user asks for that.
- Warn briefly that direct Chinese text generation may produce wrong characters or anatomy errors, then inspect and regenerate flawed pages.
- Do not log in, scrape, publish, or operate a Xiaohongshu account.

## Workflow

### 1. Receive Material

Accept any of these:

- a full pasted story
- a theme
- a comment or diary-like paragraph
- a competitor/reference example
- an existing storyboard needing refinement

If the input is thin, ask only the minimum needed questions, usually:

- What is the ending feeling: collapse, relief, twist, regret, or hope?
- Who is the protagonist: gendered, neutral, mascot, or abstract small person?
- Should the result stop at emotion, or add a final reflection?

### 2. Extract The Main Line

Summarize the story as one sentence:

```text
起点 -> 压力/误解/失败累积 -> 触发物/触发动作 -> 情绪爆点 -> 结尾余味
```

Example:

```text
留学快结束 -> 发现自己什么都没留下 -> 旧书包坏掉 -> 控制不住哭 -> 在公交站承认这几年也坏掉了
```

Keep only one main line. Move side details into supporting pages only if they improve the flip-through rhythm.

### 3. Decide Protagonist And Visual Contract

Lock a reusable protagonist before storyboarding:

- gender-neutral small person unless the user specifies otherwise
- messy medium-short hair, round face, dot eyes, small mouth
- loose T-shirt and simple pants
- emotion shown through posture: shrinking, frozen hand, low head, trembling lines, tears, sitting alone
- black hand-drawn marker lines on white background
- large blank areas and simple compositions
- optional tiny pale-blue accent on rain, tears, phone glow, zipper, cup, or pen

Keep the same protagonist across pages. Background characters should be vague silhouettes unless they are story-critical.

### 4. Choose Page Count

Choose page count based on story complexity, not a fixed template.

- 6 pages: one simple incident with one emotional turn
- 8 pages: complete emotional arc with a clear trigger and ending
- 9-10 pages: long real-life content, multiple failure beats, or when a bridge page is needed for continuity

Add a page when two adjacent pages feel like they skip a psychological step. Common bridge pages:

- expectation -> disappointing reality
- object breaks -> tears start falling
- public failure -> private self-blame
- daily detail -> larger life realization

### 5. Storyboard Before Generating

For every page, define five fields:

```text
页码:
图上文字:
画面:
情绪作用:
衔接检查:
```

Use page text of 1-3 short lines. Avoid stuffing backstory into image text.

Continuity check examples:

- If a page says "I thought I would become outgoing", the image should show expectation, not the later failure. Add the later failure as the next page.
- If a page shows an object breaking, the next page should show the uncontrolled emotion before jumping to the final scene.
- If the final page uses a metaphor, an earlier page should introduce the object or situation behind it.

### 6. Generate With Image 2

When generating finished pages, issue one image-generation call per page. Include:

- use case: `illustration-story`
- asset type: completed Xiaohongshu carousel comic page X of N, vertical 3:4
- exact Chinese text to render, verbatim
- scene description
- protagonist/anatomy constraints
- style constraints
- no extra text, logo, watermark, or QR code

Prompt pattern:

```text
Use case: illustration-story
Asset type: completed Xiaohongshu carousel comic page X of N, vertical 3:4 portrait
Primary request: Generate a finished hand-drawn Chinese diary comic page with readable handwritten Chinese text included in the image.
Exact Chinese text to render, verbatim, large at the top in rough black hand-lettering:
<page text>
Scene: <one simple scene>
Character anatomy constraint: one human character only, exactly two arms, exactly two hands, exactly two legs. No extra hands, no duplicated limbs.
Style: minimalist black hand-drawn line art, rough marker strokes, casual Xiaohongshu diary comic feeling, white background, emotionally restrained, one tiny pale blue accent only.
Composition: vertical 3:4, text top center unless the page needs bottom text, drawing in lower-middle area, lots of white space.
Constraints: Chinese text must be exactly the provided text, no extra words, no English unless included in the provided text, no logo, no watermark, no QR code, no speech bubbles, no dense background, no realistic shading.
```

For Chinese text stability:

- Prefer common characters and short lines.
- If a character is repeatedly wrong, rephrase while preserving meaning.
- Avoid rare words when a simpler word works.
- Inspect pages for wrong characters, extra hands, duplicated limbs, broken continuity, and unreadable text.

### 7. Revise Page-Level Problems

Regenerate only the flawed page unless the story structure changed.

Common fixes:

- Wrong text: shorten or rephrase the line, then regenerate.
- Extra hands: specify both hands' exact pose, for example "both hands cover the face, exactly two hands visible".
- Weak transition: add a bridge page and renumber the final folder.
- Scene/text mismatch: separate the expectation page from the reality page.

### 8. Save The Final Set

When the user asks to save outputs locally:

- Create a folder under the current workspace.
- Name it `<主题>-YYYY-MM-DD`.
- Copy final images into it.
- Name files in order: `图1.png`, `图2.png`, ..., `图N.png`.
- If new pages are inserted, renumber following pages so the folder reflects final reading order.
- Leave original generated images in Codex's generated-images directory.

Report the final folder path and page count.

## Output Style

During planning, be concise and collaborative. Show the main line, key settings, and storyboard. During generation, keep updates short and mention which pages were regenerated and why. Final responses should point to the folder and summarize structural changes, not restate every prompt.

