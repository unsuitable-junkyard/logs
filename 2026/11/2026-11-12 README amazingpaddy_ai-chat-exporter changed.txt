日時：2026-11-12
タイトル：README amazingpaddy_ai-chat-exporter changed
文字数：2188
---

Title: ChatGPT export finds 0 messages after ChatGPT's DOM change (Nov 2026) – working patch

Hi, and thanks for AI Chat Exporter.

As of Nov 12, 2026, the ChatGPT export always shows "Please select at least one message to export" for me, and no checkboxes appear.

What I found in the current ChatGPT page:
- article[data-testid^="conversation-turn-"], h5.sr-only / h6.sr-only, data-message-author-role and the copy-turn-action-button test id are all gone (0 matches).
- Messages can now be found through data-content-search-unit-key / data-chatgpt-search-unit-key. The value ends with ":user" or ":assistant" (for example "fallback-turn-0:1:assistant").
- ChatGPT appears to keep previously opened chats in the DOM, so after switching chats without a reload, old messages get mixed in. Filtering by data-chatgpt-selection-conversation-id (compared with the id in the URL) fixes this.
- The turn container is now [data-talvt-turn-state]. The copy button has to be the one that comes after the assistant message. Otherwise the user's own copy button gets clicked.

I'm not a JavaScript developer, so I used Claude (AI) to rewrite src/content_scripts/chatgpt.js based on these findings. It works for me on Edge. I also added src/lib/turndown.js to the ChatGPT content script entry in manifest.json, as a fallback converter when the copy button is not usable.

Small side note: the manifest.json inside the v4.0.1 release zip says "version": "4.0.2".

Modified files and notes (credit to your repository, Apache License 2.0):

Original program: amazingpaddy/ai-chat-exporter (Apache License 2.0)
https://github.com/amazingpaddy/ai-chat-exporter

Changes: Rewrote src/content_scripts/chatgpt.js to work with the ChatGPT page as of November 2026
Also changed manifest.json to load src/lib/turndown.js for ChatGPT pages.

Modified by: unsuitable-junkyard (https://github.com/unsuitable-junkyard)

Modified files (saved as .txt – please rename to chatgpt.js / manifest.json):
- chatgpt.js: https://github.com/unsuitable-junkyard/logs/blob/main/2026/11/2026-11-12%20ChatGPT-js.txt
- manifest.json: https://github.com/unsuitable-junkyard/logs/blob/main/2026/11/2026-11-12%20Manifest-js.txt

The full chat log with Claude (in Japanese, unedited), including how I investigated the problem step by step:
https://github.com/unsuitable-junkyard/logs/blob/main/2026/11/2026-11-12%20(Japanese)AI%20Chat%20Exporter%20troubleshooting.txt

Please feel free to use any part of it, or just ignore it. No need to reply. Thanks again!