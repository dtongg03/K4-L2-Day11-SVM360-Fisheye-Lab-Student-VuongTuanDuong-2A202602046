# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 11 | 11 | 1 | 1 | 7 | 4 |
| mid | 4 | 5 | 1 | 0 | 1 | 1 |
| edge | 3 | 3 | 0 | 0 | 0 | 0 |

## Findings action=rework
- adasind_270517.jpg L8 SPURIOUS: đã sửa
- adasind_271039.jpg L2 SPURIOUS: chưa sửa
- adasind_271039.jpg L16 SPURIOUS: chưa sửa
- adasind_295948.jpg L6 SPURIOUS: đã sửa
- adasind_295948.jpg R3 MISSING: đã sửa

## Đọc kết quả (C · Lê Ngọc Nam; B · Vương Tuấn Dương kiểm lại)

Bản khóa P2 `r1_craft` (D9C5-98BB) giữ nguyên để so; v2 là export CVAT sau sửa, khóa 1C9E-1D1E. Bảng số trên do
`python3 lab11.py rework` sinh, không sửa tay.

- **mid · missing 1 → 0, matched 4 → 5:** người đạp xe quấn khăn caro `295948` (R3) nay có box Bike truncated
  [574,606,1080,1715] thay cho polygon ego_body_2 (D1). Đây là cải thiện thật, gắn với lỗi P0 B bắt được ở QA mù.
- **center · spurious 7 → 4:** bỏ `270517` L8 và `295948` L6 (D3), gộp `271039` L2+L16 thành một ThreeWheeler (D2)
  nên bớt một box. Bốn box spurious còn lại đều là quyết định giữ có lý do: người áo sáng sau người áo đen (L+M thấy,
  R thiếu, E0), xe máy sau xe đỏ (E0), người sau van (E5) và auto đã gộp.
- **Vì sao L2/L16 vẫn "chưa sửa":** công cụ so với reference, mà reference không có box cho chiếc auto này (chỉ có
  unreadable ở bên trái). Rework đã gộp đúng hai box thành một vật (model M11 cũng thấy một xe), nhưng box gộp vẫn
  bị tính SPURIOUS so với R. Không xóa box chỉ để số giống reference; ghi vào D2 là giới hạn của reference.
- **center · missing 1 → 1:** là R1 Car ↔ L1 Truck (xe tải nhỏ, `295948`): L giữ Truck theo R04 và đã escalate sửa
  reference (D5), nên không kỳ vọng số này đổi.
- **edge không đổi (3/3 matched):** không có ca rework ở edge.
