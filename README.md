# Contacts API — Stage 2 (Auth + JWT + Ownership + Email Verify + CORS + Avatar)

## Запуск (Docker Compose)
```bash
cp .env.example .env
# за потреби змініть SECRET_KEY, CLOUDINARY_URL, SMTP_*
docker compose up -d --build
# Swagger: http://127.0.0.1:8000/docs
```

## Потік перевірки
1) `POST /auth/register` → 201 Created. Якщо email зайнятий → 409.
2) Перейди за посиланням з логів або з листа: `GET /auth/verify-email?token=...` → "Email verified".
3) `POST /auth/login` (form: `username`, `password`) → `access_token`.
4) Authorize у Swagger (Bearer).
5) CRUD `/contacts/**` — доступні лише verified користувачу та показують тільки власні контакти.
6) `GET /users/me` — з лімітами (за замовчуванням 5/хв).
7) `POST /users/me/avatar` — multipart `file`, зберігає у Cloudinary і повертає оновленого користувача.

## Нотатки
- Паролі зберігаються тільки у вигляді хешів (Passlib + bcrypt).
- JWT підписуються SECRET_KEY (з .env).
- Верифікаційні токени мають scope `email_verification` та окрему тривалість.
- CORS вмикається через `CORS_ALLOW_ORIGINS` (`*` або CSV зі списком).
- Для продакшну додайте Alembic і зовнішній rate-limit (Redis/SlowAPI).
# goit-pythonweb-hw-10
