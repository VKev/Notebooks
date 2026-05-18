---
aliases:
  - GC
  - Garbage Collector
note_type: feature
tags:
  - basic
---

## Core idea
- `Garbage Collection` là cơ chế tự động của CLR để thu hồi bộ nhớ heap mà application không còn sử dụng.

## Key points
- Microsoft .NET: `Garbage Collector` là trình quản lý bộ nhớ tự động trong CLR, cấp phát và giải phóng bộ nhớ trên managed heap.
- GC chia managed heap thành ba generation: `Gen 0` cho object mới và ngắn hạn, `Gen 1` làm buffer giữa ngắn hạn và dài hạn, `Gen 2` cho object tồn tại lâu.
- Object lớn hơn `85,000 bytes` được cấp phát trên `Large Object Heap` riêng biệt, thu gom cùng Gen 2 nhưng không compact.
- Khi Gen 0 đầy hoặc hệ thống thiếu bộ nhớ, GC được kích hoạt.
- GC thực hiện ba pha: `Mark` tìm tất cả object còn sống bằng cách duyệt từ root, `Relocate` cập nhật reference tới object sẽ bị dịch chuyển, và `Compact` nén object sống lại gần nhau và giải phóng vùng nhớ chết.
- Root bao gồm static field, local variable trên stack, CPU register, GC handle, và finalize queue.
- Object không thể truy cập từ bất kỳ root nào được coi là rác và bộ nhớ của được thu hồi.
- Object sống sót sau collection được promote lên generation cao hơn: Gen 0 lên Gen 1, Gen 1 lên Gen 2.

## Decision rules
- Tránh gọi `GC.Collect()` thủ công, vì kích hoạt full collection tốn kém và phá vỡ heuristic tối ưu của GC.
- Không dùng finalizer nếu class không giữ unmanaged resource, vì finalizer thêm overhead và delay thu hồi object.
- Không thể kiểm soát chính xác thời điểm GC chạy hay object nào bị thu hồi trước.
- Tự động giải phóng bộ nhớ, tránh memory leak do quên `free` như trong ngôn ngữ unmanaged.
- Đảm bảo memory safety, ngăn truy cập vào object đã bị giải phóng.
- Hệ thống generation giúp tối ưu bằng cách thu gom vùng nhỏ thay vì quét toàn bộ heap mỗi lần.
- GC chạy tự động, đa số trường hợp không cần can thiệp thủ công.
- Implement `IDisposable` và dùng `using` statement khi làm việc với unmanaged resource như file handle, network connection, database connection.

## Example
```csharp
using System;
using System.IO;
public class ResourceHolder : IDisposable
{
    private FileStream _stream;
    private bool _disposed;
    public ResourceHolder(string path)
    {
        _stream = new FileStream(path, FileMode.Open);
    }
    public void Dispose()
    {
        if (!_disposed)
        {
            _stream?.Dispose();
            _disposed = true;
            GC.SuppressFinalize(this); // không cần finalizer nữa
        }
    }
    ~ResourceHolder() // finalizer: backup nếu quên Dispose
    {
        Dispose();
    }
}
public class GCExample
{
    public void Demo()
    {
        using (var holder = new ResourceHolder("data.bin"))
        {
        }
        for (int i = 0; i < 1000; i++)
        {
            string s = i.ToString(); // allocation mỗi lần lặp
        }
    }
}
```

## Related notes
- [[Definition]]
- [[Stack and Heap]]
- [[Boxing and Unboxing]]
- [[Summary]]
