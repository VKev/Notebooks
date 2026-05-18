---
aliases:
  - Dynamic Dispatch
note_type: feature
tags:
  - basic
---

## Core idea
- `Polymorphism` cho phép nhiều object khác nhau cùng dùng chung một contract nhưng phản hồi theo implementation riêng của chúng.

## Key points
- `Polymorphism` nghĩa là “nhiều hình thái”.
- Bạn có thể gọi cùng một method qua interface hoặc base type, nhưng kết quả thực thi phụ thuộc object thật ở runtime.
- Bạn định nghĩa contract chung như `INotifier`.
- Nhiều class như `EmailNotifier` và `SmsNotifier` cùng implement contract đó.
- Code bên ngoài chỉ làm việc với `INotifier`, còn implementation thực tế được chọn linh hoạt.

## Decision rules
- Làm code mở rộng dễ hơn khi thêm loại implementation mới.
- Giảm `if-else` dựa trên type cụ thể.
- Giúp nơi sử dụng code tập trung vào hành vi mong muốn thay vì class cụ thể.
- Khi nhiều object cùng chia sẻ một vai trò nhưng khác cách thực hiện.
- Khi muốn thay behavior linh hoạt theo cấu hình, runtime, hoặc dependency.
- Không cần dùng nếu chỉ có một implementation cố định và không có áp lực thay đổi.
- Tránh ép polymorphism vào bài toán đơn giản nếu nó làm flow khó đọc hơn.
- Nếu contract thiết kế kém, polymorphism sẽ khó dùng đúng.
- Debug đôi khi khó hơn vì hành vi thực tế chỉ rõ khi runtime chọn implementation.

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
public class SmsNotifier : INotifier
{
    public void Send(string message)
    {
        Console.WriteLine($"SMS: {message}");
    }
}
public class NotificationService
{
    public void Notify(INotifier notifier, string message)
    {
        notifier.Send(message);
    }
}
```

## Related notes
- [[Definition]]
- [[Abstraction]]
- [[Inheritance]]
- [[Summary]]
