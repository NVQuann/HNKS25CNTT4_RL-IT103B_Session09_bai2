## 1. Phân tích lỗi & Bảng Test Cases

**Phân tích nguyên nhân kỹ thuật:**

- **Lỗi 1 (Truy cập biến động sai):** `bookingReservation.priceKey` sử dụng Dot Notation, khiến trình duyệt tìm kiếm một thuộc tính có tên định danh chính xác là "priceKey", dẫn đến kết quả `undefined` và gây lỗi logic tính toán tổng tiền (`NaN`).
- **Lỗi 2 (Xóa thuộc tính không triệt để):** `bookingReservation.discountCode = undefined;` chỉ thay đổi giá trị của key thành `undefined`. Bản thân cái key (tên thuộc tính) `'discountCode'` vẫn tồn tại trong bộ nhớ của object, điều này làm sai lệch nếu sau này dùng vòng lặp `for...in` hoặc toán tử `in` để kiểm tra. Phải dùng từ khóa `delete` để xóa bỏ hoàn toàn.
- **Lỗi 3 (Vòng lặp for...in):** `bookingReservation.key` hiểu nhầm `key` là tên cứng của một thuộc tính, dẫn đến toàn bộ vòng lặp đều in ra `undefined`. Bắt buộc phải dùng `bookingReservation[key]` để nội suy giá trị của biến `key` ra tên thuộc tính tương ứng.

**2. Bảng Test Cases đối chứng:**

| Trường hợp kiểm thử | Dữ liệu đầu vào | Kết quả sai thực tế (Khi chưa sửa) | Kết quả đúng mong đợi (Sau khi sửa) |
| **TC01: Xử lý mã giảm giá không hợp lệ** | `isVoucherValid = false` | `'discountCode' in bookingReservation` trả về `true`. | `'discountCode' in bookingReservation` trả về `false`. |
| **TC02: Truy xuất giá trị động trong vòng lặp `for...in`** | Đối tượng `bookingReservation` chứa các thông tin đã cập nhật | Lặp qua các dòng in ra giá trị `undefined` cho mọi key. | In ra được giá trị thực tế của từng key (VD: `roomPrice: 1500000`). |
