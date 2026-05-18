---
aliases:
  - LSP
note_type: feature
tags:
  - basic
---

## Core idea
- `Liskov Substitution Principle` yêu cầu object của class con phải thay thế được class cha mà không làm sai kỳ vọng của code sử dụng.

## Key points
- `LSP` tập trung vào tính thay thế đúng nghĩa, không chỉ là “compile được”.
- Nếu code đang kỳ vọng một contract nhất định, subtype không được phá contract đó.
- Base type định nghĩa hành vi hoặc cam kết chung.
- Mọi subtype phải giữ được ý nghĩa của contract đó.
- Nếu subtype phải ném exception bất hoặc không làm đúng điều base type hứa, đó là dấu hiệu vi phạm `LSP`.

## Decision rules
- Giúp inheritance và polymorphism an toàn hơn.
- Giảm bug do subtype có hành vi bất ngờ.
- Làm abstraction đáng tin cậy hơn khi truyền object qua base type hoặc interface.
- Khi bạn thiết kế hierarchy kế thừa hoặc abstraction có nhiều implementation.
- Khi một API chấp nhận base type nhưng runtime có thể nhận nhiều subtype khác nhau.
- Tránh tạo quan hệ cha con nếu subtype không thực sự giữ được contract của supertype.
- Tránh ép reuse bằng inheritance nếu điều đó làm subtype có hành vi bất thường.
- `LSP` khó thấy nếu chỉ nhìn chữ ký method; bạn phải hiểu contract ngầm của hành vi.
- Vi phạm `LSP` xuất hiện muộn, nhất là khi hierarchy lớn dần.

## Example
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

## Related notes
- [[Definition]]
- [[Interface Segregation Principle]]
- [[Summary]]
