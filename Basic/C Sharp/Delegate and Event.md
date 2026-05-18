---
aliases:
  - Delegates
  - Events
  - C# Delegate
  - C# Event
note_type: feature
tags:
  - basic
---

## One-line
- `Delegate` là type-safe reference tới method, còn `event` là cơ chế publish/subscribe được xây trên delegate.

## What is it
- `Delegate` đại diện cho method có parameter list và return type tương thích.
- `Event` cho phép một object broadcast rằng có chuyện đã xảy ra, và các object khác subscribe để nhận thông báo.
- Event handler về bản chất là method được gọi thông qua delegate.

## How it works
- Delegate có thể trỏ tới static method, instance method, anonymous method, hoặc lambda expression có signature phù hợp.
- Delegate có thể multicast, tức invocation list có nhiều method được gọi theo thứ tự.
- `event` giới hạn code bên ngoài class chủ yếu ở thao tác subscribe `+=` và unsubscribe `-=`, tránh việc code ngoài tự gán lại toàn bộ delegate.
- Khi raise event, thường dùng `EventName?.Invoke(sender, args)` để tránh lỗi khi chưa có subscriber.

## Why use it
- Tách event source khỏi event listener.
- Cho phép callback linh hoạt mà class phát event không cần biết implementation cụ thể của listener.
- Phù hợp cho UI, gameplay event, notification, observer-style flow, và async callback.

## When to use it
- Dùng `delegate` khi cần truyền behavior như parameter hoặc callback.
- Dùng `event` khi một object cần thông báo một sự kiện cho nhiều subscriber.
- Dùng `Action`, `Func`, hoặc `EventHandler<TEventArgs>` khi không cần khai báo delegate type riêng.

## When to not use it
- Không dùng event cho request-response cần return value rõ ràng; gọi method hoặc service interface thường dễ hiểu hơn.
- Không dùng delegate/event để che giấu flow quá phức tạp; nếu thứ tự xử lý quan trọng, orchestration rõ ràng sẽ dễ debug hơn.

## Limitations
- Subscriber tạo coupling tới event source và cần unsubscribe đúng lúc nếu lifecycle không trùng nhau.
- Với multicast delegate, exception trong một handler có thể làm các handler sau không được gọi nếu không tự bắt lỗi.
- Lambda inline khó unsubscribe nếu không giữ lại reference tới handler đã subscribe.

---

## Example code
```csharp
using System;

public class Health
{
    public event Action<int>? Damaged;

    public void TakeDamage(int amount)
    {
        Damaged?.Invoke(amount);
    }
}

public class DamageLog
{
    public void Attach(Health health)
    {
        health.Damaged += OnDamaged;
    }

    public void Detach(Health health)
    {
        health.Damaged -= OnDamaged;
    }

    private void OnDamaged(int amount)
    {
        Console.WriteLine($"Damage: {amount}");
    }
}
```

---

## Related notes
- [[Definition]]
- [[../Design Pattern/Observer|Observer]]
- [[../Dependency Injection/Dependency Injection|Dependency Injection]]
- [[Summary]]
