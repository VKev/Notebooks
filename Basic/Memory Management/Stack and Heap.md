---
aliases:
  - Stack vs Heap
note_type: feature
tags:
  - basic
---

## Core idea
- `Stack` và `Heap` là hai vùng bộ nhớ chính trong .NET, trong đó stack chứa data ngắn hạn theo LIFO còn heap chứa object do Garbage Collector quản lý.

## Key points
- `Stack` là vùng nhớ hoạt động theo cơ chế Last In First Out, lưu local variable, method parameter, và return address.
- `Heap` là vùng nhớ lớn hơn và linh hoạt hơn, nơi tất cả reference type object được cấp phát và do `Garbage Collector` quản lý.
- Mỗi thread có stack riêng nên local variable trên stack là thread-safe tự nhiên, trong khi tất cả thread dùng chung một managed heap.
- Khi method được gọi, CLR đẩy một stack frame lên stack chứa local variable và parameter của method đó.
- Khi method kết thúc, stack frame tự động bị pop ra và bộ nhớ được giải phóng ngay lập tức.
- Value type khai báo trong local scope như `int`, `float`, `bool`, `struct` được lưu trực tiếp trên stack.
- Reference type như `class`, `interface`, `string`, `delegate` luôn được cấp phát trên heap, nhưng biến reference trỏ tới chúng nằm trên stack.
- Ngoại lệ quan trọng: value type là field của class sẽ nằm trên heap cùng với object chứa nó, không phải trên stack.

## Decision rules
- Dùng local variable thay vì field cấp class khi data chỉ cần trong phạm vi một method.
- Không cố ép mọi thứ lên stack bằng cách dùng struct quá lớn, vì copy struct lớn trên stack cũng tốn chi phí.
- Không lo lắng quá mức về stack vs heap cho code không nằm trong hot path, vì overhead không đáng kể.
- Không thể kiểm soát chính xác thời điểm GC thu hồi object trên heap.
- Hiểu stack và heap giúp dự đoán được chi phí bộ nhớ và hiệu năng của code.
- Stack allocation nhanh hơn heap allocation vì chỉ cần dịch con trỏ stack, không cần GC quản lý.
- Biết data nằm ở đâu giúp tránh boxing không cần thiết và giảm GC pressure.
- Ưu tiên `struct` thay vì `class` khi data nhỏ, ngắn hạn, và không cần polymorphism để tận dụng stack allocation.

## Example
```csharp
public struct Position   // value type
{
    public float X;
    public float Y;
}
public class Enemy       // reference type
{
    public string Name;
    public Position Pos; // struct nằm trên heap cùng object Enemy
}
public class Example
{
    public void Run()
    {
        int hp = 100;              // stack: value type, local variable
        Position pos;              // stack: struct trong local scope
        pos.X = 1f;
        pos.Y = 2f;
        Enemy enemy = new Enemy(); // heap: object Enemy
        enemy.Name = "Goblin";     // heap: string là reference type
        enemy.Pos = pos;           // copy value lên heap vì Pos là field của class
    }
}
```

## Related notes
- [[Definition]]
- [[Boxing and Unboxing]]
- [[Garbage Collection]]
- [[Summary]]
