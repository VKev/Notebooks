---
aliases:
  - Interface vs Abstract Class
  - Abstract Class
note_type: feature
tags:
  - basic
---

## One-line
- `Interface` mô tả capability hoặc contract, còn `abstract class` là base class dùng khi các type liên quan cần chia sẻ state hoặc implementation.

## What is it
- `Interface` định nghĩa contract gồm method, property, event, hoặc indexer mà class hoặc struct phải implement.
- `Abstract class` là class không được instantiate trực tiếp và có thể chứa cả member đã implement lẫn abstract member bắt buộc class con override.
- Cả hai đều hỗ trợ abstraction, nhưng áp lực thiết kế khác nhau.

## How it works
- Một class C# chỉ inherit một base class, nhưng có thể implement nhiều interface.
- Interface không có instance field hoặc instance constructor.
- Abstract class có thể có field, constructor, protected member, virtual method, và shared behavior.
- Interface member không có implementation truyền thống thường được xem là contract public; abstract class có thể kiểm soát phần public/protected tốt hơn.

## Why use it
- Interface giúp code phụ thuộc vào capability thay vì concrete class.
- Abstract class giúp gom behavior chung của một hierarchy có quan hệ chặt.
- Cả hai đều giúp test, mock, và thay implementation dễ hơn nếu dependency được truyền rõ ràng.

## When to use it
- Dùng interface khi nhiều type không cùng hierarchy cần cùng capability, ví dụ `IDamageable`, `ISaveable`, `ILogger`.
- Dùng abstract class khi các class con thực sự cùng một họ và cần reuse field, constructor, hoặc protected helper.
- Trong Unity, interface thường hợp để tách gameplay contract; abstract `MonoBehaviour` chỉ nên dùng khi hierarchy thật sự ổn định.

## When to not use it
- Không tạo interface chỉ vì mỗi class có một implementation duy nhất và chưa có nhu cầu thay thế.
- Không dùng abstract class để ép mọi thứ vào một inheritance tree nếu composition đơn giản hơn.

## Limitations
- Interface quá nhỏ và quá nhiều có thể làm code phân mảnh.
- Abstract class tạo coupling theo inheritance, khó thay đổi hơn khi hierarchy sai.
- Default interface member là tính năng nâng cao, không nên dùng để thay thế thiết kế API rõ ràng trong codebase nhỏ.

---

## Example code
```csharp
public interface IDamageable
{
    void TakeDamage(int amount);
}

public abstract class Enemy : IDamageable
{
    protected int health;

    protected Enemy(int health)
    {
        this.health = health;
    }

    public void TakeDamage(int amount)
    {
        health -= amount;
        if (health <= 0)
            Die();
    }

    protected abstract void Die();
}
```

---

## Related notes
- [[Definition]]
- [[../OOP/Abstraction|Abstraction]]
- [[../OOP/Inheritance|Inheritance]]
- [[../OOP/Polymorphism|Polymorphism]]
- [[../SOLID/Dependency Inversion Principle|Dependency Inversion Principle]]
- [[Summary]]
