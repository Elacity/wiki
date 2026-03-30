# Background Job Service

> **Last Updated**: 2025-01-29  
> **Package**: `@elacity-js/api`  
> **Service**: `BackgroundJobService`  
> **Used By**: `@elacity-js/media-packager` package

## Overview

The Background Job Service provides a comprehensive API for managing long-running asynchronous operations on the Elacity platform. It is primarily used by the `@elacity-js/media-packager` package to track media upload, transcoding, encoding, and minting workflows, allowing users to monitor progress and resume operations across sessions.

## Key Features

- **Job Lifecycle Management**: Create, update, retrieve, and delete background jobs
- **Progress Tracking**: Monitor job completion percentage and step-by-step status
- **Step-Based Workflows**: Track individual steps within a job (e.g., upload, transcode, encode, mint)
- **Metadata Generation**: Generate IPFS metadata URIs for NFT minting workflows
- **User-Scoped Queries**: Automatically filter jobs by authenticated user account

## Installation

The Background Job Service is part of the `@elacity-js/api` package. To use it in your project (or in the `@elacity-js/media-packager` package), install the API package:

```bash
npm install @elacity-js/api
# or
yarn add @elacity-js/api
```

**Note**: The service is exported from `@elacity-js/api` and can be accessed via `client.backgroundJobs` after initializing an `ElacityClient` instance.

## Usage

### Basic Setup

```typescript
import { ElacityClient } from '@elacity-js/api';
import { ChainId } from '@elacity-js/common';

const client = new ElacityClient({
  chainId: ChainId.BASE_MAINNET, // or your target chain
});

// Authenticate first
await client.auth.login(address, signature);

// Access the background job service
const backgroundJobs = client.backgroundJobs;
```

### Creating a Background Job

Create a new background job to track a media upload workflow:

```typescript
import { JobStatus } from '@elacity-js/api';

const job = await backgroundJobs.createBackgroundJob({
  title: 'Uploading video: My Awesome Video',
  status: JobStatus.INITIALIZED,
  completion: 0,
  payload: {
    channelAddress: '0x...',
    title: 'My Awesome Video',
    description: 'A great video',
  },
  steps: [
    {
      step: 'upload_file',
      status: JobStatus.PENDING,
      description: 'Uploading media file',
    },
    {
      step: 'transcode',
      status: JobStatus.INITIALIZED,
      description: 'Transcoding media',
      estimatedDuration: 180, // seconds
    },
    {
      step: 'dash_encode',
      status: JobStatus.INITIALIZED,
      description: 'Encoding to DASH format',
      estimatedDuration: 300,
    },
    {
      step: 'generate_metadata',
      status: JobStatus.INITIALIZED,
      description: 'Generating metadata',
    },
    {
      step: 'broadcast_tx',
      status: JobStatus.INITIALIZED,
      description: 'Minting to blockchain',
    },
  ],
});

console.log('Created job with requestId:', job.requestId);
```

### Updating Job Progress

Update job status and progress as workflow steps complete:

```typescript
// Update completion percentage
await backgroundJobs.updateBackgroundJob(job.requestId, {
  $set: {
    completion: 50,
    status: JobStatus.PROCESSING,
  },
});

// Update a specific step
await backgroundJobs.updateBackgroundJob(job.requestId, {
  $set: {
    'steps.$[i].status': JobStatus.COMPLETED,
    'steps.$[i].terminatedAt': new Date().toISOString(),
    'steps.$[j].status': JobStatus.PROCESSING,
    'steps.$[j].startedAt': new Date().toISOString(),
  },
  $options: {
    arrayFilters: [
      { 'i.step': 'transcode' },
      { 'j.step': 'dash_encode' },
    ],
  },
});
```

### Retrieving a Job

Fetch a specific job by its `requestId`:

```typescript
const job = await backgroundJobs.retrieveBackgroundJob(requestId);

if (job) {
  console.log(`Job ${job.requestId}: ${job.status} - ${job.completion}%`);
  console.log('Steps:', job.steps);
} else {
  console.log('Job not found');
}
```

### Listing User's Jobs

Fetch all background jobs for the authenticated user:

```typescript
// Get all jobs
const allJobs = await backgroundJobs.fetchBackgroundJobs();

// Filter by status
const activeJobs = await backgroundJobs.fetchBackgroundJobs(
  {
    status: [JobStatus.PENDING, JobStatus.PROCESSING],
  },
  {
    limit: 10,
    offset: 0,
    sortBy: 'updatedAt',
    sort: { updatedAt: -1 }, // Most recent first
  }
);

console.log(`Found ${activeJobs.total} active jobs`);
activeJobs.data.forEach((job) => {
  console.log(`- ${job.title}: ${job.completion}%`);
});
```

