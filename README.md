# Rate Limiting with Throttler

A NestJS mini-project demonstrating how to protect an API from abuse by limiting how many requests a client can make in a given time window, using `@nestjs/throttler`.

## How It Works

- `ThrottlerModule.forRoot()` defines the rate limit rule: a time window (`ttl`) and a max request count (`limit`) within that window.
- `ThrottlerGuard` is registered globally (via `APP_GUARD`), so it runs before every request reaches a controller.
- If a client exceeds the limit within the time window, the guard rejects the request with `429 Too Many Requests` instead of letting it through.

## Rule Used

```
ttl: 60000ms (60 seconds)
limit: 10 requests
```

Meaning: a client can make at most 10 requests per 60 seconds. The 11th request within that window is rejected.

## Setup

```bash
pnpm install
```

## Running

```bash
pnpm run start:dev
```

The server runs on `http://localhost:3000`.

## Testing

Send `GET /` repeatedly (11+ times quickly):

- Requests 1–10 → `200 OK`
- Request 11+ (within the same 60s window) → `429 Too Many Requests`

```json
{
  "statusCode": 429,
  "message": "ThrottlerException: Too Many Requests"
}
```

## Tech Stack

- NestJS
- `@nestjs/throttler`

## What I Learned

- The difference between defining a rule (`ThrottlerModule.forRoot()`) and actually enforcing it (`ThrottlerGuard`)
- Registering a built-in guard globally with the `APP_GUARD` token — the same pattern used for custom guards
- What `ttl` and `limit` mean in a throttling rule
- Why rate limiting matters: protecting against brute-force attacks, abuse, and server overload
- "Throttling" as the technical term for rate limiting (from the automotive idea of a throttle valve restricting flow)