---
aliases:
  - Command Pattern
note_type: feature
tags:
  - basic
---

## One-line
- `Command` đóng gói một request thành object để có thể queue, log, undo, hoặc truyền đi như dữ liệu.

## What is it
- `Command` là behavioral pattern.
- Nó tách nơi phát yêu cầu khỏi nơi thực thi logic cụ thể.

## How it works
- Bạn định nghĩa interface như `ICommand` với method `Execute`.
- Mỗi command object chứa dữ liệu và receiver cần thiết để hoàn thành tác vụ.
- Invoker chỉ cần biết gọi `Execute`, không cần biết chi tiết bên trong.

## Why use it
- Giảm coupling giữa UI, input, menu, scheduler với business action thật.
- Hữu ích cho undo/redo, macro, queue, hoặc lịch sử thao tác.

## When to use it
- Dùng khi action cần được trì hoãn, queue, log, hoặc thực thi độc lập với nơi gọi.
- Dùng khi bạn muốn biểu diễn thao tác như dữ liệu hoặc object riêng.

## When to not use it
- Không cần nếu chỉ có một lời gọi trực tiếp rất đơn giản.
- Không nên tạo command cho mọi method nhỏ nếu không có giá trị tách biệt nào.

## Limitations
- Tăng số lượng class.
- Nếu action quá đơn giản, command pattern có thể bị nặng tay.

---

## Example code
```csharp
public interface ICommand
{
    void Execute();
}

public class Light
{
    public void TurnOn()
    {
        Console.WriteLine("Light on");
    }
}

public class TurnOnLightCommand : ICommand
{
    private readonly Light _light;

    public TurnOnLightCommand(Light light)
    {
        _light = light;
    }

    public void Execute()
    {
        _light.TurnOn();
    }
}
```

---

## Related notes
- [[Definition]]
- [[Observer]]
- [[Summary]]
