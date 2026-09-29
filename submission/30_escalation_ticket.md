# Escalation ticket

## Ticket 1 — Polygon ego_body của reference phủ nhầm vật trên đường

- **Frame:** `adasind_295948.jpg` (slice B4-center)
- **Ảnh chụp:** `submission/screenshots/esc_295948_ref_ego_polygon.jpg`
- **Expected impact:** reference dùng một tứ giác 4 điểm [0,915,229,1736] làm `ego_body`, trùm lên người che ô hồng
  (L5, cao 92 px), người áo vàng (L4, cao 98 px) và một auto (L3) ở xa trên đường, cùng người đạp xe chở thùng L2.
  Theo R09 các box này thành don't-care: ba vật trong phạm vi biến mất khỏi TP/FN, model được "miễn" khi bỏ sót,
  và mọi nhóm làm frame này đều nhận IGNORE_SCOPE (4 dòng) dù nhãn đúng. Số liệu zone `mid` của frame bị sai lệch.
- **Owner:** `data_ops`
- **Recommendation:** vẽ lại `ego_body` của `295948` theo viền người áo caro ngồi cạnh camera (dưới-trái, khoảng
  [0,1203,222,1745]) giống cách làm ở `270517`; bỏ phần polygon phủ lên đường từ y≈915 tới 1200. Sau khi sửa, chạy
  lại `compare`/`local-quality` cho B4-center. Liên quan: findings r3_diag `L5+M3`, `L4+M5` (action=escalate),
  decision log D4, guideline patch R07b.

## Ticket 2 — Class xe tải nhỏ trong reference

- **Frame:** `adasind_295948.jpg`
- **Ảnh chụp:** `submission/screenshots/esc_295948_minitruck_class.jpg`
- **Expected impact:** xe tải nhỏ thùng hở [455,894,654,1077] được reference gọi `Car`; nhãn nhóm và model YOLO26m
  đều là `Truck`. Class Truck của slice có precision 0,5 và xuất hiện một cặp WRONG_CLASS chỉ vì ca này; ai theo
  đúng R04 cũng bị tính sai.
- **Owner:** `data_ops`
- **Recommendation:** đổi class R1 của `295948` thành `Truck` theo R04 ("xe bán tải nhỏ → Truck"); rà các frame
  khác có xe tải nhỏ/xe ba gác máy 4 bánh để thống nhất. Liên quan: findings r3_diag `L1+M2` (action=escalate), D5.
