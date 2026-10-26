# Classnote

A school study space for private class notes and student-built learning apps. App Studio uses GPT-6 Astra to turn a student's description into a small interactive preview.

> This is an early open-source starter. A school administrator must configure Supabase and Vercel AI Gateway before real accounts or AI app generation work. Review it for your school's privacy and security requirements before using it with students.

## What it includes

- Sign in with a school-issued 10-digit Student ID and password. IDs are strings, so leading zeroes are kept. The ID is not a password.
- No public registration: schools provision student accounts.
- Notes by subject, search, starring, editing, and deletion.
- A local demo with clearly labeled sample notes.
- App Studio calls `openai/gpt-6-astra` server-side through Vercel AI Gateway. Generated apps are saved per student and run in a restricted, no-network iframe preview.
- Follow-up prompts can revise a student's existing app. A teacher should review apps before they are shared.

## Local setup

1. Install Node.js 20 or newer, then run `npm install`.
2. Copy `.env.example` to `.env.local` and set the Supabase project URL and anon key.
3. Apply `supabase/migrations/202610260001_classnote.sql` in the Supabase SQL editor.
4. Enable email/password sign-in in Supabase Auth and disable public sign-up.
5. Have a school administrator provision accounts using `<10-digit-id>@students.classnote.app` as the email alias, a school-issued password, and confirmed email status. Do this in an admin-only workflow; never put the Supabase service-role key in browser code.
6. Set `AI_GATEWAY_API_KEY` in the local server environment or hosting secrets. Keep it server-side and set a spending budget for the Gateway key.
7. Run `npm run dev` and visit `http://localhost:3000`.

App generation uses Vercel AI Gateway's OpenAI Responses-compatible endpoint and the model ID `openai/gpt-6-astra`. Requests are metered. The app limits prompt/output size; administrators should also set Gateway and hosting budgets and add per-student rate limits before a school-wide rollout.

## Deploy

Deploy as a Next.js app to Vercel or another Node.js host. Configure `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, and `AI_GATEWAY_API_KEY` in the host's environment settings. Only the first two are public browser settings. Never commit real `.env` files or a Supabase service-role key.

## Student privacy and safety

- Use only with school approval and a school privacy/security review.
- Keep notes about schoolwork. Do not enter student names, contact details, health information, addresses, passwords, or other sensitive personal information into notes or prompts.
- Generated HTML is untrusted. It runs in a sandboxed iframe with a restrictive content security policy and no network access. This reduces risk; it does not make arbitrary generated code risk-free.
- Supabase Row Level Security (RLS) is the authority for each student's data boundary. Keep the policies in the migration enabled.
- Check the requirements that apply to your school and location before collecting student data.

## Patent note

This repository keeps its existing GPL-3.0 license. GPLv3 section 11 includes a patent license from contributors for patent claims essential to their contributions to the covered program. That does not prevent every third party from filing or asserting a patent on a broader idea. Public source code may be relevant prior art only for what it actually discloses; it cannot guarantee that no patent can be filed. Public disclosure may also affect the inventors' own patent rights in some jurisdictions. Get qualified patent advice before relying on publication for that purpose.

## License

GPL-3.0. See [LICENSE](./LICENSE).
