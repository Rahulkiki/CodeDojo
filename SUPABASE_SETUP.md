# Supabase Setup for CodeDojo

To enable cloud authentication and data persistence, follow these steps:

## 1. Create a Supabase Project
1. Go to [Supabase](https://supabase.com/) and create a new project.
2. Under **Project Settings > API**, copy your `Project URL` and `anon public` key.

## 2. Update index.html
Open `index.html` and replace the following values at the top of the `<script>` tag:
```javascript
const SUPABASE_URL = "YOUR_PROJECT_URL";
const SUPABASE_ANON_KEY = "YOUR_ANON_KEY";
```

## 3. Set up the Database
Go to the **SQL Editor** in Supabase and run the following query to create the `problems` table:

```sql
create table problems (
  id bigint primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  user_id uuid references auth.users not null,
  name text,
  difficulty text,
  tags text[],
  description text,
  example text,
  solved boolean default false,
  bookmarked boolean default false,
  solvedDate text,
  notes text,
  code text
);

-- Enable Row Level Security
alter table problems enable row level security;

-- Create policy so users can only see their own data
create policy "Users can only access their own problems"
  on problems for all
  using ( auth.uid() = user_id );
```

## 4. Deploy
Once you've updated the keys, push your changes to GitHub:
```bash
git add .
git commit -m "Add Supabase Auth and Persistence"
git push origin main
```

Your Dojo is now cloud-powered! ⛩️
