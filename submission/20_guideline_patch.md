# Guideline patch

- **Rule mới đề xuất:** **R07b — Người trên xe ego và người đi sát bên.** Chỉ đưa vào `ignore_region`
  `reason=ego_body` phần thân xe ego **và** người đang ngồi trên chính xe gắn camera (tài xế, hành khách). Một người
  gần camera được coi là thuộc xe ego khi thỏa **cả hai**: (a) không có phương tiện riêng nhìn thấy được (bánh xe,
  pedal, tay lái riêng); (b) xuất hiện ở cùng vị trí trong các frame khác của cùng chuyến. Nếu thấy bánh xe/pedal
  riêng hoặc frame cùng chuyến không có người đó ở vị trí ấy, người đó là vật trong phạm vi: vẽ box theo R03
  (người + xe hai bánh = một `Bike`), đặt `truncated` nếu bị vòng kính/khung cắt. Polygon `ego_body` bám viền thân
  xe/người ego, không dùng tứ giác thô trùm lên vùng đường phía sau.
  - *Ví dụ đúng:* `adasind_270517.jpg` và `adasind_295948.jpg`, người áo caro ngồi dưới-trái cạnh camera → `ego_body`.
  - *Ví dụ sai (ca của nhóm):* `adasind_295948.jpg`, người quấn khăn caro bên phải có bánh xe đạp + pedal riêng và
    không xuất hiện ở `270517` → phải là `Bike` [574,606,1080,1715], không phải `ego_body` (finding r3_diag `R3`, D1).
  - *Ví dụ polygon quá rộng:* reference `295948` dùng tứ giác [0,915,229,1736] trùm người đi bộ L4/L5 ở xa (D4).
- **Áp dụng cho:** `ignore_region` reason `ego_body` (R07), class `Bike`/`Pedestrian` ở vùng gần camera (R03), kiểm
  R09 (box không nằm ≥50% trong ignore).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R07 định nghĩa `ego_body` là "thân xe/gương/tay lái
  của xe gắn camera" nhưng ADASIND quay từ xe nhỏ có người ngồi quanh camera; luật không nói người trên xe ego có
  thuộc ego_body không, cũng không có tiêu chí tách với người đi sát bên. Nhóm đã hiểu khác nhau (A: ego, B: Bike)
  và reference vẽ polygon ego rộng tới mức nuốt vật trên đường. Luật không có tiêu chí thì hai người soát sẽ tiếp
  tục bất đồng ở đúng loại vật gần camera, vốn to nhất trong ảnh.
- **Ghi chú kèm (không đổi số rule):** làm rõ R02 cho vật bị che: box bám phần **nhìn thấy**, không kéo qua phần bị
  vật khác che (ca C0 `adasind_019560.jpg` L5: reference kéo box xích lô qua người lái xe máy, finding calib L5).
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round `rework` của lượt kế tiếp (các slice mới và lần sửa reference theo ticket 30); không áp
  ngược để chấm lại bản r1_craft đã khóa.
