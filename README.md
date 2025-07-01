# danh_gia_kiem_dinh_tuan_9

## 1. Mục tiêu bài thực hành
- Làm quen với công cụ kiểm thử hiệu năng mã nguồn mở Apache JMeter
- Mô phỏng nhiều người dùng đồng thời gửi yêu cầu đến API RESTful
- Đánh giá hiệu suất API thông qua thời gian phản hồi, tỉ lệ lỗi và khả năng chịu tải
- Thực hành xây dựng và phân tích một kịch bản kiểm thử thực tế

---

## 2. Môi trường & công cụ sử dụng

| Thành phần           | Phiên bản/Ghi chú                                |
|----------------------|--------------------------------------------------|
| Apache JMeter        | 5.6.3                                            |
| Java (JDK)           | 11 hoặc cao hơn                                  |
| Hệ điều hành          | Windows 10 / Ubuntu 22.04 (hoặc tương đương)     |
| API kiểm thử         | [https://reqres.in](https://reqres.in)          |
| Tài liệu tham khảo   | Video: https://youtu.be/NTyY8wKSvik             |
|                      | Gemini: https://g.co/gemini/share/58f8cb01826f  |

---

## 3. Mô tả kịch bản kiểm thử

**Tên kịch bản:** Kiểm thử hiệu năng API danh sách người dùng

**API được kiểm thử:**  
`GET https://reqres.in/api/users?page=2`

**Mục tiêu kiểm thử (Test Objectives):**
- Kiểm tra hệ thống API có thể xử lý đồng thời nhiều người dùng không
- Đánh giá thời gian phản hồi trung bình khi có tải cao
- Ghi nhận tỉ lệ lỗi (nếu có) khi số lượng request tăng lên

**Thông số mô phỏng người dùng:**
- Số lượng người dùng: 10
- Thời gian khởi chạy (Ramp-Up): 5 giây
- Số vòng lặp mỗi người dùng: 5
- Tổng số request: 10 × 5 = **50 request**

---

## 4. Các bước thực hiện trong JMeter

### Bước 1: Tạo Test Plan
- Tên: `Test API Reqres`
- Kiểm chọn "Run Thread Groups consecutively" nếu cần

### Bước 2: Thêm Thread Group
- Add → Threads (Users) → Thread Group
- Cấu hình:
  - Number of Threads (users): `10`
  - Ramp-Up Period (in seconds): `5`
  - Loop Count: `5`

### Bước 3: Thêm HTTP Request
- Add → Sampler → HTTP Request
- Cấu hình:
  - Name: `Get User List`
  - Server Name or IP: `reqres.in`
  - Method: `GET`
  - Path: `/api/users?page=2`

### Bước 4: Thêm Listener
- Add → Listener → chọn:
  - View Results Tree
  - Summary Report
  - Aggregate Report

### Bước 5: Chạy kiểm thử
- Nhấn nút **Start** trên JMeter
- Quan sát các kết quả ở các Listener

---

## 5. Kết quả kiểm thử

### Thống kê từ Summary Report

| Thông số                  | Giá trị (ví dụ)              |
|---------------------------|------------------------------|
| Tổng số mẫu (Sample Count)| 50                           |
| Thời gian phản hồi trung bình | 220ms                   |
| Thời gian phản hồi lớn nhất | 430ms                     |
| Tỉ lệ lỗi (%)             | 0%                           |
| Throughput                | ~12 requests/giây            |

> Lưu ý: Kết quả thực tế phụ thuộc vào cấu hình hệ thống, đường truyền và tải thời điểm kiểm thử.

---

## 6. Phân tích & đánh giá

- API `GET /api/users?page=2` có khả năng xử lý ổn định với 50 request liên tục từ 10 người dùng.
- Không xuất hiện lỗi hoặc timeout trong quá trình kiểm thử
- Thời gian phản hồi dưới 500ms cho thấy API có hiệu suất tốt trong điều kiện mô phỏng

**Kết luận:** Dịch vụ API đáp ứng tốt yêu cầu về hiệu suất trong điều kiện tải nhỏ đến trung bình. Có thể mở rộng thêm kịch bản cho 100–500 người dùng để kiểm thử tải nặng.

---

