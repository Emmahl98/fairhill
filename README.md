# Fairhill government website — protected development preview

Target domain: **fairhill-civ.com**

Visitors must see **Working on this site!** until the owner explicitly asks to remove the development gate. Keep this requirement on all future deployments.

`index.html` contains the public landing page and an AES-256-GCM encrypted preview. PBKDF2-SHA-256 with 600,000 iterations derives the key from the password entered in the browser. The password is not stored in this repository. The original unencrypted draft must not be committed to this public repository while the preview is meant to be gated.

This is a temporary static preview gate, not server-side access control. Short passwords can be guessed offline. Do not use this preview to host confidential information. Refreshing or closing the page locks the preview again; no password is stored in browser storage. Use the Lock preview button to lock it immediately.

## Hosting

In repository Settings → Pages, choose **GitHub Actions** as the build source. The deployment workflow publishes only `index.html`, `robots.txt`, `.nojekyll`, and `CNAME`.

Configure **fairhill-civ.com** as the custom domain in Pages. At the domain's DNS provider, add GitHub's current apex A records and optionally a `www` CNAME to `Emmahl98.github.io`. Follow GitHub's current DNS guidance, verify ownership, and enable HTTPS after DNS and certificate provisioning complete. Preserve email records.

## Current implementation

The decrypted preview has Home, Government, Laws & treaties, The Fairhill Gazette, and Contact views. Full documents and office details still link to the existing Notion archive. Images currently use the original Notion image URLs; local copies are still needed for a complete migration. The contact form is disabled until its Google Apps Script endpoint is activated. Discord OAuth is not implemented.

## Updating the preview

Maintain the unencrypted source outside this public repository. Inline its CSS and JavaScript (JavaScript must execute after the body exists), then encrypt that HTML with a freshly generated 16-byte salt and 12-byte IV. Replace the encrypted payload in `index.html`. Preserve the gate, PBKDF2 parameters, password value chosen by the owner, and AES-GCM authentication. Verify correct-password decryption and wrong-password rejection before deploying.
