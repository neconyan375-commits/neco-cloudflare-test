# Progress

## 2026-11-01 04:04 JST — Supabase read-only connection test

- Updated the existing card with loading, success, and error states. Uses native `fetch()` with the existing publishable key, no external JavaScript dependencies, and a 10-second timeout. The existing GitHub → Cloudflare text and design are preserved.
- Target selected by the user: `neco-marubatsu-online`. Created only `public.cloudflare_test` (`id bigint primary key`, `message text not null`) and the single row `(1, 'Hello from Supabase! 🐾')`.
- Enabled RLS. On this new table only, revoked inherited grants from PUBLIC, anon, and authenticated, then granted only SELECT to anon. The only policy is SELECT TO anon USING (id = 1). Schema/default grants and existing objects/settings were not changed.
- Database verification: required schema, primary key, RLS, single row, SELECT-only grant, and single SELECT policy confirmed. Metadata fingerprints for existing public objects, functions, policies, defaults, role settings, schema ACLs, and publication membership matched before/after setup.
- Publishable-key REST checks: SELECT returned HTTP 200 with the required row; POST, PATCH, and DELETE each returned HTTP 401 / SQLSTATE 42501. Verification requests targeted only the dedicated test table.
- Browser preview used the production origin with the document replaced locally and real, unmocked Supabase traffic for the success test. Verified the success message and no uncaught page errors at desktop/tablet/mobile sizes. Simulated loading, HTTP 503, network failure, empty results, and the real 10-second timeout; expected status text appeared in each case. No horizontal overflow at widths 1440, 768, 390, and 320.
- Source scan found no secret key, legacy JWT key, or privileged credential. Only the intentionally public publishable key is included.
- Existing project advisor notices were left unchanged and are recorded in the local handoff memo; this test does not include remediation of existing application objects.
- After updating main, verify the Cloudflare commit check and open the unmodified production URL to confirm the live result. No Cloudflare configuration changes or manual deployments are required.

## 2026-11-01 04:06 JST — Production verification and missing icon

- Commit `2ea169b5279be54aa4768c4d7d51aafa2896ff67` reached main through the GitHub connector; Cloudflare's automatic build completed successfully (check run `124629562174`).
- The unmodified production URL returned the exact committed HTML. Its browser GET to Supabase returned HTTP 200, `Content-Profile: public`, and CORS approval for the production origin. Confirmed `Supabase connection: OK` and `Hello from Supabase! 🐾`, with no uncaught JavaScript errors.
- The first production console check also showed a missing-resource 404; `/favicon.ico` was absent. Added a tiny inline SVG favicon to prevent that extra request without adding files or external resources. Repeat the production console check after the follow-up auto deployment.
