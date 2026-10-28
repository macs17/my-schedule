# Облако Supabase (бесплатно)

## 1. Создай проект
1. Зайди на https://supabase.com и зарегистрируйся
2. New project → имя любое → регион ближайший → Create
3. Подожди ~2 минуты

## 2. Таблица
SQL Editor → New query → вставь и Run:

```sql
create table if not exists public.user_sync (
  user_id uuid primary key references auth.users (id) on delete cascade,
  payload jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now()
);

alter table public.user_sync enable row level security;

create policy "Users read own sync"
  on public.user_sync for select
  using (auth.uid() = user_id);

create policy "Users insert own sync"
  on public.user_sync for insert
  with check (auth.uid() = user_id);

create policy "Users update own sync"
  on public.user_sync for update
  using (auth.uid() = user_id)
  with check (auth.uid() = user_id);
```

## 3. Ключи
Project Settings → API:
- Project URL
- anon public key

## 4. В приложении
Меню → Облако → «Настройка» → вставь URL и anon key → Сохранить ключи
→ Регистрация (email + пароль) или Войти

## 5. Auth (рекомендуется)
Authentication → Providers → Email:
- можно отключить "Confirm email" для простого старта
