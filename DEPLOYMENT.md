# Local setup and deployment

## Requirements

- Node.js 20 or newer
- A PostgreSQL database (the project uses Neon and Drizzle)
- A Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey)

## Configure environment

Copy `.env.example` to `.env.local` and set:

```env
DATABASE_URL=your_postgresql_connection_string
GEMINI_API_KEY=your_google_ai_studio_key
AUTH_SECRET=your_random_secret_at_least_32_characters
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your_google_maps_javascript_api_key
```

Generate a signing secret locally in PowerShell:

```powershell
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Keep `.env.local` private and never commit or share its contents. Use a separate database and secrets for each environment.

## Run locally

From the project directory:

```powershell
npm install
npm run db:push
npm run dev
```

Open the local URL printed by Next.js, usually `http://localhost:3000`.

## Deploy

1. Push the project to your Git provider and import it into a Next.js host such as Vercel.
2. Add `DATABASE_URL`, `GEMINI_API_KEY`, and `AUTH_SECRET` as server environment variables for the deployment. Set `NEXT_PUBLIC_GOOGLE_MAPS_API_KEY` as a public browser variable if you want Google Places address suggestions on the report form; restrict that key to your deployed domains and the Maps JavaScript API. Do not use the `NEXT_PUBLIC_` prefix for secrets.
3. Generate and apply the database schema against the production database with `npm run db:push`.
4. Deploy with `npm run build` as the build command and `npm run start` as the start command (managed Next.js hosts configure these automatically).
5. Test account registration, sign-in/sign-out, report verification, and collection verification on the deployed HTTPS site.

Authentication uses scrypt password hashes and signed, HTTP-only cookies. Password reset and email verification are not configured; add an email delivery service and recovery flow before relying on them for a public production service.
