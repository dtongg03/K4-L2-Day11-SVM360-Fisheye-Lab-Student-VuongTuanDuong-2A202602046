# Tự soát


## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

## Fill ratio (K12)
- adasind_270517.jpg box 1 center: 0.893
- adasind_270517.jpg box 2 edge: 0.796
- adasind_271039.jpg box 10 edge: 0.720
- adasind_271039.jpg box 13 edge: 0.919
mean edge: 0.812 (n=3)
mean center: 0.893 (n=1)

## Ghi chú soát (A · Khuất Tuấn Anh, bản nháp `exports/r1-draft.xml`)

Soát trên overlay dựng từ chính XML nháp (box + lens_border + ego_body + unreadable) cho cả ba frame, chưa mở
reference/model của slice B4-center.

1. Phạm vi: vẽ mọi vật ≥40 px trong vòng kính. Bỏ có chủ đích: xe con xa sau Car L4 ở 270517 (cao ~39 px), xe trắng
   rất xa giữa đường ở 295948 (~26 px), các xe/người li ti ở nền. Prefill 270517 thiếu 4 vật đã thêm: ThreeWheeler
   đang chạy (L5), Pedestrian nữ mang túi (L6), xe máy đỗ sau auto (L7), người đứng sau auto (L8).
2. lens_border: hai polygon import khớp vòng kính ở cả 3 frame, không sửa. ego_body: 270517 có tay + chân người ngồi
   trên xe ego (góc dưới-trái); 271039 không có thân xe ego nên không vẽ (R07); 295948 có hai vùng ego: người áo caro
   ngồi cạnh camera (dưới-trái) và người quấn khăn caro quay lưng ngay trước camera, đi cùng chiều/cùng tốc độ, được
   coi là người điều khiển xe ego (xích lô) nên xếp ego_body thay vì box Bike. Đây là ca chưa chắc, nhờ QA soát.
3. Class: xe tải nhỏ chở hàng 295948 L1 → Truck (R04); van trắng 271039 L12 → Car; auto-rickshaw → ThreeWheeler.
4. Rider: người đạp xe chở thùng "PDF" 295948 L2 = một Bike; 271039 người áo đen L7 đang bước cạnh xe máy (không
   ngồi) → Pedestrian L7 + Bike L9 riêng; người lái trong auto 270517 L1 không box riêng.
5. Geometry: sửa box prefill ThreeWheeler 270517 L1 (ybr 1030 → 982, bỏ phần mặt đường dưới bánh), Bike L2 (xtl
   995 → 1012), Car L3 (xbr 337 → 350 theo đầu xe thật). Box bám phần nhìn thấy, không nắn thẳng.
6. Attribute: truncated cho vật bị biên khung cắt (270517 L2, 295948 L2); occluded cho vật bị vật khác che
   (vd. 270517 L3 sau người đi bộ, 271039 L13 sau van, L10 sau cột). Hai attribute đặt độc lập.
7. Thiếu/trùng: không có hai box cho cùng một vật; 271039 L6 và L7 chồng nhau nhưng là hai người khác nhau (người
   áo sáng phía sau, người áo đen phía trước, IoU ≈ 0,37).
8. ignore_region: mỗi polygon có đúng một reason (lens_border, ego_body, unreadable). Vật mỏng ~16 px ở mép trái
   270517 không đọc được là người hay xe → `unreadable`. Không box nào nằm ≥50% trong ignore.
9. Task `Day11 · ADASIND · B4-center · raw_fisheye`; export CVAT for images 1.1, không kèm ảnh, export ở cấp task để
   giữ tên task trong XML.

Ca chưa chắc cần QA xem: (a) người quấn khăn caro 295948 là ego hay Bike; (b) 271039 L14 là ThreeWheeler hay Car
(phía sau chở hàng trên nóc, nhìn mờ); (c) 270517 L8 người đứng khuất sau auto, chỉ thấy một phần.
