---
aliases:
  - State Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `State` tách hành vi theo từng trạng thái ra thành các object riêng, giúp object đổi cách xử lý khi state bên trong thay đổi.

## Key points
- `State` là behavioral pattern.
- Nó đặc biệt hữu ích khi object có nhiều trạng thái và mỗi trạng thái có rule xử lý khác nhau.
- Context giữ một reference tới state hiện tại.
- Mỗi state implement cùng một contract nhưng xử lý theo logic riêng.
- Khi state đổi, context thay object state hiện tại thay vì dùng chuỗi `if-else` lớn.

## Decision rules
- Làm code rõ hơn khi behavior phụ thuộc mạnh vào trạng thái.
- Giảm `if-else` hoặc `switch` phình to.
- Dễ thêm trạng thái mới mà ít ảnh hưởng trạng thái cũ.
- Dùng cho workflow, AI, player state, order state, hoặc UI state phức tạp.
- Khi một object đổi hành vi rõ rệt theo trạng thái hiện tại.
- Bỏ qua nếu số state ít và logic chuyển state rất đơn giản.
- Tránh tạo state object nếu một enum và vài nhánh rõ ràng là đủ.
- Tăng số lượng class.
- Nếu model state chưa ổn định, việc tách thành nhiều state object có thể hơi nặng.

## Example
```csharp
public interface IOrderState
{
    void Handle(OrderContext context);
}
public class PendingState : IOrderState
{
    public void Handle(OrderContext context)
    {
        Console.WriteLine("Order is pending");
        context.State = new PaidState();
    }
}
public class PaidState : IOrderState
{
    public void Handle(OrderContext context)
    {
        Console.WriteLine("Order is paid");
    }
}
public class OrderContext
{
    public IOrderState State { get; set; } = new PendingState();
    public void Process()
    {
        State.Handle(this);
    }
}
```

## Related notes
- [[Definition]]
- [[Strategy]]
- [[Summary]]
