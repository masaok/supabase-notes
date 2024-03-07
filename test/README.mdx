### Joining a table with multiple foreign keys using `ON` SQL behavior

```typescript
import { createClient } from "@supabase/supabase-js";

// Initialize the Supabase client
const supabaseUrl = "YOUR_SUPABASE_URL";
const supabaseKey = "YOUR_SUPABASE_KEY";
const supabase = createClient(supabaseUrl, supabaseKey);

async function fetchAttendanceWithScansAndFlatten() {
  const { data, error } = await supabase.from("attendance").select(`
      *,
      scans:scan_id_start!inner (
        timestamp
      )
    `); // Using !inner to ensure we only get records with a matching scan

  if (error) {
    console.error(error);
    return;
  }

  // Map the results to include scans.timestamp at the same level as attendance data
  const flattenedData = data?.map((item) => ({
    ...item,
    scan_timestamp: item.scans?.timestamp,
    scans: undefined, // Optional: remove the scans object if you don't need it anymore
  }));

  return flattenedData;
}

// Call the function to fetch and process the data
fetchAttendanceWithScansAndFlatten().then((data) => {
  console.log(data);
});
```

https://chat.openai.com/share/85cdc4a0-fac6-45c9-a775-3ed14b5116b3
