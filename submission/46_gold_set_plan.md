# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Normal (25): xe/người center–mid ban ngày. Hard (35): vật ở rìa vòng kính bị cong/cắt, rider và người dắt xe, ThreeWheeler, cụm người che nhau trên vỉa hè, ngược sáng/đêm | ADASIND cho thấy lỗi center do che khuất và cắt đôi một xe (271039 L2+L16), lỗi class ThreeWheeler của model, và người sát camera bị nhầm vào ego (295948) | Vẽ trên ảnh fisheye gốc (R02), giữ ID camera, intrinsics/tâm + bán kính vòng kính từng frame, phiên bản rules | Hai annotator vẽ độc lập, người QA thứ ba soát mù trước khi xem model; bất đồng dùng ảnh + rule, không bỏ phiếu; ca không giải ghi E5 và loại khỏi gold |
| rear | Normal (20): xe bám đuôi, bãi đỗ khi lùi. Hard (30): vật thấp sát cản sau gần ngưỡng 40 px, trẻ em/người sau xe, cản/biển số ego che, đèn hậu chói ban đêm | Vùng ego (cản sau) lớn, dễ vẽ ignore quá rộng như polygon reference 295948 (R09); vật thấp dễ lệch ngưỡng R01 | Polygon ego_body đo riêng cho rear, extrinsics camera sau (độ cao, góc nghiêng), ngưỡng H đo trên ảnh gốc | Như front; thêm bước đo chiều cao box quanh 40 px bằng công cụ, không ước bằng mắt; soát riêng mọi polygon ego/ignore trước khi chấm box |
| left | Normal (20): xe song song, lề đường. Hard (25): vật đi qua seam với front/rear, gương + thân xe ego che, xe máy sát thân xe, curb | Vật ở seam xuất hiện ở hai camera, dễ bị gọi DUPLICATE; méo mạnh ở rìa; gương là ego_body cố định | Extrinsics left, timestamp đồng bộ với front/rear, vùng chồng seam đo từ calibration, polygon gương | Hai người soát; ca seam phải xem đồng thời cả hai camera cùng timestamp; bất đồng về ghép box chuyển cho người giữ policy seam |
| right | Normal (20): lề phải, người trên vỉa hè, xe đỗ. Hard (25): seam với front/rear, người/xe hai bánh sát lề, cửa xe mở, vùng tối dưới gầm | Tương tự left nhưng phân bố vật khác theo chiều lưu thông; không suy từ camera trái | Extrinsics right, timestamp, vùng seam, polygon ego riêng | Như left; kiểm độ đồng thuận riêng cho right, không mượn kết quả left |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): refresh khi (1) thay/lắp lại camera hoặc
  calibration đổi (tâm/bán kính vòng kính, extrinsics lệch) — nhãn trên ảnh gốc có thể vẫn đúng nhưng vùng ego,
  seam và zone đổi; (2) guideline đổi phiên bản (vd. R07b ở `20_guideline_patch.md` đổi cách xử lý người gần
  camera) — soát lại mọi frame bị rule mới chạm tới; (3) thêm class (vd. model mới có ThreeWheeler); (4) phát hiện
  lỗi reference như ticket 30 — sửa và ghi version. Mỗi lần refresh tăng version gold và giữ bản cũ để so.
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: một xe máy vượt từ góc trước-trái sang
  hông trái — cùng timestamp thấy ở `front` (edge, bị cắt) và `left` (mid, gần đủ thân). Trước khi coi hai box là
  một vật hoặc xóa một box cần: timestamp đồng bộ của hai frame, calibration/extrinsics để chiếu hai box về cùng
  không gian (BEV), và policy output đích (giữ box ở cả hai camera cho bài toán per-camera, hay hợp nhất một box cho
  BEV). Khi chưa có ba thứ đó, giữ cả hai box, không gọi DUPLICATE.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  bài ADASIND cho thấy chính reference cũng sai (polygon ego quá rộng, xe tải nhỏ gọi Car) và hai người trong nhóm
  bất đồng ở cùng một loại vật; đồng thuận chỉ chứng minh hai người hiểu luật giống nhau, không chứng minh luật/nhãn
  đúng. Một camera trước không có ego, seam, góc nhìn và phân bố vật của rear/left/right; local-quality chỉ chấm
  rectangle so với một reference, không chấm polygon/track hay ghép cross-camera. Gold cho mỗi camera phải được soát
  độc lập trên dữ liệu của chính camera đó.