### Generating Metadata

Generate IPFS metadata URI for NFT minting:

```typescript
const metadataURI = await backgroundJobs.generateMetadata(job.requestId, {
  thumbnailCID: 'Qm...',
  encodeResult: {
    cid: 'Qm...',
    kid: '...',
    // ... other encode metadata
  },
});

console.log('Metadata URI:', metadataURI);
// Use this URI when minting the NFT
```

### Deleting a Job

Remove a completed or cancelled job:

```typescript
const result = await backgroundJobs.deleteBackgroundJob(requestId);
console.log('Deleted:', result.success);
```

## API Reference

### Types

#### `JobStatus`

Enumeration of possible job statuses:

```typescript
enum JobStatus {
  INITIALIZED = 'INITIALIZED',
  PENDING = 'PENDING',
  PROCESSING = 'PROCESSING',
  COMPLETED = 'COMPLETED',
  FAILED = 'FAILED',
  IDLE = 'IDLE',
}
```

#### `BackgroundJob<T>`

Main job entity:

```typescript
interface BackgroundJob<T = Record<string, any>> {
  _id: string;
  account: BackgroundJobAccount;
  requestId: string;
  title?: string;
  status: JobStatus;
  completion: number; // 0-100
  payload?: T;
  pendingActions?: PendingAction[];
  steps?: JobStep[];
  createdAt?: Date | string | number;
  updatedAt?: Date | string | number;
}
```

#### `JobStep`

Individual workflow step:

```typescript
interface JobStep {
  step: string;
  status: JobStatus;
  description?: string;
  caption?: string;
  startedAt?: Date | string | number;
  terminatedAt?: Date | string | number;
  estimatedDuration?: number; // seconds
  args?: Record<string, any>;
}
```

### Methods

### Methods

#### `createBackgroundJob<T>(input): Promise<BackgroundJob<T>>`

Creates a new background job to track a workflow.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `input` | `BackgroundJobInput` | Initial job configuration. |

**Returns:** `Promise<BackgroundJob<T>>` - The created job entity with a unique `requestId`.

Authentication: **Required**

---

#### `updateBackgroundJob<T>(requestId, input): Promise<BackgroundJob<T>>`

Updates an existing job using MongoDB-style update operations.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `requestId` | `string` | The unique identifier of the job. |
| `input` | `UpdateBackgroundJobInput` | Update operations (e.g. `$set`, `$unset`). |

**Returns:** `Promise<BackgroundJob<T>>`

**Example:**
```typescript
await backgroundJobs.updateBackgroundJob(requestId, {
  $set: {
    completion: 75,
    'steps.$[i].status': JobStatus.COMPLETED,
  },
  $options: {
    arrayFilters: [{ 'i.step': 'dash_encode' }],
  },
});
```

Authentication: **Required**

---

#### `retrieveBackgroundJob<T>(requestId): Promise<BackgroundJob<T> | null>`

Retrieves a single job by its `requestId`.

**Returns:** `Promise<BackgroundJob<T> | null>`

Authentication: **Required**

---

#### `fetchBackgroundJobs<T>(query?, options?): Promise<FetchBackgroundJobsResponse>`

Fetches a paginated list of jobs scoped to the authenticated user.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `query` | `BackgroundJobQueryInput` | Filtering criteria (status, title). |
| `options` | `FilterPaginationInput` | [Pagination options](../../common/Pagination.md) |

**Returns:** `Promise<FetchBackgroundJobsResponse>`

Authentication: **Required**

---

#### `generateMetadata(requestId, payload?): Promise<string>`

Generates IPFS metadata URI for NFT minting workflows. This method combines the job payload with provided additional metadata.

**Parameters:**
- `requestId`: `string`
- `payload`: `Record<string, any>` (optional) - Additional metadata to include.

**Returns:** `Promise<string>` - The IPFS URI (e.g. `ipfs://Qm...`).

Authentication: **Required**

---

#### `deleteBackgroundJob(requestId): Promise<{ success: boolean }>`

Deletes a background job.

**Returns:** `Promise<{ success: boolean }>`

Authentication: **Required**

## Common Workflows

### Media Upload Workflow

Complete example for tracking a media upload and minting process:

