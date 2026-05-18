---
aliases:
  - DIP
note_type: feature
tags:
  - basic
---

## Core idea
- `Dependency Inversion Principle` yêu cầu code cấp cao phụ thuộc vào abstraction thay vì implementation cụ thể.

## Key points
- `DIP` là nguyên tắc thiết kế, không phải chính `Dependency Injection`.
- Ý chính là business logic không nên biết quá nhiều về chi tiết thấp tầng như database, email provider, hay API cụ thể.
- Module cấp cao định nghĩa hoặc phụ thuộc vào contract như `IMessageSender`.
- Implementation thấp tầng như `EmailSender` hoặc `SmsSender` implement contract đó.
- Dependency được truyền từ ngoài vào thay vì tự `new` bên trong.

## Decision rules
- Dùng ở những boundary quan trọng như repository, payment gateway, notification, storage.
- Khi muốn business logic độc lập hơn với framework hoặc vendor cụ thể.
- Tránh thêm abstraction cho những helper cực nhỏ, cố định, và không tạo variation point nào thực tế.
- Tránh hiểu nhầm rằng mọi dependency đều phải đi qua interface.
- Giảm coupling giữa domain logic và infrastructure.
- Dễ test hơn vì có thể thay bằng fake hoặc mock.
- Dễ thay implementation mà ít ảnh hưởng code cấp cao.
- Lạm dụng abstraction có thể làm code nhiều lớp hơn cần thiết.

## Example
```csharp
public interface IMessageSender
{
    void Send(string message);
}
public class EmailSender : IMessageSender
{
    public void Send(string message)
    {
        Console.WriteLine($"Send email: {message}");
    }
}
public class NotificationService
{
    private readonly IMessageSender _messageSender;
    public NotificationService(IMessageSender messageSender)
    {
        _messageSender = messageSender;
    }
    public void Notify(string message)
    {
        _messageSender.Send(message);
    }
}
```

## Related notes
- [[Definition]]
- [[../Dependency Injection/Dependency Injection|Dependency Injection]]
- [[Summary]]
