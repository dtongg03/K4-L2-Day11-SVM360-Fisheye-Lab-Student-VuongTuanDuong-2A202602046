# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_270517.jpg
- L8 center SPURIOUS
## adasind_271039.jpg
- L8 center IGNORE_SCOPE
- L2 center SPURIOUS
- L6 center SPURIOUS
- L9 center SPURIOUS
- L15 mid SPURIOUS
- L16 center SPURIOUS
## adasind_295948.jpg
- L2 mid IGNORE_SCOPE
- L3 mid IGNORE_SCOPE
- L4 mid IGNORE_SCOPE
- L5 mid IGNORE_SCOPE
- L1+R1 center WRONG_CLASS
- L6 center SPURIOUS
- R3 mid MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 12 | 11 | 1 | 7 |
| mid | 5 | 4 | 1 | 1 |
| edge | 3 | 3 | 0 | 0 |
