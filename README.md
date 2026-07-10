# GMA Faculty Resource Exchange

A separate, static companion site to Global Migration 101 where faculty submit teaching
resources (via a Google Form) and the editorial team publishes accepted ones (via a Google Sheet).
The course site is untouched — this deploys as its own repo and URL.

**How it flows:** Faculty sign in and submit through your Google Form → responses land in a
Google Sheet → you review each row, set Status to `Approved`, and write an annotation →
the Browse page on this site reads the published "Approved" tab and updates itself within
a few minutes. No code, no commits, no rebuilds.

---

## Part 1 — Create the Google Form (one time, ~15 minutes)

Go to forms.google.com (sign in with the account that should own submissions) → Blank form.
Title: **Share a teaching resource — Global Migration 101**.

Add these questions **in this exact order** (the site reads columns by their names, so keep
the key words shown in bold):

1. **Your name** — Short answer, Required
2. **Institution / organization** — Short answer, Required
3. **Country / where you're based** — Short answer, Required
4. **Which module does this resource fit?** — Dropdown, Required, options:
   Foundations of Global Migration · Migration Data and Global Trends ·
   Religion, Ethics, and Migration · Drivers of Migration · Climate Change and Migration ·
   Refugees, Asylum, and Forced Displacement · Migration Through History ·
   Migration Journey and Experience · Borders, Sovereignty, and Policy ·
   Migration, Labor, and the Economy · Migrant Lives and Identities ·
   Media, Narratives, and Representation · Cities, Communities, and Integration ·
   Solutions, Governance, and the Future · General / multiple modules
5. **Resource type** — Dropdown, Required: Reading · Video · Case study ·
   Activity / exercise · Assignment · Dataset · Slides · Other
6. **Title of the resource** — Short answer, Required
7. **Brief description** — Paragraph, Required
8. **Link to the resource** — Short answer (validation: URL), optional
9. **Or upload a file** — File upload, optional (this forces Google sign-in for all
   respondents — exactly what we want; uploads land in a Drive folder it creates)
10. **How do you currently use this in your teaching?** — Paragraph, Required
11. **Copyright & sharing agreement** — Checkboxes, Required, single option:
    "I confirm this resource contains nothing copyrighted by others, and I agree it may be
    shared freely under a Creative Commons BY-NC 4.0 license with attribution to me."

Settings tab → Responses: **collect email addresses** (verified). Presentation: confirmation
message "Thank you! Our editorial team reviews every submission — accepted resources appear
on the Resource Exchange with your name."

Then: **Responses → Link to Sheets** to create the response spreadsheet.

## Part 2 — Set up the review Sheet (one time, ~5 minutes)

In the response spreadsheet ("Form Responses 1" tab):

1. Add two columns at the far right: **Status** and **Annotation**.
   Give Status a dropdown (Data → Data validation): `Pending`, `Approved`, `Rejected`.
2. Create a new tab named **Approved** and put this formula in cell A1 (adjust the two
   column letters if your form order differs — A:N should span Timestamp through Annotation):

   ```
   ={'Form Responses 1'!C1:N1; FILTER('Form Responses 1'!C2:N, 'Form Responses 1'!M2:M="Approved")}
   ```

   Start the range at column **C** (skip Timestamp + Email) so contributor emails are
   **never** published. Check that the first row shows the question headers.
3. **File → Share → Publish to web** → select only the **Approved** tab → **CSV** → Publish.
   Copy the link.

## Part 3 — Connect this site (one time, ~2 minutes)

Open `assets/config.js` and paste:
- `FORM_URL` — the Form's share link (Send → 🔗)
- `CSV_URL` — the published-CSV link from Part 2

## Part 4 — Deploy (one time, ~5 minutes)

1. GitHub Desktop → File → New repository → name `gma-resource-exchange` →
   set the local path to this folder's parent (or copy this folder into the new repo) →
   Publish repository (public).
2. On github.com → repo Settings → Pages → Source: `main`, folder `/ (root)` → Save.
3. Site appears at `https://cbonfini.github.io/gma-resource-exchange/` in ~2 minutes.

## Everyday admin workflow (the whole job)

1. Open the response Sheet (bookmark it).
2. Read the new row. Open the link/file. Check it really is shareable (no publisher PDFs).
3. Set **Status** to `Approved` (or `Rejected`), fix the Module/Type if the contributor
   guessed wrong, and write one or two sentences in **Annotation** (this appears publicly
   as the "Editor's note").
4. Done. The Browse page refreshes itself — published CSVs update within a few minutes.

Optional: Form settings → "Get email notifications for new responses" so you never miss one.

## Notes

- The course site (`global-migration-101`) is completely independent of this repo.
- If you later want accepted resources to ALSO appear on the course module pages, say the
  word — the same published CSV can feed those pages without changing this workflow.
- Accessibility: both pages are WCAG 2.1 AA (contrast, keyboard, labels, live regions).
