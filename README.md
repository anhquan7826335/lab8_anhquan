# lab8_anhquan
# Báo cáo kiểm thử hiệu năng ứng dụng quản lý thư viện nhạc bằng Apache JMeter

## 1. Mục tiêu

- Thực hành cấu hình Thread Group và đọc Summary Report trên Apache JMeter.
- Mô phỏng nhiều người dùng truy cập ứng dụng quản lý thư viện nhạc trực tuyến.
- Ghi nhận số request, thời gian phản hồi, tỷ lệ lỗi và thông lượng ở lần chạy được chụp.

## 2. Công cụ và môi trường kiểm thử

| Thành phần | Thông tin |
| --- | --- |
| Công cụ | Apache JMeter 5.6.3 |
| Ứng dụng | Music Library, Spring Boot |
| Địa chỉ kiểm thử | `http://localhost:8080` |
| Kịch bản | `app/MainProject/QuanLyThuVienNhacOnline/web/src/test/jmeter/music-library-load-test.jmx` |
| Loại thao tác | Đăng nhập và đọc dữ liệu |

Kịch bản sử dụng tài khoản kiểm thử đã đăng ký. Mỗi người dùng ảo giữ session riêng; các request đọc dữ liệu gồm trang chủ, thể loại, tìm kiếm thể loại, nghệ sĩ, bài hát và tìm kiếm bài hát.

## 3. Cấu hình Thread Group

Ảnh cấu hình trong JMeter hiển thị Thread Group **Music library visitors** với các thuộc tính:

<img width="959" height="545" alt="Screenshot 2026-10-14 180142" src="https://github.com/user-attachments/assets/95c0e78d-93ca-4356-b605-56a609f6b691" />


Các giá trị `${__P(...)}` là giá trị mặc định của tham số JMeter, không nhất thiết là cấu hình thực tế của một lần chạy. Theo lệnh chạy đã cung cấp, lần chạy trong Summary Report dùng **120 luồng**, ramp-up **1 giây**, thời lượng cấu hình **15 giây**. Vì vậy cấu hình thực tế khác giá trị mặc định đang hiển thị trên ảnh Thread Group.

## 4. Kết quả Summary Report

Số liệu dưới đây được ghi lại từ ảnh Summary Report:

<img width="959" height="599" alt="Screenshot 2026-10-14 180328" src="https://github.com/user-attachments/assets/2aec6b69-349f-46c9-82fa-ecaca8889493" />


Thông lượng tổng trong ảnh khoảng **18.3 samples/giây**. Summary Report cũng hiển thị tốc độ nhận khoảng **107.92 KB/giây**, tốc độ gửi khoảng **4.68 KB/giây** và kích thước phản hồi trung bình khoảng **5,870.7 bytes**.

## 5. Nhận xét

- Các request xuất hiện trong bảng có Error `0.00%`; đây là kết quả theo các mẫu được ghi nhận và assertion của kịch bản.
- Thời gian trung bình của request danh sách thể loại là **9,291 ms**; tìm kiếm thể loại là **13,656 ms**. Đây là hai thao tác đọc chậm nhất trong các dòng đang hiển thị.
- Tổng có **511 samples**, nhưng Summary Report chỉ hiển thị 31 mẫu tìm kiếm thể loại và không có dòng cho danh sách nghệ sĩ, danh sách bài hát hoặc tìm kiếm bài hát. Do đó ảnh này chưa thể hiện đủ toàn bộ hành trình đã khai báo trong Test Plan.
- Kết quả này chỉ mô tả lần chạy được chụp. Không thể từ Error `0.00%` kết luận ứng dụng đáp ứng mọi mức tải hoặc mọi chức năng.
- Bảng không hiển thị P90/P95; không suy ra các percentile này từ cột Average.

## 6. Kết luận

Hai ảnh minh họa cấu hình Thread Group dạng tham số hóa và kết quả Summary Report của một lần chạy JMeter. Lần chạy ghi nhận 511 samples, Error `0.00%`, Average tổng **3,078 ms** và throughput khoảng **18.3 samples/giây**. Thời gian phản hồi của danh sách và tìm kiếm thể loại cao trong các mẫu hiển thị; cần kiểm tra thêm bằng một lần chạy hoàn chỉnh, đảm bảo tất cả sampler có mẫu trước khi đưa ra kết luận tổng quát về hiệu năng.
