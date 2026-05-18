---
aliases:
  - DI by Constructor
note_type: feature
tags:
  - basic
---

## Core idea
- `Constructor Injection` là cách inject dependency phổ biến, trong đó dependency được truyền vào ngay khi object được tạo.

## Key points
- một hình thức của `Dependency Injection`.
- Object nhận dependency qua constructor thay vì `new` trực tiếp bên trong class.
- Class khai báo dependency cần dùng trong constructor.
- Composition root hoặc DI container sẽ tạo dependency phù hợp rồi truyền vào.
- Class sau đó chỉ làm việc với abstraction hoặc contract đã nhận.

## Decision rules
- Khi dependency là bắt buộc để class hoạt động đúng.
- Dùng cho service, application layer, hoặc domain logic có dependency rõ ràng.
- Không cần dùng nếu class thật sự không có dependency nào đáng kể.
- Tránh nhét quá nhiều dependency vào constructor vì đó là dấu hiệu class làm quá nhiều việc.
- Nếu lạm dụng abstraction quá sớm, hệ thống có thể phức tạp hơn mức cần thiết.
- Làm dependency trở nên rõ ràng ngay từ lúc đọc constructor.
- Giúp object luôn ở trạng thái hợp lệ sau khi được tạo.
- Giúp viết test dễ hơn vì có thể truyền mock hoặc fake implementation.

## Example
```csharp
public interface ILogger
{
    void Log(string message);
}
public class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}
public class UserService
{
    private readonly ILogger _logger;
    public UserService(ILogger logger)
    {
        _logger = logger;
    }
    public void CreateUser(string name)
    {
        _logger.Log($"Create user: {name}");
    }
}
```

## Related notes
- [[Definition]]
- [[Summary]]
- [[../SOLID/SOLID|SOLID]]
