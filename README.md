# Animated Leetcode

**Animated Leetcode is a skill for AI coding agents.** Install it in Codex—or another compatible agent that supports `SKILL.md`—and the AI will turn LeetCode solutions into portable, interactive lessons where the source code and algorithm state run side by side.

This is not a browser extension, npm package, or standalone lesson generator. The skill gives an AI agent a repeatable workflow, design rules, and a working HTML template so it can create and maintain animated problem pages for you.

Each lesson is a single HTML file. It needs no framework, package installation, build command, or server—open it directly in a browser.

## Who this is for

Use this skill if you want an AI coding agent to:

- create a visual explanation for a LeetCode solution;
- convert your existing code into an interactive walkthrough;
- add consistently designed lessons to a personal algorithm library; or
- maintain synchronized code execution and animation without rebuilding the interface each time.

## What a lesson includes

- Verified LeetCode number, title, and difficulty
- Problem description, example, and topic tags
- Editable test inputs that generate a real execution trace
- Synchronized source-code highlighting and algorithm visualization
- Code-only, animation-only, and side-by-side views
- Play, pause, step, scrub, and playback-speed controls
- Responsive and keyboard-accessible controls

Problem pages follow this filename convention:

```text
<leetcodeNumber>_<leetcode-title-name>.html
```

For example:

```text
1_two-sum.html
3_longest-substring-without-repeating-characters.html
121_best-time-to-buy-and-sell-stock.html
```

## Try the included lesson

Open [index.html](index.html) in a browser, then select **Two Sum**. You can also open [1_two-sum.html](1_two-sum.html) directly.

Change `nums` or `target` and select **Run input**. The lesson rebuilds the trace from your values instead of replaying a fixed recording.

## Install the skill

The complete skill is in [`skills/animated-leetcode`](skills/animated-leetcode). Install the entire directory—not only `SKILL.md`—because it also contains the starter lesson and Codex metadata.

### Windows PowerShell

```powershell
$skillTarget = Join-Path $env:USERPROFILE ".codex\skills\animated-leetcode"
New-Item -ItemType Directory -Force $skillTarget | Out-Null
Copy-Item ".\skills\animated-leetcode\*" $skillTarget -Recurse -Force
```

### macOS or Linux

```bash
mkdir -p ~/.codex/skills/animated-leetcode
cp -R ./skills/animated-leetcode/. ~/.codex/skills/animated-leetcode/
```

Start a new Codex session after installation so the skill is discovered.

## Use the skill

Mention `$animated-leetcode` in your request and provide the problem plus the solution you want explained.

```text
$animated-leetcode Create an interactive lesson for LeetCode 121,
Best Time to Buy and Sell Stock. Use Python and explain the one-pass solution.
```

You can also provide code that must be used:

```text
$animated-leetcode Turn this LeetCode solution into an animated lesson.
Keep my JavaScript implementation unchanged:

function maxProfit(prices) {
  let lowest = Infinity;
  let profit = 0;
  for (const price of prices) {
    lowest = Math.min(lowest, price);
    profit = Math.max(profit, price - lowest);
  }
  return profit;
}
```

Useful follow-up requests include:

```text
Add another test case for descending prices.
Make the pointer movement clearer on mobile.
Change the solution language to Java.
Add this lesson to index.html.
```

## How the skill works

The workflow is defined in [`SKILL.md`](skills/animated-leetcode/SKILL.md). It directs the agent to:

1. Verify the official problem number and algorithm details.
2. Preserve the library's existing visual language.
3. Design a visualization around the algorithm's actual invariant.
4. Accept and validate user-editable test input.
5. Generate a complete, seekable execution trace from that input.
6. Keep the trace synchronized with the displayed source-code lines.
7. Update the collection homepage and verify the completed lesson.

The bundled [`problem-template.html`](skills/animated-leetcode/assets/problem-template.html) provides the interaction shell and the Two Sum example. It is a starting point, not a universal animation: trees, graphs, dynamic-programming tables, pointers, and other structures should receive representations suited to their behavior.

## Visualization conventions

Stored algorithm memory such as `seen`, `memo`, `visited`, or `cache` uses a compact, scrollable log. The component title remains generic—**Memory log**—while rows show concrete runtime assignments:

```text
seen[2] = 0
seen[7] = 1
```

Structures whose shape or order carries meaning, such as trees, graphs, linked lists, heaps, stacks, and queues, retain a structural visualization.

## Repository layout

```text
.
├── index.html
├── 1_two-sum.html
└── skills/
    └── animated-leetcode/
        ├── SKILL.md
        ├── agents/
        │   └── openai.yaml
        └── assets/
            └── problem-template.html
```

## Adding lessons manually

If you are not using the skill:

1. Copy `skills/animated-leetcode/assets/problem-template.html`.
2. Rename it using the verified problem number and lowercase kebab-case title.
3. Replace the problem metadata, source code, input fields, and trace-building logic.
4. Design the animation around the new algorithm rather than retaining irrelevant Two Sum elements.
5. Add a card linking to the new file in `index.html`.
6. Open the page in a browser and test valid, invalid, boundary, and no-solution inputs where applicable.

## Portability

Generated lessons keep their CSS and JavaScript inside the HTML file. You can archive them, email them, publish them with any static host, or open them locally without additional tooling.
