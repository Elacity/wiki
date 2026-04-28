# TokenID

The `TokenID` class provides a robust way to handle token IDs across the Elacity ecosystem, ensuring compatibility between hexadecimal representations and numeric values, especially when dealing with large numbers that exceed JavaScript's `Number.MAX_SAFE_INTEGER`.

## Interfaces

### ObjectTokenID

Represents the plain object form of a token ID, often used for storage or API transfer.

```typescript
export interface ObjectTokenID {
  tokenID?: number;
  hexTokenID?: string;
}
```

### ITokenID

A simple interface for objects that can be represented as a string.

```typescript
export interface ITokenID {
  toString(): string;
}
```

## TokenID Class

### Properties

- `tokenID: number`: The numeric representation (0 if `isBig` is true).
- `hexTokenID: string`: The hexadecimal representation.
- `isBig: boolean`: True if the value exceeds `Number.MAX_SAFE_INTEGER`.

### Static Methods

#### `TokenID.from(input: string | number): TokenID`

Creates a `TokenID` instance from a string (hex or decimal) or a number.

```typescript
import { TokenID } from '@elacity-js/common';

const tid1 = TokenID.from("0x123");
const tid2 = TokenID.from(456);
const tid3 = TokenID.from("115792089237316195423570985008687907853269984665640564039457584007913129639935");
```

#### `TokenID.fromObject({ hexTokenID, tokenID }): TokenID`

Resurrects a `TokenID` from an object representation.

```typescript
const tid = TokenID.fromObject({ hexTokenID: "0x1a", tokenID: 26 });
```

### Instance Methods

#### `toBigNumber(): bigint`
Returns the value as a native JS `bigint`.

#### `toJSON(): ObjectTokenID`
Returns the serializable object representation.

#### `toHex(): string`
Returns the hexadecimal string.

#### `toString(): string`
Returns the decimal string representation.