```typescript
import { ElacityClient, JobStatus } from '@elacity-js/api';

const client = new ElacityClient({ chainId: ChainId.BASE_MAINNET });
await client.auth.login(address, signature);

// 1. Create job when upload starts
const job = await client.backgroundJobs.createBackgroundJob({
  title: `Uploading: ${videoTitle}`,
  status: JobStatus.INITIALIZED,
  payload: {
    channelAddress,
    title: videoTitle,
    description,
  },
  steps: [
    { step: 'upload_file', status: JobStatus.PENDING },
    { step: 'transcode', status: JobStatus.INITIALIZED },
    { step: 'dash_encode', status: JobStatus.INITIALIZED },
    { step: 'generate_metadata', status: JobStatus.INITIALIZED },
    { step: 'broadcast_tx', status: JobStatus.INITIALIZED },
  ],
});

// 2. Update progress during upload
await client.backgroundJobs.updateBackgroundJob(job.requestId, {
  $set: {
    completion: 20,
    'steps.$[i].status': JobStatus.PROCESSING,
  },
  $options: {
    arrayFilters: [{ 'i.step': 'upload_file' }],
  },
});

// 3. Mark upload complete, start transcode
await client.backgroundJobs.updateBackgroundJob(job.requestId, {
  $set: {
    completion: 40,
    'steps.$[i].status': JobStatus.COMPLETED,
    'steps.$[i].terminatedAt': new Date().toISOString(),
    'steps.$[j].status': JobStatus.PROCESSING,
    'steps.$[j].startedAt': new Date().toISOString(),
  },
  $options: {
    arrayFilters: [
      { 'i.step': 'upload_file' },
      { 'j.step': 'transcode' },
    ],
  },
});

// 4. Continue updating as workflow progresses...
// (transcode → encode → metadata → mint)

// 5. Generate metadata before minting
const metadataURI = await client.backgroundJobs.generateMetadata(job.requestId, {
  thumbnailCID,
  encodeResult,
});

// 6. After successful mint, mark job complete
await client.backgroundJobs.updateBackgroundJob(job.requestId, {
  $set: {
    completion: 100,
    status: JobStatus.COMPLETED,
    'steps.$[i].status': JobStatus.COMPLETED,
    'steps.$[i].terminatedAt': new Date().toISOString(),
  },
  $options: {
    arrayFilters: [{ 'i.step': 'broadcast_tx' }],
  },
});
```

### Polling for Job Status

Poll a job to check its progress:

```typescript
async function pollJobStatus(
  requestId: string,
  onUpdate: (job: BackgroundJob) => void,
  intervalMs: number = 2000
): Promise<BackgroundJob> {
  return new Promise((resolve, reject) => {
    const interval = setInterval(async () => {
      const job = await client.backgroundJobs.retrieveBackgroundJob(requestId);
      
      if (!job) {
        clearInterval(interval);
        reject(new Error('Job not found'));
        return;
      }

      onUpdate(job);

      if (job.status === JobStatus.COMPLETED) {
        clearInterval(interval);
        resolve(job);
      } else if (job.status === JobStatus.FAILED) {
        clearInterval(interval);
        reject(new Error(`Job failed: ${job.title}`));
      }
    }, intervalMs);
  });
}

// Usage
try {
  const completedJob = await pollJobStatus(
    requestId,
    (job) => console.log(`Progress: ${job.completion}%`),
    2000
  );
  console.log('Job completed!', completedJob);
} catch (error) {
  console.error('Job failed:', error);
}
```

## Integration with Media Package

The Background Job Service is designed to work seamlessly with the `@elacity-js/media-packager` package for media upload workflows. The media package will use this service to:

- Create background jobs when media uploads start
- Update job progress during transcoding and encoding
- Track minting workflow steps
- Generate metadata URIs before blockchain minting
- Monitor job completion status

See the [Media Package Documentation](../../media-packager/SUMMARY.md) for media-specific integration examples.

## Related Documentation

- [Media Minting Process](../../../../elacity-web/docs/wiki/Technical/Media-Minting-Process.md) - Frontend implementation details
- [GraphQL Schema](../../../../packages/api/graphql/schema.graphql) - Complete API schema
- [Channel Service](./channel.md) - Channel management for media uploads

## Error Handling

All methods throw errors on failure. Handle them appropriately:

```typescript
try {
  const job = await client.backgroundJobs.createBackgroundJob(input);
} catch (error) {
  if (error.message.includes('authentication')) {
    // Re-authenticate
    await client.auth.login(address, signature);
  } else {
    console.error('Failed to create job:', error);
  }
}
```

## Best Practices

1. **Always store `requestId`**: The `requestId` is the primary identifier for jobs. Store it locally to resume workflows after page reloads.

2. **Use step-based tracking**: Break workflows into discrete steps for better progress granularity.

3. **Update completion percentage**: Keep `completion` field synchronized with actual progress for accurate UI display.

4. **Handle failures gracefully**: Check `status === JobStatus.FAILED` and provide user feedback.

5. **Clean up completed jobs**: Consider deleting old completed jobs to reduce clutter (optional, based on your use case).

6. **Poll responsibly**: When polling for updates, use reasonable intervals (2-5 seconds) to avoid excessive API calls.
