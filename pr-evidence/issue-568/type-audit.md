# JSDoc return-value review for issue #568

Reviewed head `8acdd9a6822a4edce0440cf534c0edb82a3a5f27` against base `e809ccd993cca12ded4dfc40b9ad79233ed71a19`.

The following 27 functions were traced through their existing return/resolve paths. Promise types describe fulfilled values; rejected errors are not part of the fulfilled-value union.

| Function | Evidence for the annotation |
| --- | --- |
| `updateCache` | Disabled-cache path returns `Promise.resolve()`; enabled path adopts `Cache.put`, which completes with undefined: `Promise<void>`. |
| `StorageFirefoxOS.get` | DeviceStorage request success resolves the returned File: `Promise<File>`. |
| `StorageFirefoxOS.scanForArchives` | Pushes matching `file.name` strings and resolves the array at end of enumeration: `Promise<Array<string>>`. |
| `populateOPFSStorageQuota` | Estimate handler updates the DOM without returning a value; unsupported API path falls through: `Promise<void>\|undefined`. |
| `cache.wait` | Resolves its documented optional string/object argument unchanged; omission resolves undefined. |
| `checkUrl` | Successful HEAD request calls `resolve()`; all error paths reject: `Promise<void>`. |
| `checkUrlByImage` | Successful image load calls `settle(resolve)` without an error/value: `Promise<void>`. |
| `checkAltLibrary` | Returns either `checkUrlByImage` or `checkUrl`, both `Promise<void>`. |
| `initializeFrame` | Load handler writes the template then calls `resolve()`: `Promise<void>`. |
| `binarySearch` | Empty search returns a number/null directly; other calls return a Promise whose recursive/direct results are numbers or null. |
| `PromiseQueue.enqueue` | Queued factory's fulfillment handler passes its value to the enqueuing Promise's resolver, preserving generic `T`. JSDoc now belongs to the method. |
| `xz.readSlice` | Allocates an `Int8Array`, awaits `_readLoop`, then returns that data: `Promise<Int8Array>`. |
| `xz.readSliceSingleThread` | Returns `readSlice` or waits and adopts its fulfillment/rejection: `Promise<Int8Array>`. |
| `xz._readLoop` | Returns the allocated `Int8Array` when complete or recursively adopts another loop. |
| `xz._fillInBufferIfNeeded` | Both input-not-empty and filled-buffer paths resolve literal `0`: `Promise<0>`. |
| `_searchArchiveParts` | Collects Files from `storage.get` and resolves the accumulated array at the first missing part. |
| `getMainPageDirEntry` | Invokes the callback with a directory entry or null. Its internal Promise is not returned, so the incorrect Promise return tag was removed. |
| `findDirEntriesFromFullTextSearch` | Both the worker-result path and empty-result path return the accumulated directory-entry array: `Promise<Array<DirEntry>>`. |
| `callLibzimWorker` | Resolves `event.data`; both WASM and ASM worker implementations post content/results/suggestions objects, status strings, or numeric article counts. |
| `getDirEntryByPath` | Search miss returns null; search hit adopts `dirEntryByUrlIndex`: `Promise<DirEntry\|null>`. |
| `DirEntry.readData` | Calls `ZIMFile.blob` without metadata mode: plain bytes are Uint8Array, compressed bytes Int8Array, and logged ZSTD data errors can resolve undefined. |
| `ZIMFile.blob` | Adopts plain Uint8Array reads, XZ/ZSTD Int8Array reads, or a `{ptr, size}` metadata object. Compressed metadata requests resolve null; logged ZSTD data errors can resolve undefined. |
| `setListings` | Listing exhaustion returns null directly; asynchronous processing eventually adopts null; its logging catch returns undefined. |
| `readMimetypeMap` | Creates numeric map keys with parsed string values; normal and error paths return a Map: `Promise<Map<number, string>>`. |
| `zstd.readSlice` | Adopts `_readLoop`'s Int8Array/undefined result; initialization failure rejects. |
| `zstd.readSliceSingleThread` | Direct and deferred paths adopt `readSlice`: `Promise<Int8Array\|undefined>`. |
| `zstd._readLoop` | Successful completion returns the allocated Int8Array; its logging catch has no return value and resolves undefined. |

Scope checks independently confirmed that only JSDoc blocks changed in all 10 files and that executable ASTs are unchanged. The minified production bundle and service worker are byte-identical to builds of the base commit with the same dependencies. Automated tests do not validate these documentation types; the review above traces the implementation directly.
