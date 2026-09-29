# QA review · B4-center

Mã khóa: D9C5-98BB

- Reviewer độc lập (B): Vương Tuấn Dương
- Chủ nhãn (A): Khuất Tuấn Anh
- Slice: B4-center (`adasind_270517.jpg`, `adasind_271039.jpg`, `adasind_295948.jpg`)
- File review: `submission/r1_craft/annotations.xml`, mã trong `lock.txt` = D9C5-98BB, khớp mã A bàn giao
- Căn cứ: ảnh gốc + `qa_overlay.html` + `docs/02-rules-vi.md` (rules v1.0.0). **Chưa mở reference hay model của
  slice B4-center** khi viết review này.
- Cách soát: đi từng frame trên overlay, phóng to từng box và vùng ignore, tìm vật ≥40 px chưa có box.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_295948.jpg | ego_body_2 (polygon phải) | R07, R03 | **Thấy:** dưới người quấn khăn caro có bánh xe đạp riêng (x≈640–700, y≈1440–1560) và bàn chân đặt trên pedal (y≈1660–1700). Ở `270517` cùng chuyến (cùng người áo caro ngồi dưới-trái) không có người lái nào ngay trước camera. **Cần kiểm lại:** người này nhiều khả năng là người đạp xe đạp đi song song, không phải thân xe ego; khi đó phải là một box `Bike` (người + xe, R03, truncated) và bỏ polygon ego_body. Mức P0 vì vùng ignore đang loại một vật trong phạm vi. Ảnh: `screenshots/qa_295948_ego_vs_bike.jpg`, `screenshots/qa_270517_no_rider_ahead.jpg` |
| adasind_271039.jpg | none (sau xe hàng rong, x≈1045–1080, y≈865–915) | R01 | **Thấy:** sau xe hàng rong đỏ ở mép phải có đầu và vai một người (áo hồng) nhô lên trên mặt quầy. **Cần kiểm lại:** phần nhìn thấy cao khoảng 45–50 px, bị quầy che và biên khung cắt; nếu đo lại vẫn ≥40 px thì thiếu một box Pedestrian (occluded + truncated). Nếu không đọc được rõ thì dùng `unreadable`. Ảnh: `screenshots/qa_271039_right_stall.jpg` |
| adasind_295948.jpg | L6 | R01 | **Thấy:** box Pedestrian 24×53 px ở xa (288–312, 922–975) nằm trước một biển/cửa màu đỏ sẫm, không thấy rõ đầu hay chân. **Cần kiểm lại:** có thể là biển hiệu, không phải người; nếu không chứng minh được là người thì bỏ box hoặc chuyển thành `unreadable`. Ảnh: `screenshots/qa_295948_L6.jpg` |
| adasind_271039.jpg | L14 | R04 | **Thấy:** vật chở hàng trên nóc, phía trái là mảng phẳng màu xám, phía phải là đầu đen có đèn. **Cần kiểm lại:** chưa đủ căn cứ để chọn ThreeWheeler hay Car (van); nếu là hai xe (van xám + auto đen) thì đang gộp hai vật vào một box. Ảnh: `screenshots/qa_271039_L14_L2_L16.jpg` |
| adasind_271039.jpg | L2, L16 | R02 | **Thấy:** hai box ThreeWheeler liền nhau (196–258 và 258–326) nằm sau dãy người đi bộ; ranh giới giữa hai auto không thấy rõ, phần mui của L2 chỉ lộ một dải nhỏ. **Cần kiểm lại:** xác nhận đây là hai xe khác nhau (không trùng) và cạnh chung x=258 có căn cứ trên ảnh. Ảnh: `screenshots/qa_271039_L14_L2_L16.jpg` |
| adasind_270517.jpg | L8 | R01, R06 | **Thấy:** người đứng khuất sau auto L1, chỉ thấy một dải tối 19×70 px có dạng đầu/vai. **Cần kiểm lại:** hình dáng người khá mờ; A cần nêu căn cứ nhận là người. Nếu không chắc thì `unreadable`. Ảnh: `screenshots/qa_270517_L8.jpg` |

## Đã soát, không có nhận xét

- `lens_border`: hai polygon mỗi frame khớp vòng kính, không lệch.
- `ego_body` ở `270517` (tay + chân người ngồi cùng xe ego) và polygon dưới-trái ở `295948`: đúng R07. `271039` không
  có ego_body: đúng R07.
- `270517`: sửa prefill L1 (bỏ phần mặt đường) và L2 (thu cạnh trái) có căn cứ trên ảnh; L5, L6, L7 thêm đúng.
- `271039`: L7 (người áo đen bước cạnh xe máy) + L9 (xe máy) tách riêng đúng R03 vì người không ngồi trên xe.
- `295948`: L2 người đạp xe chở thùng là một Bike, truncated + occluded hợp lý; L1, L7 là Truck đúng R04.
- Attribute occluded và truncated không bị lẫn vào nhau (R05).

## Kết luận QA

- Số nhận xét: 6 (1 mức P0 cần ưu tiên: ego_body_2; 2 nghi thiếu/thừa theo R01; 3 cần A giải thích thêm).
- Ca chưa rõ: người sau xe hàng rong `271039`, phân loại L14.
- QA đã chốt trước khi mở reference/model. A phản hồi sau mốc này; C phân xử ở P4.

## Kiểm lại sau rework (P5, B · Vương Tuấn Dương)

Kiểm trên `submission/rework/annotations-v2.xml` (lock2 1C9E-1D1E), đối chiếu từng quyết định rework trong
`40_decision_log.csv` với ảnh gốc:

| frame | quyết định | kết quả kiểm lại |
|---|---|---|
| adasind_295948.jpg | D1: bỏ ego_body_2, thêm Bike | Đạt. Polygon bên phải đã xóa; box Bike [574,606,1080,1715] ôm người + xe đạp, từ khăn trên đầu tới bàn chân trên pedal, truncated=true. ego_body dưới-trái giữ nguyên. |
| adasind_271039.jpg | D2: gộp L2+L16 | Đạt. Còn một box ThreeWheeler [196,825,318,900], không còn cạnh cắt ở x=258. |
| adasind_270517.jpg | D3: bỏ L8 | Đạt. Không còn box ở dải tối sau auto. |
| adasind_295948.jpg | D3: bỏ L6 | Đạt. Không còn box trước biển đỏ. |

Không phát hiện thay đổi ngoài bốn ca trên (số hình 45 → 42: bỏ 4, thêm 1).
