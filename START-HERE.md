# SK HomeCare — final package

## Folders
- `website/` → Firebase Hosting serves this folder as the public site.
- `backend-do-not-upload/` → Supabase SQL only. Never upload this folder to the website.
- `firebase.json` → root Hosting config; its `public` setting points to `website/`.

## Before first deployment
1. Supabase → Authentication → Users: create `shaikhayan7744@gmail.com`. If Auto Confirm is off, complete email verification before signing in. This account is already confirmed and assigned as the admin in the current Supabase project.
2. Authentication → Providers → Email → turn off **Enable Sign-ups** after confirming you can sign in as admin.
3. The setup SQL is already applied to the current project. It includes the required table grants, with row-level security still enabled. For a new Supabase project, run all of `backend-do-not-upload/supabase-setup.sql` after creating the admin user.
4. The homepage and admin panel already contain the same Supabase Project URL and anon/publishable key. Never use a `service_role` key in either file.
5. Firebase Hosting CLI is not installed in this workspace. Install and sign in, then run these commands from the project root (the folder containing `firebase.json`):
   ```text
   npm install -g firebase-tools
   firebase login
   firebase use --add
   firebase deploy --only hosting
   ```
   `firebase use --add` asks you to choose your Firebase project. Hosting deploys only `website/`; the Supabase SQL folder is excluded.
6. After the first deploy, add the Firebase Hosting URL in Supabase → Authentication → URL Configuration. Set the Site URL to the live site URL; if you later connect `https://skhomecare.in`, update it to that address.
7. Before launch, test `/admin.html` with the confirmed admin account and make one clearly labeled test booking, verify it appears in admin, then remove that test record. Test a non-admin account and confirm it cannot open admin data.
8. Confirm the published business hours and “Same-day service” claim are accurate. Google review buttons remain inactive until real Google review URLs are added to `SKCONFIG`.

## Add another admin later
Create the user in Supabase Authentication → Users, then run once in SQL Editor:

```sql
INSERT INTO public.admin_users (user_id)
SELECT id FROM auth.users WHERE email = 'their-email@example.com'
ON CONFLICT DO NOTHING;
```
