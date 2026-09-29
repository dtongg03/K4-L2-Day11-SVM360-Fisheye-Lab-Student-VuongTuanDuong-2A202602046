# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | BOX_GEOMETRY | 1 |
| center | B4 | IGNORE_SCOPE | 1 |
| center | B4 | MISSING | 5 |
| center | B4 | SPURIOUS | 20 |
| center | B4 | WRONG_CLASS | 1 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B4 | BOX_GEOMETRY | 1 |
| edge | B4 | MISSING | 1 |
| edge | B4 | SPURIOUS | 1 |
| mid | B4 | IGNORE_SCOPE | 6 |
| mid | B4 | MISSING | 3 |
| mid | B4 | SPURIOUS | 6 |
| mid | B4 | WRONG_CLASS | 1 |
| mid | C0 | ATTRIBUTE | 1 |
| unknown | B4 | IGNORE_SCOPE | 2 |
| unknown | B4 | MISSING | 2 |

## Top defects
- SPURIOUS: 28 (ví dụ frame adasind_019560.jpg)
- MISSING: 11 (ví dụ frame adasind_270517.jpg)
- IGNORE_SCOPE: 9 (ví dụ frame adasind_295948.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

Lỗi nổi bật nhất: **SPURIOUS (28)**, tập trung ở `center` slice B4 (20). Đọc kỹ các dòng findings thì con số này gộp
ba nguồn khác nhau, không phải một kiểu lỗi của người vẽ:

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:
  1. **E4_model_domain (phần lớn M_only ở center):** 6/6 auto trong slice bị YOLO26m gọi Car/Truck (vd.
     `270517` M6+M9 cùng một auto L5, `271039` M11 phủ auto L2+L16), và model box cả người ngồi trong xe
     (`270517` M5, M10, M11 trong auto L1), trái R03. Đủ nhiều ca cùng mẫu nên gọi là lỗi miền model, không phải nhãn.
  2. **E1_annotator_error (L_only ít nhưng có thật):** `270517` L8 và `295948` L6 là box thiếu căn cứ trên vật mờ;
     `271039` L2+L16 cắt một auto thành hai box. Cả ba đều do B bắt được ở QA mù trước khi mở reference.
  3. **E0_reference_defect:** `271039` L6+M8 (người sau người áo đen) và L9 (xe máy sau xe đỏ) thấy rõ trên ảnh,
     model cũng thấy L6, nhưng reference không có → bị tính SPURIOUS dù L đúng.
  Lỗi nguy hiểm nhất dù chỉ 1 ca là **IGNORE_SCOPE/MISSING P0** ở `295948` R3: A đưa người đạp xe vào `ego_body`
  (E1, R07), làm một vật trong phạm vi biến mất khỏi mọi phép đo.
- Cách sửa và ai nhận việc (`owner`): `annotator` (A) đã rework v2: bỏ L8, L6, gộp L2+L16, thay ego_body_2 bằng
  Bike → mid missing 1→0, center spurious 7→4 (`rework/delta.md`). `guideline`: thêm luật phân biệt người trên xe
  ego với người đi sát bên (`20_guideline_patch.md`, v1.1.0). `data_ops`: sửa polygon ego_body và class xe tải
  nhỏ trong reference `295948` (`30_escalation_ticket.md`). `ai_team`: không dùng M_only của YOLO26m làm tín hiệu
  thiếu nhãn cho ThreeWheeler cho tới khi có class này.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/qa_295948_ego_vs_bike.jpg`,
  `qa_270517_no_rider_ahead.jpg` (R07/R03, findings r2_qa + r3_diag `ego_body_2`/`R3`); `qa_270517_L8.jpg`,
  `qa_295948_L6.jpg` (R01, D3); `qa_271039_L14_L2_L16.jpg` (R02, D2); `esc_295948_ref_ego_polygon.jpg` (R09, D4);
  `esc_295948_minitruck_class.jpg` (R04, D5). Decision log D1–D8.
