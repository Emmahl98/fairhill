# Fairhill government website — protected development preview

Target domain: **fairhill-civ.com**

Visitors must see **Working on this site!** until the owner explicitly asks to remove the development gate. Keep this requirement on all future deployments.

`index.html` contains the public landing page and an AES-256-GCM encrypted preview. PBKDF2-SHA-256 with 600,000 iterations derives the key from the password entered in the browser. The password is not stored in this repository. The original unencrypted draft must not be committed to this public repository while the preview is meant to be gated.

This is a temporary static preview gate, not server-side access control. Short passwords can be guessed offline. Do not use this preview to host confidential information. Refreshing or closing the page locks the preview again; no password is stored in browser storage. Use the Lock preview button to lock it immediately.

## Hosting

In repository Settings → Pages, choose **Deploy from a branch**, select **main** and **/ (root)**, then click **Save**. GitHub publishes updates automatically after changes to `main`; no custom Actions workflow is needed. Keep `.nojekyll` in the root. Branch publishing serves repository files, so never commit unencrypted preview content, private submissions, or secrets. Public documentation such as this README remains accessible.

Configure **fairhill-civ.com** as the custom domain in Pages. At the domain's DNS provider, add GitHub's current apex A records and optionally a `www` CNAME to `Emmahl98.github.io`. Follow GitHub's current DNS guidance, verify ownership, and enable HTTPS after DNS and certificate provisioning complete. Preserve email records.

## Current implementation

The decrypted preview has Home, Government, Laws & treaties, The Fairhill Gazette, and Contact views. Full documents and office details still link to the existing Notion archive. The design is predominantly white, with #6fa710 green and #ff7f0a orange. The homepage uses the original Fairhill town panorama with different crops, plus the original flag. Gazette cards use their corresponding issue covers. These images currently load from the existing public Notion image URLs. The contact form is disabled until its Google Apps Script endpoint is activated. Discord OAuth is not implemented.

## Updating the preview

Maintain the unencrypted source outside this public repository. Inline its CSS and JavaScript (JavaScript must execute after the body exists), then encrypt that HTML with a freshly generated 16-byte salt and 12-byte IV. Replace the encrypted payload in `index.html`. Preserve the gate, PBKDF2 parameters, password value chosen by the owner, and AES-GCM authentication. Verify correct-password decryption and wrong-password rejection before deploying.

## Planned in-game imagery

Replace the shared placeholder with these owner-provided screenshots when available:

| Placement | Suggested image | Composition |
| --- | --- | --- |
| Homepage hero | Town panorama: landmark buildings, rooftops and landscape | Wide 4:3 crop; keep the important landmark near the centre and leave top-right clear for the small text card |
| About Fairhill | Citizens together in a town square or a community event | Landscape 4:3; include people and recognisable architecture |
| Territory | Kingdom map or high aerial screenshot | Landscape; legible settlements and borders; a future interactive map can replace the image |
| Gazette cards | Relevant building, event or people for each issue | Landscape 16:10 crops, with no essential details in the top-left label area |
| Discord/community section | Group screenshot, gathering or shared build | Landscape 16:10 with people as the focal point |

Screenshots around 1600–2400 pixels wide are useful. Hide HUD/chat where possible, and capture alternate crops if you can. The existing town imagery and Gazette covers are in place for now; owner-provided screenshots can replace the repeated panorama later. Government office pages can later use building exteriors or portraits without requiring them for this version.
