# Pagination Types

Generic types for handling paginated requests and responses across Elacity services.

## FilterPaginationInput

Standardized options for querying lists.

```typescript
export interface FilterPaginationInput {
  offset?: number;   // Number of items to skip
  limit?: number;    // Maximum items to return
  sortBy?: string;   // Field name to sort by
  searchBy?: string; // Search query string
}
```

## PaginatedResponse<T>

A generic wrapper for paginated data.

```typescript
export interface PaginatedResponse<T> {
  total: number;  // Total matching records in database
  offset: number; // Current offset
  limit: number;  // Requested limit
  data: T[];      // Array of items
}
```
