---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.3`: Section này chủ yếu dựa trên manual `6000.3`. Một số khuyến nghị thay đổi theo render pipeline, nên luôn đọc đúng pipeline mà project đang dùng.

## Core keywords
- `Draw Call`: Là lúc CPU bảo GPU vẽ thứ gì đó. Chi phí CPU lớn nhất nằm ở render state updates.
- `SetPass calls`: Là chỉ số rất hữu ích để nhìn lượng chuyển trạng thái render. Unity khuyến nghị kiểm tra trong `Stats`, `Profiler`, hoặc `Frame Debugger`.
- `Render state`: Bao gồm shader, texture, buffer, và các thiết lập cần cho GPU vẽ. Nếu nhiều object dùng cùng render state, Unity tối ưu dễ hơn vì CPU phải cập nhật state ít hơn.

## Render state commands
- `Blend`: Điều khiển cách màu từ fragment shader trộn với render target. Unity `6.3` lưu ý bật blending có thể tắt một số GPU optimization như `Early-Z`.
- `Cull`: Quy định mặt nào của polygon bị loại bỏ. `Cull Back` là mặc định; `Cull Off` hay dùng cho wall hai mặt hoặc một số transparent effect.
- `ZTest`: Quy định điều kiện pass/fail của depth test. Mặc định là `LEqual`.
- `ZWrite`: Quy định có ghi vào depth buffer hay không. Opaque bật; semi-transparent tắt, nhưng khi tắt có thể phải sort ở CPU.
- `Stencil`: Dùng stencil buffer để đánh dấu hoặc lọc pixel. Hay gặp ở mask, portal, outline, hoặc hiệu ứng nhiều pass.
- `ColorMask` và `Offset`: `ColorMask` chặn ghi vào một số kênh màu; `Offset` tạo depth bias để tránh artifact như z-fighting. Đây cũng là render state nên thay đổi của chúng vẫn ảnh hưởng flow chuẩn bị draw call.

## Optimization methods
- `Batching`: Gộp mesh hoặc dữ liệu vẽ để giảm draw call. `Static batching` vẫn hữu ích; `dynamic batching` cần profile kỹ và không còn là lựa chọn mặc định tốt trên hardware hiện đại.
- `GPU Instancing`: Một draw call cho nhiều instance cùng mesh và material. Rất hợp cho object lặp lại nhiều lần như cây hoặc prop.
- `SRP Batcher`: Giảm render state updates trong URP/HDRP. lựa chọn Unity `6.3` khuyến nghị bật trong URP và HDRP.

## Decision rules
- `Need optimize in URP/HDRP`: Ưu tiên kiểm tra `SRP Batcher` trước. Sau đó mới đánh giá `GPU Instancing` hoặc kỹ thuật SRP-native khác theo content cụ thể.
- `Need many identical meshes`: Cân nhắc `GPU Instancing`. Điều kiện quan trọng là mesh và material phải thật sự giống nhau hoặc tương thích.
- `Need optimize many static objects`: Cân nhắc `Static batching`. Nhưng phải chấp nhận trade-off memory và điều kiện tương thích.
- `Need optimize moving small meshes`: Chỉ thử `Dynamic batching` sau khi profile. Không mặc định coi nó là tối ưu trên máy hiện đại.

## Common traps
- `Ít draw call hơn luôn tốt hơn`: Sai. Có lúc kỹ thuật giảm draw call lại tăng CPU hoặc memory ở chỗ khác, nên phải profile.
- `GPU Instancing luôn tốt hơn SRP Batcher`: Sai. Trong URP/HDRP, Unity `6.3` khuyến nghị ưu tiên `SRP Batcher` cho case tổng quát.
- `Dynamic batching luôn nên bật`: Sai. Unity nói rõ kỹ thuật cũ cho low-end cũ, và trên hardware hiện đại có thể còn tệ hơn draw call thường.
- `Material nào cũng batch chung được`: Sai. Khác material instance, shader pass, lightmap, hoặc property quan trọng là có thể làm batch bị vỡ.

## Review questions

### Draw call là gì và vì sao nó làm CPU-bound?
- Vì CPU phải chuẩn bị render state rồi mới gửi lệnh vẽ sang GPU. Khi số draw call quá nhiều, CPU bị bận ở phần chuẩn bị này.

### Render state là gì và vì sao nó quan trọng với draw call?
- Vì mỗi draw call cần một state phù hợp trước khi GPU vẽ. Nếu state thay đổi liên tục giữa các object, CPU phải set lại nhiều lần và chi phí render tăng.

### Khác nhau ngắn gọn giữa `Batching`, `GPU Instancing`, và `SRP Batcher` là gì?
- `Batching` gộp mesh hoặc dữ liệu vẽ, `GPU Instancing` vẽ nhiều instance cùng mesh/material trong một draw call, còn `SRP Batcher` giảm chi phí render state updates trong SRP.

### Trong URP/HDRP nên nghĩ gì trước khi bật `GPU Instancing`?
- Trước hết kiểm tra `SRP Batcher`, vì Unity `6.3` khuyến nghị bật nó cho URP/HDRP trước các lựa chọn cổ điển hơn.

### Khi nào `Dynamic batching` làm chậm hơn?
- Khi chi phí transform vertex trên CPU lớn hơn chi phí của draw call, điều rất dễ xảy ra trên hardware hiện đại.

### Muốn kiểm tra vấn đề draw call thì nhìn ở đâu?
- `Rendering Statistics`, `Profiler` phần Rendering, và `Frame Debugger`.

## Related notes
- [[Draw Call]]
- [[Definition]]
- [[Render State]]
- [[Batching]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[../Unity|Unity]]
