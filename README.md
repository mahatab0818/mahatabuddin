# Mahatab Uddin — portfolio site

A single-file static site (`index.html`) — no build step, no framework, no third-party
scripts or fonts. The only outbound request the page ever makes is the contact
form submission.

## 1. Wire up the contact form (required)

Static HTML cannot send email by itself — there's no server to do the sending.
This site is wired to **Formspree**, a form-to-email relay, so you don't need
to run a backend:

1. Go to https://formspree.io and create a free account with **mhtb.emon@gmail.com**.
2. Create a new form. Formspree gives you an endpoint like:
   `https://formspree.io/f/abcdwxyz`
3. Open `index.html`, find this line (in the Contact section):
   ```html
   <form id="contactForm" action="https://formspree.io/f/YOUR_FORMSPREE_ID" method="POST" novalidate>
   ```
4. Replace `YOUR_FORMSPREE_ID` with the ID Formspree gave you.
5. Confirm the verification email Formspree sends to mhtb.emon@gmail.com.

That's it — submissions will arrive by email, no data is stored on the site
itself, and Formspree's free tier covers modest traffic.

**Alternative:** if you'd rather not use a third-party relay, swap the fetch
call in the `<script>` block for a call to your own backend/serverless
function (e.g. a small endpoint that uses SMTP or an email API), or simply
remove the JS handler so the form falls back to a `mailto:` link.

## 2. Deploy

Any static host works — drag-and-drop the folder onto:
- **Netlify** or **Cloudflare Pages** — both automatically read the included
  `_headers` file and apply the security headers below.
- **Vercel** — add an equivalent `headers` block in `vercel.json`.
- **GitHub Pages** — headers must instead be set via a `<meta>` tag (already
  partially done in `index.html`) since GitHub Pages doesn't support custom
  response headers.

## 3. Security notes

- `index.html` ships a `<meta>` Content-Security-Policy as a static-file
  fallback. It's stricter once you deploy behind a real host, because the
  `_headers` file sets the same policy (plus `frame-ancestors`, which `<meta>`
  cannot express) as real HTTP response headers.
- No cookies, no analytics, no third-party fonts or scripts — only the
  Formspree POST when a visitor submits the contact form.
- The contact form has a hidden honeypot field (`_gotcha`) to filter basic
  spam bots without a CAPTCHA.
- If you later add any other third-party script (analytics, a chat widget,
  etc.), update the CSP `script-src`/`connect-src` in **both** `index.html`
  and `_headers` to explicitly allow it — don't loosen the policy to `*`.

## 4. Image assets

`logo-face.jpg` (navbar) and `hero-photo.jpg` (hero section) must stay in the
same folder as `index.html` — they're referenced with relative paths, so
keep all files together when you deploy.

## 5. Theme toggle

The sun/moon button in the nav switches between dark and light palettes.
It defaults to the visitor's OS preference and remembers their choice in
`localStorage` after that — no cookies, no server involved.

## 6. Editing content

Everything is in `index.html` — experience, skills, projects, and
certifications are plain HTML blocks, no CMS or data file involved. Search
for the section id (`#experience`, `#skills`, `#projects`, `#credentials`)
to find what you need.
