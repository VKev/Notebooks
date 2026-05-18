---
aliases:
  - Command Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `Command` đóng gói một request thành object để có thể queue, log, undo, hoặc truyền đi như dữ liệu.

## Key points
- `Command` là behavioral pattern.
- Nó tách nơi phát yêu cầu khỏi nơi thực thi logic cụ thể.
- Bạn định nghĩa interface như `ICommand` với method `Execute`.
- Mỗi command object chứa dữ liệu và receiver cần thiết để hoàn thành tác vụ.
- Invoker chỉ cần biết gọi `Execute`, không cần biết chi tiết bên trong.

## Decision rules
- Giảm coupling giữa UI, input, menu, scheduler với business action thật.
- Hữu ích cho undo/redo, macro, queue, hoặc lịch sử thao tác.
- Khi action cần được trì hoãn, queue, log, hoặc thực thi độc lập với nơi gọi.
- Khi bạn muốn biểu diễn thao tác như dữ liệu hoặc object riêng.
- Bỏ qua nếu chỉ có một lời gọi trực tiếp rất đơn giản.
- Tránh tạo command cho mọi method nhỏ nếu không có giá trị tách biệt nào.
- Tăng số lượng class.
- Nếu action quá đơn giản, command pattern có thể bị nặng tay.

## Example
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

## Related notes
- [[Definition]]
- [[Observer]]
- [[Summary]]
