# 01 · Trailer from channel positioning

- **Purpose:** the same one-line request, run twice. With the trailer Skill installed, the agent
  runs nine steps; without it, the agent gives a generic answer.
- **In the video:** Intro, the opening demo.

## As shown in the video

In the terminal:

```text
Based on AWP's channel positioning, generate a complete promotional video.
```

On the "Today" card:

```text
Based on AWP’s channel positioning, generate a complete promotional video.
```

## Use it in your own project

Install the example Skill first: copy
[`examples/channel-trailer-skill/awp-content-trailer-generating/`](../examples/channel-trailer-skill/awp-content-trailer-generating/SKILL.md)
into your agent's skills folder (Claude Code: `.claude/skills/`; Codex: `.agents/skills/`; any other agent: keep it in `skills/` and name `skills/awp-content-trailer-generating/SKILL.md` in your prompt), and set
`brand_dir` in its `config/default.yaml` to the folder that holds your brand notes. Then replace
`AWP` with your own channel:

```text
Based on {your channel}'s channel positioning, generate a complete promotional video.
```

If your agent does not pick up Skills from a folder on its own, start the prompt with
`Use skills/awp-content-trailer-generating/SKILL.md.`

Run it once without the Skill as well. The difference between the two answers is the point of
the video.

With the example Skill you get the full plan for the video (truth sheet, script, timeline and five
shot prompts); rendering the clips needs a video generator of your own, see
[the Skill's README](../examples/channel-trailer-skill/README.md).
