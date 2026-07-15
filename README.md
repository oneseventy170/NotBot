# NotBot

**Proof a human wrote it.** Type a message and it comes with a replay of every keystroke — the false starts, the backspaces, the pauses. Send it as a link, and whoever opens it watches the message get written back, keystroke by keystroke. Proof a person made it, not a prompt.

- **Zero backend** — nothing runs on a server; it's one static HTML file.
- **Zero stored** — the message lives *inside* the share link, nowhere else.
- **Open source** — MIT licensed (see below).

## Run it

No build, no backend, no dependencies (besides webfonts). Just open the file:

```
open index.html
```

Or drop `index.html` + `notbotFav.svg` on any static host — GitHub Pages, Netlify, Vercel, Cloudflare Pages.

> For local development, serve over HTTP so share links resolve correctly:
> ```
> python3 .claude/serve.py     # http://localhost:8912, caching disabled
> ```

## How it works

While you type, each edit is recorded as a tiny diff — `{time, position, chars deleted, chars inserted, was-it-a-paste}`. On save, that operation log is delta-encoded, `deflate-raw` compressed, and packed into the URL fragment (`#m=…`). The recipient's browser decodes it and replays the writing. Typed text renders green; pasted blocks are flagged amber.

Because the recording rides in the URL fragment — and fragments are never sent in HTTP requests — the message never touches the host. The server only ever sees a request for the static file; everything else happens in the browser.

**The link is the message.** There's no lookup, no database. Anyone who has the link can read it, so treat the link like the message itself. The sender can also set the replay speed (1×/2×/3×), which travels baked into the link.

## Honest limitation

This is **client-side** proof. It's persuasive — you can watch the writing happen — but it is *not* tamper-proof: a determined faker could hand-edit the payload, or type out AI text from a second screen. A hardened version would sign and timestamp the event stream server-side at capture time. This v1 trades that away to stay fully static and backend-free.

## License

MIT — do anything you like with this code (use, modify, distribute, sell), just keep the copyright notice. No warranty.

```
MIT License

Copyright (c) 2026 Stefan Rhoades

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
