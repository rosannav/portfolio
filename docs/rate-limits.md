---
aliases:
---
# Rate limits
!!! info "Source material"
    [https://web.archive.org/web/20260707035037/https://openlibrary.org/developers/api/#rate-limits](https://web.archive.org/web/20260707035037/https://openlibrary.org/developers/api/#rate-limits)

Open Library does not require authentication for search endpoints, but all requests are subject to rate limiting.

| Request type | Limit                 |
| ------------ | --------------------- |
| Default      | 1 request per second  |
| Identified   | 3 requests per second |

## Identified requests
To access the higher limit, include a `User-Agent` header with your application name and contact information:

```plaintext
User-Agent: MyAppName (contact@example.org)
```

Identified requests allow Open Library to contact you if your application generates sustained request volume.
