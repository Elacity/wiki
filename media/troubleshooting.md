# Troubleshooting

Common issues and solutions when using the `@elacity-js/media` package.

## Installation Issues

### FormData Not Available (Node.js)

**Error:**
```
FormData is not available. In Node.js, please install "form-data" package
```

**Solution:**
```bash
npm install form-data
```

### Missing ABI Encoder

**Error:**
```
ABI encoder is required for minting
```

**Solution:**
Provide an ABI encoder function when creating `MediaUploadService`:

```typescript
import { defaultAbiCoder } from '@ethersproject/abi';

const mediaService = new MediaUploadService(apiClient, contractRunner, {
  abiEncoder: (types, values) => defaultAbiCoder.encode(types, values),
});
```

## Upload Issues

### Upload Progress Not Working

**Symptom:** Progress callback not being called during file upload.

**Causes:**
- Running in Node.js (XMLHttpRequest not available)
- Browser doesn't support XMLHttpRequest upload progress

**Solution:**
- Browser: Ensure you're using a modern browser
- Node.js: Progress tracking is not available, upload will complete without progress updates

### File Upload Fails

**Error:** `Upload failed: 400 Bad Request`

**Possible Causes:**
1. File too large (check backend limits)
2. Invalid file type
3. Missing authentication token

**Solution:**
```typescript
// Check file size
if (file.size > 2 * 1024 * 1024 * 1024) { // 2GB
  throw new Error('File too large');
}

// Ensure authenticated
await apiClient.auth.login(address, signature);
```

## Encoding Issues

### Encoding Never Completes

**Symptom:** Workflow hangs at encoding step.

**Possible Causes:**
1. Backend workflow not processing
2. Strategy not available (Firebase not initialized, polling service unavailable)
3. Polling timeout (backend)

**Solution:**
```typescript
// Check background job status
const job = await apiClient.backgroundJobs.retrieveBackgroundJob(requestId);
console.log('Job status:', job.status);
console.log('Steps:', job.steps);

// Check available strategies
import { WorkflowListenerFactory } from '@elacity-js/media';
const strategies = WorkflowListenerFactory.getAvailableStrategies(apiClient.backgroundJobs);
console.log('Available strategies:', strategies.map(s => s.constructor.name));

// For frontend, ensure Firebase is initialized if you want real-time updates
// Otherwise, polling strategy will be used automatically
```

### Encoding Fails

**Error:** Job status is `FAILED` at `dash_encode` step

**Possible Causes:**
1. Invalid transcode result
2. Backend encoding service error
3. Insufficient backend resources

**Solution:**
- Check backend logs
- Verify transcode completed successfully
- Retry the upload

## Minting Issues

### Mint Transaction Fails

**Error:** `Mint transaction failed`

**Possible Causes:**
1. Insufficient balance for minting fee
2. Invalid mint data encoding
3. Contract permissions issue

**Solution:**
```typescript
// Check balance
const balance = await provider.getBalance(address);
const mintingFee = await getMintingFee(); // Query CoreStorage contract
if (balance < mintingFee) {
  throw new Error('Insufficient balance');
}

// Verify ABI encoder is correctly configured (Ethers example)
import { EthersAbiEncoder } from '@elacity-js/contracts-ethers-adapter';
const abiEncoder = new EthersAbiEncoder();
```

### Token ID Not Extracted

**Symptom:** `tokenId` is undefined or incorrect

**Cause:** Current implementation uses `totalSupply - 1` as workaround

**Solution:**
- Token ID should be available in the result
- If not, query the channel contract for the latest token ID
- Future: Event parsing will be added to contract runner interface

## Background Job Issues

### Job Not Found

**Error:** `Background job {requestId} not found`

**Possible Causes:**
1. Job was deleted
2. Wrong requestId
3. Authentication issue (jobs are user-scoped)

**Solution:**
```typescript
// Verify authentication
const token = apiClient.auth.getToken();
if (!token) {
  await apiClient.auth.login(address, signature);
}

// List all jobs to find the correct one
const jobs = await apiClient.backgroundJobs.fetchBackgroundJobs();
console.log('My jobs:', jobs.data);
```

### Job Status Stuck

**Symptom:** Job status doesn't update

**Possible Causes:**
1. Backend not updating job
2. Network issues
3. Job was created on different account

**Solution:**
- Check backend logs
- Verify you're authenticated with the same account that created the job
- Manually update job if needed:
```typescript
await apiClient.backgroundJobs.updateBackgroundJob(requestId, {
  $set: { status: JobStatus.COMPLETED },
});
```

## Performance Issues

### Slow Uploads

**Symptom:** File upload takes too long

**Solutions:**
- Check network connection
- Verify backend is responding
- Consider chunked uploads for very large files (future enhancement)

### Polling Too Frequent

**Symptom:** Too many API calls when polling

**Solution:**
- Adjust polling interval (currently 2 seconds)
- Use Firebase listeners in frontend instead of polling
- Consider exponential backoff for failed requests

## Best Practices

1. **Always store `requestId`**: Save it locally to resume workflows after page reloads
2. **Handle errors gracefully**: Check job status on errors
3. **Use progress callbacks**: Provide user feedback during long operations
4. **Set appropriate timeouts**: Don't wait indefinitely for workflows
5. **Monitor background jobs**: Periodically check job status for long-running operations

## Getting Help

- Check [Media Upload Service](services/media-upload-service.md) for API details
- Review [Workflow Architecture](architecture/workflow.md) for flow understanding
- See [Background Job Service](../api/services/background-job.md) for tracking details
