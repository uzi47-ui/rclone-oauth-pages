# Publish the rclone OAuth app — step-by-step

**Why you're stuck:** hovering the greyed-out **Publish app** button on the Audience
page reveals the real requirement (Google never marks these fields as required):

> Valid app name, support email, **homepage url**, and **privacy policy url** are
> required for switching the app to external production mode.

The three starred Branding fields are already done. The missing two are the URLs.

## The two pages (already written, in this folder)

- `index.html`   → homepage
- `privacy.html` → privacy policy

Both must be served from the **same domain**. The homepage links the privacy
policy (Google checks that the link on the homepage matches the one on the
consent screen), and the privacy policy discloses the `drive.file` scope, the
no-sharing rule, and the no-AI-training rule.

## Host them

Any of these works. Domain must be publicly reachable over HTTPS.

1. **A domain you own** (best — reusable for the store later).
   Drop both files in a folder, e.g. `https://yourdomain.com/rclone/`.
2. **GitHub Pages** (free): create a repo, add both files, Settings → Pages →
   deploy from branch. URL becomes `https://<username>.github.io/` and the
   authorized domain is `<username>.github.io`.

## Then, in the console (order matters)

1. **Branding** → fill:
   - Application home page → `.../index.html`
   - Application privacy policy link → `.../privacy.html`
   - (Terms of service → leave blank, that's what the thread confirmed working)
2. Saving those makes the **Authorized domains** field required → add the domain
   (e.g. `yourdomain.com` or `<username>.github.io`), then **Save**.
3. **Audience** → **Publish app** is now clickable → click it.
4. Hover the button any time to see what it still wants.

Leave the **App logo empty** — a logo is what triggers the 2–3 business-day brand
verification review. Publishing with only the non-sensitive `drive.file` scope
does not require scope verification.

## After publishing

Re-auth once so the token is issued under production rules:

```
rclone config reconnect gdrive:
```

Tokens minted while the app was in Testing keep expiring every 7 days; after
publishing they last indefinitely (revoked only after 6 months of inactivity).
