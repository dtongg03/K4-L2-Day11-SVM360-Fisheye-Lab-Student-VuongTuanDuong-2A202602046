# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| **Frame có người/xe sát camera và vùng ego** (vd. `adasind_295948.jpg`, `adasind_270517.jpg`, dưới-trái/phải vòng kính, zone mid) | 1 MISSING P0 (người đạp xe bị đưa vào ego_body, R3), 4 IGNORE_SCOPE do polygon ego của reference quá rộng (L2–L5), 1 WRONG_CLASS (xe tải nhỏ) | Lỗi phạm vi P0 làm vật lớn nhất ảnh biến mất khỏi mọi phép đo, và cả người vẽ lẫn reference đều sai ở cùng loại vùng. Luật R07 chưa có tiêu chí (patch R07b) nên cần hai người soát độc lập mọi frame có người gần camera | Overlay L/R/M, ảnh `qa_295948_ego_vs_bike.jpg`, `esc_295948_ref_ego_polygon.jpg`, frame khác cùng chuyến để chứng minh ai thuộc xe ego |
| **Vỉa hè đông, vật che nhau ở center** (vd. `adasind_271039.jpg`) | 7 SPURIOUS L ở center trước rework (4 sau rework): 2 box gộp/cắt sai auto (E1), 2 vật khuất L và/hoặc M thấy nhưng R thiếu (E0), 1 người mờ sau van (E5); 1 MISSING chưa giải (người sau xe hàng rong) | Center chứa 12/20 box reference; lỗi ở đây không do méo fisheye mà do che khuất và đếm vật trong cụm. Đây cũng là nơi reference thiếu vật, nên số FP của người vẽ bị thổi phồng nếu không rà | Crop phóng to từng vật khuất, chiều cao đo được (≥40 px), model_compare cell, quyết định D2/D7/D8 |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame, 20 box reference, một camera phía trước và một chuyến đi;
một lỗi đổi một con số lớn (Truck precision 0,5 chỉ vì một xe). Reference là teaching reference, có lỗi (2 ticket).
Không suy ra tỉ lệ lỗi hay "zone nào khó nhất" cho cả bộ 48 frame, càng không cho hệ SVM bốn camera; hai lát cắt
trên chỉ là nơi nên soi trước.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: (1) mỗi ô camera × normal/hard lấy từ nhiều chuyến/ngày,
giới hạn tối đa 2–3 frame mỗi cảnh và cách nhau ≥2 giây, để 200 frame không bị vài cảnh dài chiếm chỗ; frame liền
nhau của cùng cảnh chỉ tính như **một** ca độc lập khi đếm độ phủ. (2) Lập bảng kiểm độ phủ cho từng camera theo các
trục: ngày/đêm/ngược sáng, mật độ vật, có vật ở rìa vòng kính, có ego_body/gương, có vật đi qua seam, có ThreeWheeler
và rider. Ô nào thiếu một trục thì bổ sung trước khi review. (3) Ghi nguồn chuyến/timestamp của từng frame để truy
lại. Kế hoạch này chỉ giúp **tìm ca cần soi**: mẫu được chọn có chủ đích (thiên về hard) nên tỉ lệ lỗi đo trên 200
frame không đại diện cho 50.000 frame; muốn đo tỉ lệ lỗi cần thêm một mẫu ngẫu nhiên phân tầng riêng.
