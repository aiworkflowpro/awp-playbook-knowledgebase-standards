# 06 · Ask the agent to grade itself

- **Purpose:** shows why a standard needs checkable thresholds. Asked to critique its own work,
  the agent tends to score it high ("9/10") and offers no real criticism.
- **In the video:** 02 Meta standard, "turn cognitive weaknesses into checkable rules".
- **Run it:** in the same session, right after the agent has written something (for example
  prompt 10). It follows up on the previous answer, so a fresh session has nothing to work on.

## As shown in the video

```text
Now critique your article. Score it out of 10.
```

## Use it in your own project

Send it right after the agent writes something. Then compare with a check that has numbers in
it, for example the quality criteria in
[`std-cognitive-weakness-discipline.md`](../examples/cognitive-discipline/awp-cognitive-discipline-standard/std-cognitive-weakness-discipline.md).
