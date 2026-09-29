# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): vẽ 4 polyline trên `parking-lot-core.jpg` (960×720).
  (1) Vạch trắng ở tiền cảnh giữa-dưới, từ (403,652) xuống mép dưới ảnh (538,719); (2) vạch trắng tiền cảnh bên phải
  từ (695,624) đến (952,683); (3) và (4) hai vạch ngắn của hàng ô giữa bãi, (173,522)→(248,563) và
  (328,531)→(421,554). Cả bốn là đoạn sơn ngắn, song song nhau, cách đều theo chiều ngang, mỗi đoạn là ranh giới giữa
  hai ô đỗ kề nhau; đầu vạch dừng ở chỗ sơn kết thúc, không kéo vào lối xe chạy. Vạch (1) bị mép dưới ảnh cắt nên
  chỉ vẽ tới biên ảnh.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: (a) dải sơn mảnh chạy ngang gần hết bề rộng bãi ở y≈515–545
  (đầu các ô hàng giữa) không vẽ là `parking_line` vì nó là vạch đầu/đuôi nối nhiều ô, chạy dọc theo lối xe, không
  tách riêng một ô; (b) mép bê tông/curb vàng ở cuối bãi (y≈465–470) là biên bãi, không phải vạch chia ô; (c) các
  đoạn sơn rất xa ở nền trên (y<500) quá mờ, không đọc chắc vai trò nên không vẽ.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: polygon phủ lối xe chạy trống giữa hàng ô giữa và hàng
  ô tiền cảnh. Cạnh trên dừng ngay dưới đầu dưới các vạch hàng giữa (y≈532–579), cạnh dưới dừng ở đầu trên các vạch
  tiền cảnh (y≈591–680); hai bên chạm biên ảnh. Không có xe, cây hay curb trong vùng này; chiếc xe đỏ ở xa
  (x≈350, y≈460) nằm ngoài polygon. Đây chỉ là mặt đường trống nhìn thấy trên ảnh tĩnh, không khẳng định xe có thể
  đi an toàn qua vùng đó.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): dải sơn ngang dài ở mục (a) cũng góp phần tạo
  đầu ô đỗ; nhóm chọn không gán `parking_line` vì nó không chia hai ô kề nhau. Người soát (B) cần xác nhận cách hiểu
  này với `docs/11-parking-lines-vi.md`. Ảnh đối chiếu `parking-lot-contrast.png` được dùng để phân biệt: ở đó vạch
  ngắn tiền cảnh chia ô, còn lối xe cong ở giữa không có vạch chia ô.
