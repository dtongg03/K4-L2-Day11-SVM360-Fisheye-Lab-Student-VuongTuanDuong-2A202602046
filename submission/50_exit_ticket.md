# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao? **Không phải lỗi DUPLICATE mặc định, mà cần quy tắc riêng.** `DUPLICATE` là hai box cho một vật
   **trong cùng một ảnh**. Ở seam, mỗi camera là một ảnh riêng; vật thật sự nhìn thấy ở cả hai nên mỗi ảnh có đúng một
   box là hợp lệ, dù hai box khác kích thước và zone (vd. `edge` ở front, `mid` ở left). Chỉ gọi là trùng khi policy
   output đích (vd. danh sách vật trên BEV) yêu cầu một vật một box và đã có timestamp đồng bộ + calibration để chứng
   minh hai box là cùng vật. Quy tắc riêng cần nói: giữ box ở từng camera cho nhãn per-camera; việc ghép thuộc tầng
   fusion, có trường liên kết (cùng object ID) thay vì xóa box. (Đóng góp: C soạn, A và B đọc lại.)
2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera. **Giữ cùng track ID** khi vẫn là cùng một vật
   quan sát liên tục (kể cả khi bị che ngắn rồi xuất hiện lại ở vị trí hợp lý). **Thêm keyframe** khi hình học đổi
   lớn: vật vào vùng rìa vòng kính và bị cong/cắt, đổi hướng, bị che một phần làm box co lại, không để nội suy kéo box
   qua phần khuất (R02). **Đặt Outside** khi vật ra khỏi vòng kính/khung hoặc bị che hoàn toàn; khi quay lại nếu chắc
   cùng vật thì mở lại cùng ID, nếu không chắc thì ID mới. **Trước khi nối track qua hai camera** cần: timestamp đồng
   bộ giữa hai camera, calibration nội/ngoại tham số để chiếu hai vị trí về cùng hệ tọa độ (BEV) và kiểm vị trí/vận
   tốc khớp, policy output (một ID toàn cục hay ID theo camera), và người soát xem đồng thời hai frame ở seam. Thiếu
   một trong các thứ đó thì giữ hai track riêng. (Đóng góp: C soạn, B kiểm theo guideline.)
3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm? **Ca `adasind_295948.jpg`, người
   quấn khăn caro (`ego_body_2` → R3).** A tin người này là người lái xe ego vì quay lưng, rất gần và đi cùng chiều,
   nên vẽ `ego_body`. B (QA mù, trước reference) phản bác bằng hai bằng chứng: có bánh xe đạp + pedal riêng, và frame
   `270517` cùng chuyến không có ai ngồi trước camera. C mở reference ở P4 (R3 Bike truncated) và phân xử theo ảnh,
   không theo số đông → A rework thành box Bike, delta mid missing 1→0 (D1). Ngược lại, ở cùng frame nhóm tin nhãn
   mình đúng khi reference sai (polygon ego quá rộng, xe tải nhỏ gọi Car) và đã escalate thay vì sửa nhãn cho giống
   reference (D4, D5). **Nếu làm lại:** trước khi vẽ bất kỳ `ego_body` nào, A sẽ mở các frame khác của cùng chuyến để
   xác định ai thuộc xe ego (tiêu chí nay đã đưa vào patch R07b), và với vật mờ/khuất (L8, L6) sẽ phóng to và ghi
   chiều cao đo được trước khi vẽ, dùng `unreadable` khi không đọc được thay vì đoán là người. (Đóng góp: A viết phần
   tự nhìn lại, B xác nhận nhận xét QA, C tổng hợp.)
