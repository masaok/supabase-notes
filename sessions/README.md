## Refreshing Tokens and Persistent Sessions in Supabase

```typescript
import { createClient } from "@supabase/supabase-js";

export const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL;
export const supabaseAnonKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY;

export default createClient(supabaseUrl, supabaseAnonKey, {
  autoRefreshToken: true,
  persistSession: true,
  detectSessionChange: true,
});
```

Client-side: https://supabase.com/docs/reference/javascript/initializing?example=with-additional-parameters

Server-side: https://supabase.com/docs/reference/javascript/admin-api

Gemini: https://g.co/gemini/share/67d403d30de1
