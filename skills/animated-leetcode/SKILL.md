---
name: animated-leetcode
description: Create or update polished, self-contained HTML lessons that explain LeetCode problems through synchronized source code and algorithm-specific animation. Use for building an animated problem page, adding a problem to an existing visual LeetCode library, or standardizing such pages into a reusable single-file format.
---

# Animated Leetcode

Build each problem as a portable HTML lesson that makes the algorithm's changing state easy to understand. Preserve the host project's established design when one exists; otherwise adapt the starter in [assets/problem-template.html](assets/problem-template.html).

## Lesson contract

Each problem page must contain:

- the problem title and difficulty;
- a faithful problem description, example input/output, and constraints when useful;
- topic tags;
- the solution source code;
- an algorithm-specific visualization synchronized with the active source line; and
- Code, Animation, and Side-by-side display modes; and
- editable test inputs that regenerate and run the execution trace.

Keep one problem in one self-contained `.html` file unless the user requests a different architecture. Include CSS and JavaScript in the file, avoid a build step, and do not introduce remote dependencies merely for convenience.

Name every problem file `<leetcodeNumber>_<leetcode-title-name>.html`: use the verified official problem number, then an underscore, then the title in lowercase kebab case. For example, Two Sum is `1_two-sum.html` and Longest Substring Without Repeating Characters is `3_longest-substring-without-repeating-characters.html`. Never invent or guess the numeric prefix; verify it before creating or renaming the file.

## Compact invocation input

Accept this compact request payload after the host agent's skill invocation:

```text
{<leetcode number>/<leetcode title>/<leetcode link>}
```

In Codex, the complete form is `$animated-leetcode {<leetcode number>/<leetcode title>/<leetcode link>}`. When this same skill folder is installed in Claude Code at `~/.claude/skills/animate-leetcode/`, the complete form is `/animate-leetcode {<leetcode number>/<leetcode title>/<leetcode link>}`.

Parse only the first two `/` separators. Treat the first field as the problem number, the second as the title, and the complete remainder as the URL so `https://` and URL path separators remain intact. Trim whitespace around the first two fields. Verify that the number, title, and official LeetCode link refer to the same problem before generating the lesson. Apply any text after the closing brace as additional user requirements.

## Workflow

1. Inspect nearby problem pages and the collection index before editing. Match their navigation, tokens, spacing, and interaction model.
2. Confirm the official LeetCode problem number, algorithm, language, example trace, and complexity from supplied material. If essential problem facts are missing, consult an authoritative source. Show a problem number only when verified, and label it `LeetCode N`; otherwise omit the number. Use user-supplied problem text as given; otherwise write a concise faithful description and link to the original rather than copying a long webpage verbatim.
3. Identify the smallest set of state that explains the algorithm. Design the visual around that state instead of reusing an unrelated generic animation.
4. Accept user-editable test input appropriate to the problem. Validate it inline with actionable messages and sensible visualization bounds, then generate the execution trace from the submitted values rather than replaying a hard-coded example.
5. Implement execution as an ordered trace. Every trace step should specify the active code line, explanatory text, and complete visible algorithm state so seeking backward is deterministic.
6. Synchronize Play/Pause, previous, next, scrubber, speed, and restart behavior. Playback must stop cleanly at the final step and replay from the beginning. Submitting new input must stop old playback, rebuild the trace, reset the timeline, and run the new trace.
7. Add or update the card in the collection index when the repository has one.
8. Verify valid, invalid, boundary, duplicate, negative, and no-solution inputs where applicable, along with all three display modes, narrow-screen behavior, keyboard controls, and the final result. Check the browser console for errors when browser tooling is available.

## Visualization choices

Choose representations that expose the algorithm's invariant:

- arrays and two pointers: indexed cells with independently labeled pointers;
- hash maps and sets: a compact, scrollable memory log with one plain entry per row, plus clear lookup hits and misses; avoid decorative chips or cards unless the user requests them;
- linked lists and trees: nodes and directional edges, preserving identity during movement;
- graphs: nodes, edges, frontier, visited state, and traversal order;
- dynamic programming: a table whose changed cell and dependency cells are distinct;
- stacks and queues: ordered containers with push/pop or enqueue/dequeue motion;
- sorting: values as bars or cells, distinguishing comparison, swap, and fixed regions.

Use motion to show state transitions, not as decoration. Keep the current item, comparison candidates, stored/visited values, and confirmed result visually distinct. Ensure meaning is still available through labels or text, not color alone.

### Stored-memory convention

When the algorithm accumulates memory in a named variable or parameter, such as `seen`, `memo`, `visited`, `cache`, a frequency map, or a lookup table, default to a compact logging format:

- keep the panel title generic, such as `Memory log`; do not put a parameter name, array expression, or schema in the title;
- show the concrete variable expression inside each row, using actual runtime values, such as `seen[2] = 0`, rather than a placeholder such as `seen[value] = index`;
- show one plain entry or operation per row in insertion/execution order;
- make the panel fixed-height and vertically scrollable when entries can grow;
- automatically keep the newest or currently matched row visible;
- distinguish lookup hits, misses, inserts, and updates without turning entries into decorative cards or chips.

Use a structural view instead when topology or order is the concept being taught, such as a tree, graph, heap, linked list, stack, or queue. A memory log may accompany that view, but should not replace the structure that explains the invariant.

## Implementation invariants

- Render each step from complete state rather than relying on accumulated DOM mutations. This makes reverse navigation and scrubbing reliable.
- Keep displayed code line numbers aligned with trace line references after code edits.
- Derive the scrubber maximum and step counter from the trace length.
- Stop an existing playback timer before manual navigation or starting a replacement timer.
- Derive the trace from the same algorithm displayed in the code panel; do not merely swap labels into a fixed animation.
- Keep custom input parsing bounded and safe. Display validation failures without discarding the last valid trace.
- Make controls actual buttons with accessible labels and visible focus states.
- Respect `prefers-reduced-motion` and keep the layout usable around 360 px wide.
- Escape dynamic text inserted as HTML, or use `textContent`, when input is not a hard-coded trusted fixture.
- Avoid em dashes in all generated user-facing copy. Use commas, colons, parentheses, or separate sentences instead.

## Quality bar

The lesson should explain why each transition happens, not merely announce the current line. At the final step, show the returned result and connect it to the original input. Complexity claims must match the displayed implementation.

Do not treat the starter's Two Sum hash-map visualization as a universal layout. Retain its shell and controls when useful, but redesign the stage for the data structure and invariant of the requested algorithm.
