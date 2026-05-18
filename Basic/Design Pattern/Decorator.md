---
aliases:
  - Decorator Pattern
note_type: feature
tags:
  - basic
---

## One-line
- `Decorator` thêm hành vi mới vào object bằng cách bọc object đó trong một wrapper cùng contract.

## What is it
- `Decorator` là structural pattern.
- Nó cho phép mở rộng behavior mà không sửa class gốc hoặc tạo quá nhiều subclass.

## How it works
- Có một contract chung như `INotifier`.
- Decorator cũng implement contract đó nhưng giữ reference tới object gốc.
- Khi nhận lời gọi, decorator có thể thêm logic trước hoặc sau khi chuyển tiếp sang object được bọc.

## Why use it
- Mở rộng behavior linh hoạt theo tổ hợp.
- Tránh explosion số lượng subclass khi có nhiều tính năng phụ thêm.

## When to use it
- Dùng khi bạn muốn thêm logging, caching, validation, retry, hoặc behavior phụ theo kiểu lớp bọc.
- Dùng khi nhiều tính năng phụ có thể được kết hợp linh hoạt.

## When to not use it
- Không cần nếu chỉ có một biến thể cố định và rất đơn giản.
- Không nên xếp quá nhiều decorator nếu flow trở nên khó theo dõi.

## Limitations
- Nhiều lớp wrapper có thể làm debug khó hơn.
- Stack decorator dài có thể làm behavior thực tế bớt rõ ràng.

---

## Example code
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

---

## Related notes
- [[Definition]]
- [[Strategy]]
- [[Summary]]
