---
aliases:
  - Singleton Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `Singleton` đảm bảo một class chỉ có một instance dùng chung và cung cấp điểm truy cập toàn cục tới instance đó.

## Key points
- `Singleton` là creational pattern.
- Nó được dùng cho object mang tính global như config, cache, hoặc service registry đơn giản.
- Constructor được giới hạn để code bên ngoài không tự tạo nhiều instance.
- Class giữ một static instance duy nhất và trả lại instance đó mỗi khi được yêu cầu.
- Một số implementation còn thêm lazy initialization để chỉ tạo instance khi cần.

## Decision rules
- Hữu ích khi một resource thật sự chỉ nên có một bản dùng chung.
- Tránh việc tạo nhiều object cùng vai trò dẫn đến state không đồng bộ.
- Khi domain thật sự cần một instance duy nhất trong toàn app hoặc toàn process.
- Dùng cho config, service bootstrap đơn giản, hoặc cache dùng chung rất rõ ràng.
- Tránh dùng như default cho mọi service.
- Tránh dùng khi dependency có thể truyền rõ ràng qua constructor, vì global access dễ tạo coupling ẩn.
- Dễ tạo global state và làm test khó hơn.
- Có thể che giấu dependency thật của class.
- Nếu quản lý lifecycle kém, singleton dễ trở thành điểm gom quá nhiều trách nhiệm.

## Example
```csharp
public sealed class GameConfig
{
    private static readonly GameConfig _instance = new GameConfig();
    public static GameConfig Instance => _instance;
    public string Language { get; private set; } = "en";
    private GameConfig()
    {
    }
    public void SetLanguage(string language)
    {
        Language = language;
    }
}
```

## Related notes
- [[Definition]]
- [[../Dependency Injection/Dependency Injection|Dependency Injection]]
- [[Summary]]
