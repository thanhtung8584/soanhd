# Kết quả kiểm thử v1.0

- JavaScript app.js/core.js/server.mjs: kiểm tra cú pháp PASS.
- Chromium headless: PASS nhập CSV và XLSX, giữ số 0 đầu, đọc đúng bên mua XML, từ chối XML thiếu vùng mua.
- Tính tiền: PASS số lượng × đơn giá, chiết khấu trước thuế, VAT và tiền bằng chữ; không nhân thời hạn tháng.
- 11 DOCX mẫu: PASS điền các placeholder đã gắn, không còn placeholder sau render với fixture đầy đủ.
- PASS lặp 2 sản phẩm ở báo giá/Connect PL01; PASS phần chức năng nằm trong HSS PL01.
- PASS thiếu trường được báo; PASS lưu khách hàng vào IndexedDB và tải lại.
- UI: PASS duyệt mẫu báo giá, chọn khách hàng, nhập sản phẩm và tải ZIP có DOCX + bảng kê CSV.
- Apps Script backend chạy trong mô phỏng Drive: PASS load/save, base64, hash mẫu không đổi, xung đột revision và giữ phiên bản cũ.
- Render LibreOffice: các Word kiểm thử mở/chuyển PDF được; xem bản render 11 mẫu. Sửa header báo giá lặp/tràn chữ bằng header văn bản; bỏ trang trắng sau Mẫu 95.

## Chưa kiểm chứng trên môi trường thật

- OAuth, iframe bridge và Drive trên một Apps Script deployment thật.
- API AI với khóa/model thật, chi phí/độ chính xác.
- OCR ảnh/PDF và gộp PDF tự động chưa có trong bản này.
- Chỗ trống/dấu chấm chưa gắn placeholder trong Connect cần người dùng cấu hình. PASS placeholder không có nghĩa là mọi trường mẫu đã nhận diện đầy đủ.
- Việc đủ nội dung và bố cục sau nhập dữ liệu dài/nhiều dòng cần kiểm tra Word trước phát hành.

Chạy kiểm thử: npm install → npx playwright install chromium → npm test. Có thể đặt BROWSER_PATH nếu dùng Chromium có sẵn. Node 20+.
