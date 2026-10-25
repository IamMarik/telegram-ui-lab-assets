---
name: tg-rich
description: Compose or format Telegram Rich Messages in Rich Markdown or Rich HTML for preview in Telegram UI Lab. Use for a message draft, announcement or existing text intended for either Rich format.
---

# Telegram Rich

Turn the user's text or brief into ready-to-paste Rich Message source. Follow the requested language, tone and Rich format; otherwise keep the original language and default to Rich HTML. Preserve meaning, facts, numbers, links and code; do not invent missing facts.

Use short paragraphs and purposeful headings, lists and emphasis. Keep essential information visible; put optional details in independent, non-nested `<details>` sections when useful.

1. Choose the matching syntax and preview mode:
   - [Rich Markdown](https://core.telegram.org/bots/api#rich-markdown-style), `/richmd`: `# Heading`, `**bold**`, `*italic*`, `- item`, `[label](URL)`. Do not apply MarkdownV2 escaping.
   - [Rich HTML](https://core.telegram.org/bots/api#rich-html-style), `/richhtml`: supported tags such as `<h2>`, `<p>`, `<b>`, `<i>` and `<ul><li>`. Escape literal text and attribute values, keeping formatting tags intact. No CSS or unsupported browser HTML.

   Use these basics directly. Consult the matching reference for unfamiliar syntax; if it is unavailable and the syntax is uncertain, ask for the relevant excerpt.

2. In Rich Markdown, Markdown works inside `<details>` with blank lines around its body:

   ```markdown
   <details><summary>More details</summary>

   - A supporting point

   </details>
   ```

   In Rich HTML, use HTML throughout:

   ```html
   <details><summary>More details</summary><ul><li>A supporting point</li></ul></details>
   ```

   Rich Markdown is not parsed inside most other block HTML tags: use `<blockquote><b>Important</b></blockquote>`, not Markdown emphasis inside that tag.

3. For embedded images, use supplied public HTTP(S) URLs in standalone media blocks. Local paths and invented media identifiers do not work in this paste-only flow. For buttons, use supplied URLs or known bot actions; callback buttons require handlers. Do not present invented callback data as a working action. Ask for missing links or action details only when needed to fulfill the request.

4. Keep each complete source within 3,500 UTF-16 code units, including markup, escapes, URLs and whitespace, excluding the outer response fence. With execution tools, verify using JavaScript `source.length`. This is the bot's paste budget, separate from Rich rendering limits. Split longer content into complete standalone messages with valid markup, preserving all content. The bot previews each part separately without assembling fragments.

5. By default, return only one fenced `markdown` or `html` source block per message. Fences start at column 1, outside quotations and lists. Use at least four backticks and more than any backtick run in the source. Keep all markup inside the fence; do not render it or wrap it in JSON.

The user copies each block's contents without the outer fence and sends it to Telegram UI Lab in the matching mode.
