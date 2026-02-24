# Listener Strategies

> **Last Updated**: 2026-02-24

## Overview

`@elacity-js/media-packager` uses a workflow-level strategy interface for progress tracking. A `MediaUploadHandle` runs exactly one listener strategy for the full workflow lifecycle.

Available built-in strategies:

- `websocket` (default): subscribes to `wfp-socket/ws/{requestId}`
- `polling`: long-polls `backgroundJobs.retrieveBackgroundJob(requestId)`

You can also provide a custom strategy that implements `WorkflowProgressListenerStrategy`.

## Why This Model

- Listener selection happens once during workflow setup, not during encoding internals.
- The transport layer no longer hardcodes direct `this._socketUrl` management in the handle.
- Same public `IMediaUploadHandle` API regardless of strategy.

## Configure Strategy

Strategy is configured in `MediaUploadService` constructor options.

### Default (WebSocket)

```typescript
const mediaService = new MediaUploadService(apiClient, contractRunner, contractExecutor, {
  abiEncoder,
});
```

### Force Long Polling

```typescript
import { PollingProgressListenerStrategy } from '@elacity-js/media-packager/listeners';

const mediaService = new MediaUploadService(apiClient, contractRunner, contractExecutor, {
  abiEncoder,
  listenerStrategy: new PollingProgressListenerStrategy({ pollInterval: 3000 }),
});
```

### Force WebSocket

Do not pass `listenerStrategy`. WebSocket is the default built-in strategy.

### Provide Custom Strategy

```typescript
import {
  WorkflowProgressListenerStrategy,
  WorkflowListenerContext,
  UploadProgress,
} from '@elacity-js/media-packager';
import { JobStatus } from '@elacity-js/api';

class MyStrategy implements WorkflowProgressListenerStrategy {
  readonly name = 'my-strategy';

  private context?: WorkflowListenerContext;
  private listeners = new Set<(payload: UploadProgress) => void>();

  configure(context: WorkflowListenerContext): void {
    this.context = context;
  }

  start(): void {
    // start connection, then emit progress and sync step/job state
    this.context?.syncStep({
      step: 'transcode',
      completion: 50,
      status: JobStatus.PROCESSING,
      overallCompletion: 60,
    });

    this.listeners.forEach((listener) => listener({
      progress: 60,
      step: 'transcode',
      caption: 'Transcoding...',
    }));
  }

  stop(): void {}

  onProgress(callback: (payload: UploadProgress) => void): () => void {
    this.listeners.add(callback);
    return () => this.listeners.delete(callback);
  }

  isAvailable(): boolean {
    return true;
  }

  getPriority(): number {
    return 100;
  }
}

const mediaService = new MediaUploadService(apiClient, contractRunner, contractExecutor, {
  abiEncoder,
  listenerStrategy: new MyStrategy(),
});
```

## Handle Lifecycle

```typescript
const request = await mediaService.createRequest(input);
const handle = await mediaService.execute(request);

const unsubscribe = handle.onProgress((progress) => {
  console.log(progress.progress, progress.step, progress.caption);
});

handle.startListening();
await handle.waitCompletionOf('generate_metadata', {
  timeoutMs: 600_000, // 10 minutes (default)
});
handle.stopListening();
unsubscribe();
```

## Notes

- No Firebase dependency is used by this workflow listener system.
- In Node.js, WebSocket strategy requires a global `WebSocket` implementation. Use polling if unavailable.
- Strategy choice applies to both `createRequest()`/`execute()` and `resumeFromJob()` handles created by the same service instance.
