# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4
- Tên nhóm: lab11 29092026
- Repo Public: [Link KX-DAY11-TenNhom](https://github.com/nstpvcpiooi/K4-DAY11-Lab1129092026)
- Máy giữ hồ sơ chính / người quản lý: Khuất Tuấn Anh
- Slice chung lấy từ mode.json: B4-center
- Tên định danh vai A dùng cho --self: Khuat Tuan Anh
- Kênh trao đổi nội bộ: Discord
- Đại diện nộp (vai C): Lê Ngọc Nam, 2A202602060
- Commit chốt bài: [627c91e](https://github.com/nstpvcpiooi/K4-DAY11-Lab1129092026/commit/627c91e7e7c101c01003cba10ff5bf607815ef68)

## 2. Ba vai chính


| Vai                              | Họ và tên          | MSSV        | Tên định danh trong mode | Trách nhiệm                                                | Bằng chứng đóng góp                       |
| -------------------------------- | --------------------- | ----------- | --------------------------- | ------------------------------------------------------------ | ---------------------------------------------- |
| A · Gán nhãn                  | Khuất Tuấn Anh      | 2A202602259 | Khuat Tuan Anh              | Parking/C0/slice, self-QC, lock, rework                      | `parking/`, `p1_calib/` (DFD6-CC25), `r1_craft/` (D9C5-98BB, selfqc 9/9), `rework/` (1C9E-1D1E); findings calib + r1_craft; commit 79f81d4, b5be810 |
| B · QA độc lập               | Vương Tuấn Dương | 2A202602046 | Vuong Tuan Duong            | Review trước reference, finding QA, kiểm lại ca sửa     | `r2_qa/qa_review.md` (6 nhận xét + kiểm lại 4 ca rework), 5 dòng r2_qa, `screenshots/qa_*.jpg`; commit 7b24e63 (QA chốt trước reference) |
| C · Chẩn đoán & điều phối | Lê Ngọc Nam         | 2A202602060 | Le Ngoc Nam                 | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | `00_setup/`, `r3_diag/`, 27 dòng r3_diag, `40_decision_log.csv`, `10_error_card.md`, `20/30/45/46/50`, `manifest.json`; commit e041944, 8d682f2 |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha


| Mốc                              | Người giao → nhận | File / commit / mã khóa             | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
| --------------------------------- | --------------------- | ------------------------------------- | ----------------------------- | --------------------------- |
| P0 · Chốt môi trường và vai | C → A, B             | `00_setup/mode.json` (slice B4-center, self = Khuat Tuan Anh), `doctor.txt`, `sensor_context.md`; `parking/annotations.xml` + `observations.md` (CVAT task #46, job #45); nháp `45_sampling_plan.csv` | A: parking export nhận 4 `parking_line` + 1 `free_space`; B: soát vai trò vạch trong `observations.md` | Xong. doctor chỉ cảnh báo chưa có `gh`, cần tự kiểm repo Public |
| P2 · Khóa bản đầu            | A → B, C             | `r1_craft/annotations.xml`, `lock.txt`, `selfqc.md`; slice B4-center; mã khóa **D9C5-98BB** (CVAT task #48); C0 calib khóa DFD6-CC25 | B nhận đúng file + mã trong lock.txt; C kiểm selfqc 9/9 mục đã tick và 4 dòng r1_craft trong findings | Xong. 3 ca chưa chắc ghi cuối selfqc.md chuyển cho QA |
| P3 · Chốt QA mù                | B → C, A             | `r2_qa/qa_review.md`, `qa_overlay.html`, 5 dòng r2_qa trong findings, 6 ảnh `screenshots/qa_*.jpg`; QA trên mã D9C5-98BB | C: mỗi nhận xét có frame → object_ref → rule → ảnh; XML vẫn là bản A đã khóa; chưa mở reference/model | QA đã chốt: 6 nhận xét (1 P0 ego_body_2). Ca chưa rõ: người sau xe hàng rong 271039, class L14 |
| P4 · Quyết định sửa          | C → A, B             | `r3_diag/` (compare, local_quality, model_compare, iou_sweep, zone_table), 27 dòng r3_diag trong findings, `40_decision_log.csv` D1–D8 | A: nhận 4 việc rework (D1–D3); B: đối chiếu từng quyết định với nhận xét QA ban đầu | Xong. Rework: ego_body_2→Bike, gộp L2+L16, bỏ 270517 L8 và 295948 L6. Escalate D4, D5. Mở: D7, D8 |
| P5 · Kiểm bản sửa             | A → B → C           | `rework/annotations-v2.xml`, `lock2.txt` (mã **1C9E-1D1E**), `delta.md`; mục "Kiểm lại sau rework" trong `r2_qa/qa_review.md` | B: 4/4 ca rework đạt trên ảnh; C: số trước/sau gắn với D1–D3 | Xong. mid missing 1→0, center spurious 7→4; L2/L16 vẫn SPURIOUS vì reference không có xe này (đã ghi lý do) |
| P6 · Chốt nộp                  | A, B → C             | `10_error_card.md`, `20_guideline_patch.md`, `30_escalation_ticket.md` (2 ticket), `45_review_plan.md`, `45_sampling_plan.csv`, `46_gold_set_plan.md`, `50_exit_ticket.md`, `manifest.json` | C: `check` exit 0, `failed_gates` rỗng; A: nhãn v2 đúng bản khóa; B: ảnh bằng chứng mở được | Chờ commit chốt + push |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử: `adasind_295948.jpg`, người quấn khăn caro, R07/R03. A (P2) cho là người lái xe ego nên vẽ
  ignore `ego_body`; B (P3, trước reference) chỉ ra bánh xe + pedal riêng và frame 270517 cùng chuyến không có người
  lái trước camera → nghi Bike. C (P4) mở ảnh + reference (R3 Bike truncated) → quyết định rework theo B
  (`40_decision_log.csv` D1; `screenshots/qa_295948_ego_vs_bike.jpg`; findings r2_qa/r3_diag cùng ca).
- Ca còn mở: D7 người sau xe hàng rong 271039 (B theo dõi, cần frame lân cận) và D8 L15 người sau van 271039 (E5).
  Escalation D4, D5 gửi data_ops qua `30_escalation_ticket.md`.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: [Điền phần việc thực tế]
- Thay đổi phân công nếu có: [Thời điểm, lý do, người nhận; nếu không đổi thì ghi rõ]

## 5. Xác nhận trước khi nộp

- [ ]  A xác nhận nhãn và export đúng phiên bản: [Tên / bằng chứng]
- [ ]  B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: [Tên / bằng chứng]
- [ ]  C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên / bằng chứng]
- [ ]  manifest.json tại commit chốt có failed_gates rỗng.
- [ ]  Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ]  C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.

Chỉ đánh dấu việc đã kiểm thật. Nhóm nộp một hồ sơ chung; check không tự chấm đóng góp từng người. Giữ nguyên header/các cột enum của findings.csv; tên người được ghi trong tài liệu này hoặc phần note thích hợp.
