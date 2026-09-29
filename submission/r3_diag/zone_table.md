# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 12 | 1 | 7 | 4 | 7 | SPURIOUS (6) |
| mid | 5 | 1 | 1 | 2 | 4 | SPURIOUS (1) |
| edge | 3 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **center** gãy nhiều nhất cho cả hai phía. L
  có 7 spurious/12 n_ref ở center (so với 1 ở mid, 0 ở edge); M thừa 7 và thiếu 4 ở center. Edge lại ổn: L ghép đủ
  3/3, M chỉ thiếu 1 (auto 271039 L13 bị gọi Car). Mid có 1 L missing, là người đạp xe quấn khăn caro 295948 (R3) mà
  A đã đưa nhầm vào `ego_body`.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi L ở center
  không do méo fisheye mà do (1) vỉa hè đông người/xe che nhau ở 271039: A vẽ vật khuất một phần (L6, L9, L15) mà R bỏ
  qua, và cắt một auto nhìn ngang thành hai box (L2+L16); (2) hai box thiếu căn cứ (270517 L8, 295948 L6), B đã bắt
  được ở QA. Lỗi mid là lỗi phạm vi `ego_body` (quyết định sai người thuộc xe ego), không phải hình học. Phía M, phần
  lớn "thừa/thiếu" ở center là do model không có class ThreeWheeler (6/6 auto bị gọi Car/Truck, có lúc hai box cho
  một xe) và box người ngồi trong xe (trái R03), nên không nên đọc là model yếu ở center. Giới hạn: chỉ 3 frame, 20
  box reference, một camera; center có 12/20 box nên tự nhiên chiếm đa số lỗi; center/mid/edge là khoảng cách tới
  tâm vòng kính, không nói vật gần/xa xe. Không suy ra phân bố lỗi của cả hệ SVM từ slice này.
