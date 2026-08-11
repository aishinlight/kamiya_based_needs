# The Shape of a Life — Ikigai Self-Reflection

A single-file, self-contained web survey based on Mieko Kamiya's needs for a life
worth living. Eight statements, a 1–7 scale, and an octagon "resonance figure"
that shows the shape of a person's answers. Frame: a **reflective self-portrait**,
not a validated psychometric measure.

The eight items are original wording (grounded in Kamiya) — so, unlike the
published Ikigai-9 or Gallup's Q12, there is no third-party item copyright to
worry about here.

---

## 1. Put it online (GitHub Pages)

1. Create a new repository (e.g. `ikigai-reflection`).
2. Add `index.html` to the root of the repo.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source = Deploy from a branch**,
   **Branch = main**, folder **/(root)**, then **Save**.
5. After a minute your survey is live at
   `https://<your-username>.github.io/<repo-name>/`.

That URL is what you share. It works out of the box as a self-scoring tool:
each respondent answers, sees their own resonance figure, and can download
their own answers as a CSV. **Nothing is sent anywhere until you turn on
collection below.**

---

## 2. (Optional) Collect responses into a Google Sheet

GitHub Pages can only serve static files — it can't store submissions itself.
The simplest free way to collect them is a Google Apps Script attached to a
Google Sheet. Ten-minute setup.

**a. Create the sheet.** New Google Sheet. In row 1, add these headers exactly:

```
timestamp | life_satisfaction | bright_future | change_growth | resonance | freedom | self_actualisation | meaning_value | purpose | note
```

**b. Add the script.** In the sheet: **Extensions → Apps Script**. Delete
anything there and paste:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const d = JSON.parse(e.postData.contents);
  sheet.appendRow([
    d.timestamp, d.life_satisfaction, d.bright_future, d.change_growth,
    d.resonance, d.freedom, d.self_actualisation, d.meaning_value,
    d.purpose, d.note || ""
  ]);
  return ContentService.createTextOutput("ok");
}
```

**c. Deploy it.** Click **Deploy → New deployment** → gear icon → **Web app**.
Set **Execute as: Me**, **Who has access: Anyone**. Click **Deploy**, authorise,
and copy the **Web app URL** (ends in `/exec`).

**d. Wire it up.** Open `index.html`, find this line near the bottom:

```javascript
const COLLECT_ENDPOINT = "";
```

Paste your URL between the quotes and re-upload the file. That's it — a consent
checkbox now appears on the survey, and each submission lands as a new row in
your sheet.

Responses are anonymous by default (no name, email, or IP is collected). If you
later want an optional email or role field, add it to the form and to both the
sheet headers and the script row.

---

## 3. Make it yours

- **The words.** Every title and statement lives in the `NEEDS` array near the
  top of the `<script>`. Edit freely.
- **The colours.** The palette is at the very top of the `<style>` block, in
  `:root`. To move from the indigo/gold reflective look to the Beyond Engagement
  crimson/black/white, change `--ink`, `--gold`, and `--shu`.
- **The framing text.** Intro, results headline, and footer are plain HTML in
  the body — rewrite in your own voice.

---

*Based on Mieko Kamiya (神谷美恵子, Kamiya Mieko), Ikigai ni tsuite
(生きがいについて, "On the Meaning of Life"), 1966.*
