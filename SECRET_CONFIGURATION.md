# Server signing keys

These four independent Python projects require a private signing key in their
server process environment before starting the app or running Django commands:

| Project | Required variable |
| --- | --- |
| BRAIN TUMOR DETECTION | `BRAIN_TUMOR_FLASK_SECRET_KEY` |
| Human Activity Detection | `HUMAN_ACTIVITY_FLASK_SECRET_KEY` |
| Project047 - ShoppingWebsitewithDjango | `SHOPPING_DJANGO_SECRET_KEY` |
| Project070 - CalculatorWithDjango | `CALCULATOR_DJANGO_SECRET_KEY` |

Generate a separate key for each app using Python's standard library:

```bash
export BRAIN_TUMOR_FLASK_SECRET_KEY="$(python -c 'import secrets; print(secrets.token_urlsafe(64))')"
```

In PowerShell:

```powershell
$env:BRAIN_TUMOR_FLASK_SECRET_KEY = python -c "import secrets; print(secrets.token_urlsafe(64))"
```

Substitute the variable for the app you are running. For deployed apps, generate
the value privately and save it in the hosting platform's secret manager or
environment configuration. Keep each key stable across restarts and instances.
Never expose it through frontend code or commit the generated value.

The `.env.example` beside each application lists its required variable. The apps
do not automatically read `.env`; configure your process environment or hosting
platform explicitly. Startup rejects missing, empty, and whitespace-only keys.

If any app has been deployed using its previous committed key, rotate that key
before deployment. Previously committed values remain in Git history, so do not
reuse them or retain them as fallback signing keys. Rotation can invalidate
existing sessions, cookies, and signed links. The four tracked Django settings
bytecode files were also removed because they can retain the previous keys.
