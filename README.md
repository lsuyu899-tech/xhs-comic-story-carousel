# xhs-comic-story-carousel

Codex skill for turning a topic, diary, comment, or long real-life story into a Xiaohongshu/Rednote hand-drawn comic carousel.

The skill is designed for this workflow:

1. Receive a raw topic or pasted story.
2. Ask lightweight questions to lock the main line, protagonist, page count, and ending feeling.
3. Build a page-by-page storyboard with continuity checks.
4. Generate complete vertical comic pages directly with Image 2/image generation, including the Chinese text inside the image.
5. Inspect and regenerate flawed pages, especially wrong Chinese characters, extra hands, or weak transitions.
6. Save final pages in order as `图1.png`, `图2.png`, etc.

## Install

Use Codex's skill installer:

```powershell
python C:\Users\User\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py --repo lsuyu899-tech/xhs-comic-story-carousel --path skills/xhs-comic-story-carousel
```

After installing, restart Codex to pick up the new skill.

Manual install is also possible:

1. Download or clone this repository.
2. Copy `skills/xhs-comic-story-carousel` into your Codex skills directory:

```text
~/.codex/skills/xhs-comic-story-carousel
```

3. Restart Codex.

## Use

In Codex, ask for the skill by name:

```text
Use $xhs-comic-story-carousel

主题：留学快结束，突然觉得三年时间和钱都浪费了
主角：中性小人
要求：像小红书手绘情绪漫画，直接用 Image 2 生成完整版图片
```

You can provide either a short theme or a full pasted story. The skill will first shape the story into a coherent carousel, then generate images when the storyboard is ready or when you explicitly ask to skip planning.

## What It Produces

- Xiaohongshu-style vertical 3:4 comic carousel pages
- Minimalist black hand-drawn line art on a white background
- Short emotional Chinese page text rendered directly inside each image
- A consistent neutral small-person protagonist unless another setting is requested
- A final local folder named like `<主题>-YYYY-MM-DD`

## Boundaries

This skill does not log in to Xiaohongshu, scrape content, publish posts, read cookies, or operate any social media account. It only helps with story planning, image prompts, image generation, revision, and local file organization.

