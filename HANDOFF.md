# HANDOFF — すばる陶芸 リニューアル

Updated: 2026-09-10 JST

## 1. Project goal

- 現行サイト `https://www.subarutougei.com/` をベースにリニューアルする。
- 現行サイトにある実写真・実作品を素材として使う。ストック写真へ置き換えない。
- 外部予約・地図などのリンクは忠実に維持する。
- 「少し重くても動きがあり、一般ユーザーが見て驚く」「コンペに出せる完成度」が方向性。
- ChatGPTはPMとして、確認待ちで止まりすぎず完成まで進める。

## 2. Production / repository

- Repository: `https://github.com/oosaka0123-sudo/subaru-tougei-renewal`
- Branch: `main`
- GitHub Pages: `https://oosaka0123-sudo.github.io/subaru-tougei-renewal/`
- IMPORTANT: `https://oosaka0123-sudo.github.io/` だけではこのサイトは開かない。LINE共有時も必ず `/subaru-tougei-renewal/` まで含める。
- Pages workflow: `.github/workflows/pages.yml`
- Latest functional site commit before this handoff update: `ce0b12b93eee7130b719ab53bd495a7260fa9065`
- Deploy run `34466509791` completed successfully.

## 3. Current pages

1. `index.html` — HOME / hero movie
2. `experience.html` — 1日陶芸体験
3. `works.html` — 作品紹介
4. `price.html` — 開講日・料金
5. `access.html` — アクセス・お問い合わせ

## 4. Hero movie — current important state

- File: `assets/hero-web.mp4`
- Source is REAL Subaru Tougei work photos, animated with Google Vertex AI Veo.
- Two source scenes were generated as slow camera-orbit videos and edited into one seamless-feeling background loop.
- Current output: about 30 sec / 1280x720 / about 2.5 MB / muted.
- The two Veo source clips were generated from actual work images, not newly invented stock-style pottery.
- Current playback is intentionally slow for promotion:
  - `assets/app.js`: hero video `playbackRate = 0.55`
  - `assets/styles.css`: hero video zoom / crop is stronger; mobile currently uses about `scale(1.26)`.
- User direction: TOP background should feel more promotional — closer crop, slower camera feeling, let the ceramic surface/details breathe.
- Do not revert this to the old 13-second slideshow-like movie.
- Generation provenance is recorded in `assets/PROVENANCE.md`.

## 5. Experience page image

- `experience.html` has one large real-work image near the top.
- Local asset: `assets/experience-real.jpg`
- This image came from the existing Subaru Tougei site material.
- Keep at least one real image on the experience page for trust / authenticity.

## 6. Contact / privacy decision

- Public email address display was REMOVED from all pages and JS because the user was concerned about spam.
- Do NOT re-add `subaru.pottery@gmail.com` visibly to HTML, footer, or JavaScript unless the user explicitly changes this decision.
- Current contact direction: phone + booking / reservation flow.
- Phone currently used: `06-6709-2511`.
- Booking link currently used: `https://www.asoview.com/channel/activities/subarutougei/offices/675/courses`

## 7. External links to preserve

- Asoview reservation link above.
- Google Maps link already embedded in access/footer.
- Existing-site link: `https://www.subarutougei.com/`
- Do not casually replace these with guessed URLs.

## 8. Critical bug history / DO NOT REPEAT

### UTF-8 corruption

A PowerShell cache-buster edit once rewrote `index.html` with the wrong encoding. Japanese text became mojibake and broken tags caused CSS not to apply. In LINE in-app browser it looked like an unstyled broken page.

Fix was restoring the correct UTF-8 homepage from the previous commit and redeploying.

Rules from now on:
- Never rewrite Japanese HTML with a command that can silently use Windows legacy encoding.
- Prefer GitHub connector writes, Python with `encoding='utf-8'`, or explicit UTF-8-safe tooling.
- After every HTML edit verify: Japanese text present, CSS HTTP 200, hero video HTTP 200, `Content-Type: text/html; charset=utf-8`.

### LINE / URL confusion

A screenshot that looked like the site had disappeared was actually the GitHub Pages user-root 404 page. The Subaru site itself remained at the project subpath.

Always share and test this exact base URL:
`https://oosaka0123-sudo.github.io/subaru-tougei-renewal/`

## 9. Local working copy on the Windows PC

- Publish working tree: `C:\Users\oosak\subaru-tougei-build\publish`
- Build/work folder: `C:\Users\oosak\subaru-tougei-build`
- Veo source outputs were created there, including `hero2-veo.mp4` and `hero3-veo.mp4`.
- A helper script `veo_orbit.py` was used during generation.
- Existing shared Google media tooling lives in `C:\Users\oosak\ai-agent` and supports Vertex AI / Veo.

## 10. AI media infrastructure

Existing `ai-agent` repository already implements Google Vertex AI media generation.
- Image model path exists.
- Video model used: Veo 3.1 fast generation path.
- Google Cloud project observed during generation: `rss7-ai-media`.
- Real Veo generation succeeded for this project.

## 11. Three-AI council status

The user often asks for ChatGPT + Claude + Gemini to discuss decisions.

An actual council run was attempted, but the orchestrator failed closed because required GitHub / Gemini / OpenAI secrets were not all registered. Therefore DO NOT claim that a real three-model consultation completed for the current site decisions.

Until those secrets are configured, it is okay to use three clearly-labelled design/implementation/UX perspectives internally, but say it is a role-based evaluation rather than a real external model meeting.

## 12. Next-session priorities

1. Open the exact Pages URL in LINE in-app browser and Chrome; visually QA the hero after the new `0.55` speed and close crop.
2. If the user still wants it slower / closer, tune only two places first: `playbackRate` in `assets/app.js` and hero-video transform/object-position in `assets/styles.css`.
3. Recheck all 5 pages on mobile, especially sticky reservation button and menu.
4. Verify every external booking/map link.
5. Keep email address hidden.
6. Consider OG / LINE share preview metadata once visual layout is approved.
7. Original Wix production domain has NOT been switched over; GitHub Pages is still the demo/public preview environment.

## 13. PM rule for continuation

Do not restart design from zero. Continue from the current published site. Preserve the successful Veo hero direction, real-work photography, five-page structure, and existing external links. Make focused improvements, deploy, then verify the actual public URL on mobile.
