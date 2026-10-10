# Hy Tech Solutions Django platform

This project is a Django/Python implementation of the requested Hy Tech Solutions website and internal portal foundation.

## Run locally

1. Copy `.env.example` to `.env` and replace the placeholders with your local values. Do not commit `.env`.
2. Install dependencies: `python -m pip install -r requirements.txt`
3. Apply migrations: `python manage.py migrate`
4. Create starter content: `python manage.py seed_demo`
5. Create an admin account: `python manage.py createsuperuser`
6. Start the server: `python manage.py runserver`

## Deploy to Vercel

The project includes `api/index.py`, `vercel.json`, WhiteNoise static-file handling, and a production static collection command. From the project root:

1. Install dependencies with `python -m pip install -r requirements.txt`.
2. Run `python manage.py collectstatic --noinput` locally to verify static assets.
3. Import the project into Vercel and keep the framework preset as `Other`.
4. Add the variables from `.env.example` in Vercel Project Settings. Use `DJANGO_DEBUG=False`, set `DJANGO_ALLOWED_HOSTS` to the Vercel domain, and set `CSRF_TRUSTED_ORIGINS` to the `https://` domain.
5. Set the TiDB, SMTP, Cloudinary, Groq, and Gemini values as Vercel environment variables for Production, Preview, and Development as needed.
6. Run migrations against TiDB before the first production request: `python manage.py migrate --noinput`.

Vercel functions are stateless, so TiDB should be used for production data. The included `tidb-ca.pem` is a public CA certificate used for the TiDB TLS connection; do not put passwords or API keys in source control.

Unauthenticated visitors see the marketing landing page. Authenticated users are redirected to `/dashboard/`, where task and project metrics are calculated from the database. Admins can edit branding, contact information, services, projects, tasks and integration credentials at `/admin/`.

The settings module uses SQLite by default for local development and switches to TiDB/MySQL when `DATABASE_URL` or the `TIDB_*` variables are present. Email, Cloudinary, Groq and Gemini credentials are read from environment variables. The admin-managed `IntegrationSettings` singleton is available for authenticated administrators who need to update those values after login.

Rotate the credentials shared in the original request before deployment because they have been exposed in the conversation. Set new values in Vercel environment variables or in the authenticated admin integration settings.
