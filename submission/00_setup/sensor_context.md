# Sensor context

- Rig (theo quan sát, không phải thông số nhà cung cấp): một camera fisheye đơn, ảnh dọc 1080×1920, hướng về phía
  trước theo chiều xe chạy, đặt thấp và lệch trái so với trục xe. Trong C0 (`adasind_019560.jpg`) và slice
  B4-center (`adasind_270517.jpg`) mép dưới-trái có một phần xe/người lái ego, nên nhiều khả năng camera gắn trên
  một xe hai bánh hoặc xe nhỏ chạy bên trái đường. ADASIND không kèm tài liệu rig, calibration hay vị trí lắp chính
  xác; nhóm không suy ra tiêu cự, góc nhìn hay độ cao camera từ ảnh.
- `ego_body`: nhìn thấy ở góc dưới-trái của vòng kính, ví dụ chân, giày và áo của người lái/người ngồi sau
  (`adasind_270517.jpg`, khoảng x 0–150, y 1030–1720) và phần thân xe/tay lái cùng bóng xe ego (`adasind_019560.jpg`,
  khoảng x 0–150, y 1070–1560). Có frame không thấy thân xe ego (ví dụ `adasind_006840.jpg`,
  `adasind_271039.jpg`), khi đó không vẽ polygon `ego_body`.
- Vòng kính (lens circle): gần tròn, tâm hơi dưới giữa ảnh (theo `assets/frames.csv`, ví dụ `adasind_019560.jpg` có
  cx≈604, cy≈906, r≈771 px), bán kính gần bằng 70% chiều rộng. Vòng kính bị cắt hai bên trái/phải và chiếm khoảng
  70–75% diện tích khung; dải đen/viền kính phía trên (y<~150) và phía dưới (y>~1650) là vùng `lens_border`, không
  có thông tin cảnh.
- Giới hạn: chỉ có một camera, không có ảnh trái/phải/sau, không có timestamp đồng bộ, không có calibration nội/ngoại
  tham số. Mọi nhận xét về bốn camera SVM trong kế hoạch 45/46 là giả lập, không đo được trên dữ liệu này.
