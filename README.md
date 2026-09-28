# 144 Bulletin Brief

**AppADay #144** | Spirituality | AI-powered

**Live app:** https://augustineiacopelli.github.io/appaday-144-bulletin-brief/
**Portfolio:** https://augustineiacopelli.github.io/appaday/

Paste a Catholic parish bulletin or upload its PDF, and Claude turns it into a one page digest: Mass times with this week's schedule changes up top, confession and adoration, events, and what the parish is asking of you. Unclear items carry a visible flag. Add events to your calendar, share, or print.

## How it works

Open Settings with the gear and add your Anthropic API key and an optional session name. Both stay in your browser's localStorage and the key is sent only to api.anthropic.com. Paste the bulletin text, choose the bulletin PDF, or do both, then set the week of date, which defaults to the coming Sunday. Press Generate brief.

The app makes one call to the Messages API with `claude-sonnet-5`, thinking disabled, and a 4,000 token ceiling. A PDF travels as a base64 document block ahead of the pasted text, and PDFs over 20 MB are refused before sending. The system prompt asks for a single JSON object in a fixed schema covering the parish, the week, weekend, weekday, and special Masses, schedule changes, confession, adoration, events, asks, and notes. Anything the bulletin does not state comes back as null or an empty list rather than a guess, and every item carries a flag field that names the reason when something is ambiguous, such as an inferred year or a date with no time.

The reply is sliced to its outermost braces, parsed, and normalized so every field has the expected type. Event dates that fail a strict YYYY-MM-DD check are cleared and flagged, and nothing outside the schema is ever rendered. If the reply cannot be parsed, the app shows a retry message instead of a broken digest.

## The digest

Sections always appear in the same order: the Mass schedule table with a highlighted band for this week's changes above it, then Confession and Adoration, Events sorted by date, Asks sorted by deadline with undated asks last, and Notes. Empty sections disappear. Missing values read Not listed in muted type, and flagged items show an amber marker with the reason printed as text so it survives on paper.

## Calendar, share, and print

Events with a valid date and start time get an Add to calendar button that downloads a .ics file. The file carries a full America/Chicago VTIMEZONE with daylight rules for the second Sunday in March and standard rules for the first Sunday in November, so events on either side of the time change import at the right local time. A missing end time defaults to one hour, and events starting before 6:30 AM or reading as open periods get no button.

Share sends a plain text version of the digest through the device share sheet and falls back to copying it to the clipboard. Print produces a compact two column, black on white letter page with all controls hidden.

## History

Each successful brief is saved by week, up to twelve weeks, with the oldest dropped first. Only the parsed JSON is stored, never the PDF. Open the history drawer with the clock icon to reopen a past week, or delete one with a two tap confirm.

## Tech

A single self-contained `index.html` of vanilla HTML, CSS, and JavaScript with no build step. Fonts are Cormorant Garamond and Inter from Google Fonts. The API is called directly from the browser with the `anthropic-dangerous-direct-browser-access` header and the user's own key.

---

Part of [AppADay](https://augustineiacopelli.github.io/appaday/), one complete app shipped every day.
