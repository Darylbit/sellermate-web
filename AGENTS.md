# SellerMate — AI development rules

## Purpose and scope
Build a Traditional Chinese (zh-Hant) ecommerce analytics website for Malaysian small sellers. Currency is MYR (RM). The public GitHub Pages site is **client-side only**, with no live LLM, no API keys, and no misleading claims of real sales data. The sample dataset is explicitly marked as simulated. This site is a reusable front-end foundation for a later full SellerMate AI Agent project.

## Required pages and routes
- `index.html`: dashboard with clearly labeled sample data and summary KPIs.
- `calculator.html`: working profit and target-price calculator, including invalid-input handling.
- `products.html`: searchable, sortable product comparison and CSV import; local processing only.
- `about.html`: architecture, features, limitations, and future roadmap.
All pages must have consistent navigation, responsive layout, accessible labels, and a link back to the homepage. Use relative links for GitHub Pages project subpaths.

## Technology and organization
- Plain HTML5, CSS3, and vanilla JavaScript (ES modules). No build step or external runtime dependencies.
- Shared styles in `css/styles.css`; shared data and pure formulas in `js/core.js`; page logic in `js/app.js`.
- Keep calculations pure and testable; never calculate financial values by prompting an LLM.
- Data is imported locally in the browser; never upload it to an external server.
- No secrets, credentials, tracking, remote CDNs, or backend URLs in committed files.

## Financial definitions
- `platformFee = sellingPrice * platformFeePct / 100`.
- `netProfit = sellingPrice - productCost - otherCost - shippingCost - adCost - platformFee`.
- `netMarginPct = netProfit / sellingPrice * 100` when sellingPrice > 0.
- `minimumPrice = fixedCosts / (1 - platformFeePct/100 - targetMarginPct/100)`; reject denominator <= 0.
- `ROAS = revenue / adSpend` and `CPA = adSpend / orders`; if denominator is zero, show N/A, never infinity.
- Money: RM with two decimal places; negative numbers explicitly labeled as losses.
- Results are estimates excluding taxes/returns unless entered in other costs. Do not confuse ROAS with profit.
- Never silently clamp user inputs. Validate finite nonnegative values and sensible percentages.

## UI and accessibility
- Brand: SellerMate, teal/indigo accents, generous whitespace, high contrast, responsive cards.
- All form fields must have visible labels and clear inline errors. Use semantic HTML, headings, landmarks, and keyboard-focus indicators.
- Charts must include textual equivalents or tables. Respect reduced motion.
- Do not show sample data as real shop performance.

## CSV contract
UTF-8 CSV headers: `name,price,cost,orders,ad_spend,other_cost,shipping_cost,platform_fee_pct`.
- `name` nonempty, `price` > 0, `cost`, `orders`, `ad_spend`, `other_cost`, `shipping_cost` >= 0, `platform_fee_pct` 0..100.
- Allow optional columns `other_cost`, `shipping_cost`, `platform_fee_pct`; use documented defaults (0, 0, 10).
- CSV parsing must support quoted fields and escaped double quotes; reject invalid rows with actionable messages.
- All imported data stays in memory; no localStorage persistence without explicit user consent.

## Change procedure (follow this every time)
1. Read this file and `README.md` before making edits.
2. Identify affected pages, shared components, and financial formulas.
3. Implement changes in shared CSS/JS rather than copying logic across pages.
4. Keep navigation, mobile responsiveness, and accessible labeling intact.
5. Add/update tests for every change to formulas or CSV parsing.
6. Run `node tests/test-core.mjs` and manually verify all four pages on desktop/mobile widths.
7. Update `README.md` with any new feature, input schema, or limitation.
8. Explain what changed, what was tested, and any remaining limitations.

## Extension examples
- New page: create `reports.html`, add link to nav on all pages, reuse header/footer/classes, add page-specific initializer to `js/app.js`, document it in README.
- New financial metric: add pure function to `js/core.js`, add edge-case tests, then display it in UI with a clear definition.
- AI chat: only after adding a secure backend/proxy with server-side credentials, rate limiting, and explicit data privacy handling; never call Groq with a public frontend key.

## Acceptance criteria
All four pages load on GitHub Pages; calculators produce correct results and show errors; CSV sample can be imported; product comparison updates; sample-vs-imported state is obvious; no console errors; no secrets; tests pass; site remains usable on phones.
