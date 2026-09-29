# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `d9c598bbbe7ad7b1db0e0bbd4ee5a294474c7cf6bc04466d85738bc41a07b626`; slice `B4-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_270517.jpg, adasind_271039.jpg, adasind_295948.jpg. Frame thiếu trong export: không.
TP=18; FP=8; FN=2; số lần đối chiếu=27; mean IoU của TP=0.788.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.667 | 0.926 | 0.852 |
| precision | 0.692 | 0.694 | 0.500 |
| recall | 0.900 | 0.893 | 0.667 |
| jaccard | 0.643 | 0.621 | 0.500 |
| dice | 0.783 | 0.760 | 0.667 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 2 | 1 | 1 | 0.926 | 0.667 | 0.667 | 0.500 | 0.667 |
| Car | 4 | 0 | 1 | 0.963 | 1.000 | 0.800 | 0.800 | 0.889 |
| Pedestrian | 7 | 4 | 0 | 0.852 | 0.636 | 1.000 | 0.636 | 0.778 |
| ThreeWheeler | 4 | 2 | 0 | 0.926 | 0.667 | 1.000 | 0.667 | 0.800 |
| Truck | 1 | 1 | 0 | 0.963 | 0.500 | 1.000 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_270517.jpg | 7 | 1 | 0 | 0.875 | 0.875 | 1.000 |
| adasind_271039.jpg | 10 | 5 | 0 | 0.667 | 0.667 | 1.000 |
| adasind_295948.jpg | 1 | 2 | 2 | 0.250 | 0.333 | 0.333 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 2 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 4 | 0 | 0 | 1 | 0 |
| Pedestrian | 0 | 0 | 7 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 4 | 0 | 0 |
| Truck | 0 | 0 | 0 | 0 | 1 | 0 |
| <extra> | 1 | 0 | 4 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
