# Fix false-looking writing rate-limit loop

## What will change
- Keep prompt batches at exactly 30 consecutive timestamps.
- Pace Agnes requests from the previous request's completion, not only its start, so queued batches cannot fire immediately after a long response.
- Give a confirmed Cloudflare 1015 block enough cooling time before a bounded retry instead of repeating every 45 seconds.
- Reduce duplicated surrounding-script text in each request while retaining the opening, local scene context, character bible, and all 30 requested timestamps.
- Replace the misleading status with a precise message that identifies Agnes's upstream temporary block and the automatic wait.

## Verification
- Check compilation and the current runtime logs.
- Invoke a real prompt batch and verify it either succeeds or reports the exact provider response without a stale “writer busy” state.
- Confirm batching remains 30 timestamps per request.

## Technical details
- Update the single Agnes queue and backoff policy in the server text client.
- Tighten prompt context sizing in the storyboard writer without changing timestamp-to-prompt mapping.
- Update only the writing progress message in the interface.
