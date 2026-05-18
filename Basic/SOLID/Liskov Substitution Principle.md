---
aliases:
  - LSP
note_type: feature
tags:
  - basic
---

## One-line
- `Liskov Substitution Principle` yêu cầu object của class con phải thay thế được class cha mà không làm sai kỳ vọng của code sử dụng.

## What is it
- `LSP` tập trung vào tính thay thế đúng nghĩa, không chỉ là “compile được”.
- Nếu code đang kỳ vọng một contract nhất định, subtype không được phá contract đó.

## How it works
- Base type định nghĩa hành vi hoặc cam kết chung.
- Mọi subtype phải giữ được ý nghĩa của contract đó.
- Nếu subtype phải ném exception bất thường hoặc không làm đúng điều base type hứa, đó thường là dấu hiệu vi phạm `LSP`.

## Why use it
- Giúp inheritance và polymorphism an toàn hơn.
- Giảm bug do subtype có hành vi bất ngờ.
- Làm abstraction đáng tin cậy hơn khi truyền object qua base type hoặc interface.

## When to use it
- Dùng khi bạn thiết kế hierarchy kế thừa hoặc abstraction có nhiều implementation.
- Dùng khi một API chấp nhận base type nhưng runtime có thể nhận nhiều subtype khác nhau.

## When to not use it
- Không nên tạo quan hệ cha con nếu subtype không thực sự giữ được contract của supertype.
- Không nên ép reuse bằng inheritance nếu điều đó làm subtype có hành vi bất thường.

## Limitations
- `LSP` khó thấy nếu chỉ nhìn chữ ký method; bạn phải hiểu contract ngầm của hành vi.
- Vi phạm `LSP` thường xuất hiện muộn, nhất là khi hierarchy lớn dần.

---

## Example code
```csharp
public abstract class Shape
{
    public abstract double GetArea();
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public override double GetArea()
    {
        return Width * Height;
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }

    public override double GetArea()
    {
        return Math.PI * Radius * Radius;
    }
}

public class AreaPrinter
{
    public void Print(Shape shape)
    {
        Console.WriteLine(shape.GetArea());
    }
}
```

---

## Related notes
- [[Definition]]
- [[Interface Segregation Principle]]
- [[Summary]]
