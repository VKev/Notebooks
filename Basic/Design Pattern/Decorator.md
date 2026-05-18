---
aliases:
  - Decorator Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `Decorator` thêm hành vi mới vào object bằng cách bọc object đó trong một wrapper cùng contract.

## Key points
- `Decorator` là structural pattern.
- cho phép mở rộng behavior mà không sửa class gốc hoặc tạo quá nhiều subclass.
- Có một contract chung như `INotifier`.
- Decorator cũng implement contract đó nhưng giữ reference tới object gốc.
- Khi nhận lời gọi, decorator có thể thêm logic trước hoặc sau khi chuyển tiếp sang object được bọc.

## Decision rules
- Mở rộng behavior linh hoạt theo tổ hợp.
- Tránh explosion số lượng subclass khi có nhiều tính năng phụ thêm.
- Khi bạn muốn thêm logging, caching, validation, retry, hoặc behavior phụ theo kiểu lớp bọc.
- Khi nhiều tính năng phụ có thể được kết hợp linh hoạt.
- Bỏ qua nếu chỉ có một biến thể cố định và đơn giản.
- Tránh xếp quá nhiều decorator nếu flow trở nên khó theo dõi.
- Nhiều lớp wrapper có thể làm debug khó hơn.
- Stack decorator dài có thể làm behavior thực tế bớt rõ ràng.

## Example
```csharp
public interface INotifier
{
    void Send(string message);
}
public class EmailNotifier : INotifier
{
    public void Send(string message)
    {
        Console.WriteLine($"Email: {message}");
    }
}
public class LoggingNotifierDecorator : INotifier
{
    private readonly INotifier _inner;
    public LoggingNotifierDecorator(INotifier inner)
    {
        _inner = inner;
    }
    public void Send(string message)
    {
        Console.WriteLine("Log before send");
        _inner.Send(message);
    }
}
```

## Related notes
- [[Definition]]
- [[Strategy]]
- [[Summary]]
