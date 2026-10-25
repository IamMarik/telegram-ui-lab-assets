---
name: telegram-rich-message
description: Compose or format Telegram Rich Messages in Rich Markdown or Rich HTML for preview in Telegram UI Lab. Use for a message draft, announcement or existing text intended for either Rich format.
---

# Telegram Rich Message

Turn the user's text or message brief into ready-to-paste Telegram Rich Markdown or Rich HTML. Follow their requested language, tone and output format. Use the requested Rich format; default to Rich Markdown when none is specified.

1. Read the official syntax for the selected format:
   - [Rich Markdown](https://core.telegram.org/bots/api#rich-markdown-style): preview with `/richmd`. Use `**bold**` and `*italic*`; do not apply MarkdownV2 escaping.
   - [Rich HTML](https://core.telegram.org/bots/api#rich-html-style): preview with `/richhtml`. Use supported tags such as `<h2>`, `<p>`, `<b>`, `<i>` and `<ul><li>`. This is Telegram Rich HTML; use no CSS or unsupported browser HTML.
2. Preserve the meaning, facts, numbers, links and code in existing text. When drafting from a brief, use the supplied facts without inventing details.
3. Make the message readable on a phone. Use short paragraphs and purposeful headings, lists and emphasis.
4. Put optional supporting details in separate `<details>` sections when useful. Keep the main message and essential information visible. Do not nest expandable sections. In Rich Markdown, put Markdown directly inside each section, with blank lines around the body:

   ```markdown
   <details><summary>More details</summary>

   - A supporting point

   </details>
   ```

   In Rich HTML, use HTML throughout the section:

   ```html
   <details>
     <summary>More details</summary>
     <ul>
       <li>A supporting point</li>
     </ul>
   </details>
   ```

5. In Rich HTML, escape literal text and attribute values as required, while keeping formatting tags intact. Return raw markup rather than escaping the entire source or wrapping it in JSON.
6. Keep each message's complete source within 3,500 UTF-16 code units, including tags, escapes, URLs and whitespace. Count the source, not just the visible text; JavaScript's `source.length` gives this measure. Exclude only the outer response fence. This leaves margin below the ordinary 4,096-character text-message limit used to paste source into the bot; the larger Rich Message rendering limit does not increase this input budget. If the content is longer, split it into complete standalone messages at sensible boundaries without silently dropping content. Keep tags, details sections, tables and code blocks valid within each part. The bot previews each part separately and does not assemble fragments.
7. By default, return only the source in a standard copyable fenced code block labeled `markdown` or `html` to match the selected format. Use one block per Telegram message; use multiple blocks only when splitting for length. Start opening and closing fences at column 1 on separate lines, outside lists and blockquotes. Use at least four backticks and a fence longer than any backtick run in the source, so inner code blocks cannot close it early. Keep every source line, including `<blockquote>`, `<details>` and Markdown quote markers, inside the fence. Show literal source throughout; do not render the HTML or turn the response itself into a quotation.

The user copies each code block with its Copy control, selects the matching mode in Telegram UI Lab and sends that source as a separate message. When copying manually, omit the outer backticks and language label. If the selected reference cannot be accessed and the requested syntax is uncertain, ask for the relevant excerpt instead of inventing it.
