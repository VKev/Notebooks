---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---


## Core keywords
- `Asynchronous`
  Tách `waiting` khỏi `blocking`.
  Không tự động có nghĩa là chạy song song trên nhiều thread.
- `Main Thread`
  Đây là nơi phần lớn Unity API phải được gọi.
  Chặn thread này sẽ làm game đứng hoặc khựng frame.
- `Continuation`
  Đây là phần code chạy tiếp sau khi việc chờ đã hoàn thành.
  Chỗ resume quan trọng không kém bản thân việc chờ.

## Tool choices
- `Coroutine`
  Phù hợp với flow theo frame, thời gian, hoặc điều kiện gameplay.
  Dễ đọc khi logic có dạng làm A -> chờ -> làm B.
- `Async/Await`
  Phù hợp với `Task`, `I/O`, và flow cần kết quả trả về.
  Mạnh hơn khi cần xử lý lỗi và ghép nhiều bước async.
- `Awaitable`
  Là async type gốc của Unity trong `6.3`, hiểu main thread và update loop tốt hơn.
  Hợp khi bạn muốn dùng `await` nhưng vẫn bám sát execution model của Unity.

## Popular Unity features
- `Awaitable`
  Có các API nổi bật như `NextFrameAsync`, `WaitForSecondsAsync`, `FixedUpdateAsync`, `EndOfFrameAsync`, `MainThreadAsync`, và `BackgroundThreadAsync`.
  Đây là feature trung tâm nếu bạn muốn async flow theo kiểu Unity-native.
- `AsyncOperation`
  Thường xuất hiện ở scene loading, resource loading, và các operation do engine xử lý.
  Có thể theo dõi qua `progress`, `isDone`, event `completed`, hoặc `await` ở các API phù hợp.
- `AwaitableCompletionSource`
  Dùng để tự raise completion từ user code.
  Hợp để bridge UI, callback, hoặc event-based logic sang flow `await`.

## Decision rules
- `Need frame timing`
  Ưu tiên `Coroutine`.
  Đây là lựa chọn tự nhiên khi flow bám theo player loop.
- `Need task result`
  Ưu tiên `async/await`.
  Đặc biệt hợp với network, file, hoặc service call.
- `Need Unity-native await`
  Ưu tiên `Awaitable`.
  Đặc biệt hợp khi bạn cần frame timing, thread switching, hoặc chờ Unity async API bằng cú pháp `await`.
- `Need background work`
  Không dùng `Coroutine` để giả lập.
  Phải chọn đúng API cho background execution như `Awaitable.BackgroundThreadAsync()` hoặc công cụ phù hợp khác.

## Common traps
- `Async = thread`
  Sai. `async` là flow model, không phải thread model.
  Có `await` không có nghĩa là code đã chạy ở background thread.
- `Coroutine = parallel`
  Sai. `Coroutine` vẫn chạy trên `main thread`.
  Nó chỉ chia flow theo các điểm `yield`.

## Interview questions and answers
### Coroutine có tạo thread mới không?
- Không. `Coroutine` chỉ là cơ chế tạm dừng và resume logic trên `main thread`.

### Khi nào nên ưu tiên `async/await` hơn `Coroutine`?
- Khi bạn làm việc với API trả về `Task`, cần kết quả trả về, hoặc cần flow `I/O` dễ compose và bắt lỗi.

### `Awaitable` khác `Task` ở điểm nào dễ nhớ nhất?
- Theo Unity `6.3`, `Awaitable` ít allocation hơn, hiểu main thread và update loop tốt hơn, nhưng không an toàn để `await` nhiều lần trên cùng một instance.

### Khi nào nên dùng `AwaitableCompletionSource`?
- Khi bạn muốn biến một event hoặc tương tác UI thành một điểm completion có thể `await` được.

### Vì sao async code vẫn có thể làm game khựng?
- Vì phần nặng vẫn có thể đang chạy trên `main thread`, hoặc continuation quay về `main thread` rồi làm quá nhiều việc trong một frame.

## Related notes
- [[Definition]]
- [[Coroutine]]
- [[AsyncAwait]]
- [[Awaitable]]
- [[AsyncOperation]]
- [[AwaitableCompletionSource]]
- [[../Unity|Unity]]
