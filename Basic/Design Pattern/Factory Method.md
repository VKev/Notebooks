---
aliases:
  - Factory
note_type: feature
tags:
  - basic
---

## Core idea
- `Factory Method` tách logic tạo object ra khỏi nơi sử dụng, giúp code chọn implementation phù hợp mà không phải `new` trực tiếp ở nhiều nơi.

## Key points
- `Factory Method` là creational pattern.
- đóng gói logic khởi tạo object vào một method hoặc class chuyên trách.
- Nơi sử dụng chỉ yêu cầu một abstraction hoặc một loại sản phẩm.
- Factory quyết định tạo implementation cụ thể nào dựa trên input hoặc context.
- Khi có loại mới, bạn mở rộng factory thay vì sửa khắp nơi gọi `new`.

## Decision rules
- Khi việc tạo object có điều kiện hoặc cần nhiều bước khởi tạo.
- Khi cùng một contract có nhiều implementation khác nhau.
- Không cần factory nếu object đơn giản và chỉ có một cách tạo rõ ràng.
- Tránh tạo factory riêng chỉ để bọc một lệnh `new` không có logic gì thêm.
- Nếu lạm dụng, code có thể vòng vo hơn cần thiết.
- Gom logic khởi tạo vào một chỗ.
- Giảm coupling giữa nơi sử dụng và implementation cụ thể.
- Làm code dễ thay đổi khi cách tạo object phức tạp dần.

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
public static class NotifierFactory
{
    public static INotifier Create(string channel)
    {
        return channel switch
        {
            "email" => new EmailNotifier(),
            "sms" => new SmsNotifier(),
            _ => throw new NotSupportedException(channel)
        };
    }
}
```

## Related notes
- [[Definition]]
- [[Strategy]]
- [[Summary]]
