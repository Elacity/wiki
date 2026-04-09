# Utilities

The `@elacity-js/common` package includes various utility functions to simplify common tasks in both frontend and backend development.

## Async Utilities

### `wait(duration: number): Promise<void>`
Simple delay for the specified duration in milliseconds.

### `retry<T>(fn: () => Promise<T>, options?: RetryOptions): Promise<T>`
Retries an async function with exponential backoff.
- `retries`: Number of attempts (default: 5).
- `interval`: Base interval in ms (default: 0).
- `backoffFactor`: Multiplier for each attempt (default: 1).

### `fetchTimeout(resource: string | URL, options?: FetchTimeoutOptions): Promise<Response>`
A `fetch` wrapper with a built-in timeout (default: 30s).

### `fetchRetry(resource: string | URL, options?: FetchRetryOptions): Promise<Response>`
Combines `fetchTimeout` and `retry` for robust network requests.

### `promiseAllStepN<T>(n: number, tasks: (() => Promise<T>)[])`
Executes a list of async tasks with a maximum concurrency of `n`.

## URL & IPFS Utilities

### `ipfsLink(path: string): string`
Prefixes a CID or path with the default IPFS gateway URL.

### `toIpfsGateway(link: string): string`
Replaces generic IPFS links (`ipfs://` or Pinata) with the Elacity-configured gateway.

### `ipfsLinkFor(path: string): string`
Dynamic transformer that intelligently converts various path formats into internal IPFS gateway links.

### `convertIfHiveURL(url: string): string`
Converts `hive://` public gateway URLs into accessible HTTPS links.

## String Utilities

- `toLowerCase(val: string): string`: Safely converts to lowercase, handling null/undefined.
- `uid(length?: number): string`: Generates a random alphanumeric string (default length: 16).
- `randomUUID(): string`: Generates a standard UUID v4.
- `truncateText(text: string, start?: number, end?: number): string`: Truncates a string with ellipses (e.g., wallet addresses).

## Date Utilities

- `ensureDate(value: any): Date | null`: Safely converts various inputs (number, string, Date) into a `Date` object or `null`. Also exported as `asDate`.
- `timeFormat(durationInSeconds: number): string`: Formats a duration (e.g., `3661` -> `"1:01:01"`).
- `convertDuration(duration: DurationRecord): number`: Converts a duration object (e.g., `{ hours: 1, minutes: 30 }`) into total seconds.
- `convertDurationFromSeconds(durationInSeconds: number): DurationRecord`: Converts total seconds into a per-unit duration object (e.g., `3665` -> `{ hours: 1, minutes: 1, seconds: 5 }`).

## Other Utilities

- `isMobile(): boolean`: Detects mobile browsers via user agent.
- `isAppleMobile(): boolean`: Detects iOS devices.
- `popupWindowCenter(options: PopupOptions)`: Opens a centered popup window (useful for wallet connections or OAuth).
- `arrayIntersect<T>(arr1: T[], arr2: T[]): T[]`: Calculates intersection of two flat arrays.
- `onlyDefined(obj: Record<string, any>)`: Returns a new object containing only the keys that have defined values.
