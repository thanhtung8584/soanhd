# VNPT Contract Studio v1.0

Ứng dụng lập bộ hồ sơ hợp đồng: GitHub Pages xử lý file trong trình duyệt, Apps Script lưu khách hàng và mẫu Word trên Google Drive. Có chế độ độc lập lưu IndexedDB để dùng thử.

## Đánh giá tính khả thi và logic

- Khả thi ngay: nhập khách hàng Excel XLSX/CSV/XML hóa đơn; nhập nhiều sản phẩm; bảng phụ huynh/học sinh; điền DOCX; xuất ZIP gồm Word, dữ liệu và bảng kê CSV.
- Khả thi có bước duyệt: nhận diện mẫu bằng ngữ cảnh AI, trích xuất khách hàng từ văn bản. AI trả gợi ý, người dùng xác nhận; không tự sửa điều khoản.
- Chưa tích hợp: OCR giấy phép dạng ảnh/PDF; DOC cũ trực tiếp; xuất/gộp PDF tự động; đồng bộ thời gian thực; phân quyền nhóm. Cần bổ sung máy chủ OCR/PDF và cơ chế tài khoản nếu mở rộng.
- Không cam kết AI tự nhận diện đúng mọi mẫu. Mẫu được cấu hình và duyệt một lần; điền hàng loạt bằng quy tắc.
- Dữ liệu khách hàng là chuỗi để giữ số 0 đầu. Excel đã làm mất số 0 thì ứng dụng không thể phục hồi chắc chắn.
- Một khách hàng nhiều sản phẩm; phụ huynh/học sinh là bảng riêng. Người ký, người mua, nhà trường và bên cung cấp không được đồng nhất tự động.
- Đơn giá nhập CHƯA thuế, tính theo toàn bộ gói. Thời hạn tháng là mô tả, không nhân thêm vào tiền. Chiết khấu trên tiền hàng, VAT sau chiết khấu, làm tròn VND từng dòng. Thuế do người dùng cấu hình; không suy đoán từ báo giá nguồn.
- Tất cả mẫu đều cần duyệt lần đầu. Trong hồ sơ HSS, cước phí/phần chức năng nằm ở PL01; đầu mối là PL02. Connect: cước phí PL01, đăng ký PL02, đầu mối PL03.

## 1. Upload GitHub cho người mới

1. Tạo repository `contract-studio` trên GitHub. Chọn private nếu tài khoản/gói hỗ trợ Pages cho repo private; nếu public phải xem lại các mẫu trước khi upload.
2. Giải nén ZIP, mở thư mục `contract-studio`. Upload toàn bộ nội dung bên trong để `index.html` nằm ngay gốc repository.
3. Không upload file khách hàng thực tế, sao lưu JSON hoặc khóa API. Các mẫu đã loại thông tin khách hàng chính nhưng vẫn có nội dung/nhận diện đơn vị VNPT: rà soát trước khi công khai.
4. Cách đơn giản: Settings → Pages → Deploy from a branch → main → /(root) → Save. Không cần workflow.
5. Hoặc chọn GitHub Actions và thêm `.github/workflows/pages.yml`. Có thể tạo thư mục/file này bằng Add file → Create new file nếu upload web bỏ thư mục ẩn.
6. Mở `https://TEN_TAI_KHOAN.github.io/contract-studio/` để thử. Không có bước npm/build để triển khai; package.json chỉ phục vụ kiểm thử.

## 2. Apps Script + Google Drive

1. Vào script.google.com, tạo dự án mới. Dán `apps-script/Code.gs` vào Code.gs.
2. Tạo HTML tên `Index`, dán `apps-script/Index.html`.
3. Trong Code.gs đổi `GITHUB_URL` sang URL Pages vừa tạo (có dấu / cuối).
4. Project Settings → bật hiển thị appsscript.json; dán file manifest đi kèm.
5. Deploy → New deployment → Web app.
6. **Execute as: User accessing the web app**. Chọn phạm vi truy cập phù hợp người trong tổ chức. KHÔNG đổi sang chạy dưới tài khoản chủ dự án nếu không có kiểm soát người dùng.
7. Mở link /exec, đăng nhập và cấp quyền Drive. Mỗi người có thư mục riêng `VNPT Contract Studio` trong Drive cá nhân.
8. Thư mục chứa `data.json` và các DOCX mẫu. Mẫu đổi được tạo phiên bản mới, phiên bản cũ giữ lại. Không tự lưu bộ hồ sơ xuất vào Drive, tránh tạo file rác.
9. Sau khi sửa Code.gs/Index, cập nhật deployment bằng version mới. Sau khi sửa GitHub, đợi Pages deploy xong và tải lại.

Apps Script làm cửa sổ chủ, nhúng giao diện GitHub; giao diện gửi yêu cầu load/save qua postMessage kiểm tra origin và token phiên. Chỉ Apps Script gọi DriveApp, không cần API key Google trong GitHub. Khóa phiên bản và UserLock ngăn ghi đè giữa hai cửa sổ. Nếu báo xung đột, sao lưu rồi tải lại. Không hỗ trợ nhiều người cùng sửa một kho chung ở bản này.

## 3. Sử dụng

1. Cấu hình đơn vị cung cấp và nhân viên liên hệ. Dữ liệu người ký/ủy quyền phải nhập theo thông tin thực tế.
2. Thêm khách hàng hoặc nhập Excel/CSV/XML. XLSX đọc sheet đầu; phải có hàng tiêu đề. XML hỗ trợ vùng bên mua NMua/BuyerParty; cấu trúc khác báo lỗi để tránh lấy nhầm bên bán.
3. Chọn mẫu, xem từng đoạn/ô bảng. Điền các chỗ trống bằng trường bên dưới; bấm Lưu nội dung đoạn. Việc sửa đoạn có thể giản lược định dạng run trong đoạn đó, giữ run đầu.
4. Mẫu Connect chứa chỗ trống chưa gắn đầy đủ: đặc biệt ngày/nơi ký, nhà trường, học sinh, ủy quyền và đầu mối. Cấu hình, xem bản xuất, duyệt trước khi dùng thật. Dấu chấm/chỗ trống không phải placeholder sẽ không bị phát hiện tự động.
5. Tick Đã duyệt cấu hình mẫu và Lưu mẫu.
6. Lập hồ sơ, chọn khách hàng, số/ngày hợp đồng, nhập sản phẩm. Nhập đơn giá cho cả gói, chọn VAT thực tế.
7. Chọn các chứng từ. Bộ xuất theo thứ tự mẫu. Bấm Tạo bộ Word/ZIP. Trường placeholder thiếu sẽ chặn xuất.
8. Mở tất cả Word để kiểm tra bố cục, điều khoản và dữ liệu trước khi ký/phát hành. Có thể Save as PDF bằng Word hoặc LibreOffice.
9. Sao lưu định kỳ để giữ bản dữ liệu/mẫu. Token AI không có trong bản sao lưu.

## 4. Cấu hình mẫu

- Trường đơn: `{{customer.name}}`, `{{customer.taxId}}`, `{{customer.budgetCode}}`, `{{provider.name}}`, `{{contract.number}}`, `{{contract.date}}`, `{{contact.name}}`.
- Tổng tiền: `{{contract.total}}`, bằng chữ: `{{contract.totalWords}}`.
- Bảng sản phẩm: ô đầu dòng `{{#products}}{{stt}}`, các ô giữa `{{name}}`, `{{description}}`, `{{unit}}`, `{{unitPriceText}}`, `{{subtotalText}}`, `{{taxText}}`, `{{totalText}}`, ô cuối thêm `{{/products}}`.
- Bảng phụ huynh: `{{#students}}{{stt}}`, `{{parent}}`, `{{name}}`, `{{phone}}`, `{{note}}{{/students}}`. Ô chữ ký để trống, không tự tạo chữ ký.
- Mở/đóng vòng lặp phải nằm trong cùng một dòng bảng. Không hỗ trợ vòng lặp lồng nhau, textbox lặp và trường tính phức tạp.
- Bản gốc không bị sửa. Những template trong gói đã tách bỏ hướng dẫn soạn thảo Mẫu 95.
- Danh sách chức năng HSS là nội dung cố định của gói QLGA mẫu. Đổi sản phẩm thì cần đổi/chọn phụ lục chức năng phù hợp, không chỉ đổi tên và giá.

## 5. AI tùy chọn

**Chế độ kết hợp Apps Script:** không cần dựng server AI riêng. Trong Project Settings → Script Properties thêm `AI_API_KEY`, `AI_MODEL`, và tùy chọn `AI_BASE_URL`. Nút AI sẽ gọi UrlFetchApp qua cầu nối. Khóa thuộc cấu hình dự án, không nhập trên GitHub. Chỉ mở quyền truy cập cho nhóm người được phép dùng khóa và tài liệu này.

**Chế độ GitHub độc lập:**

`server/server.mjs` là API trung gian Node.js, gọi nhà cung cấp tương thích Chat Completions JSON mode. Cần Node 20+; không có dependency npm.

1. Sao chép `server/.env.example` thành `.env` ngoài repo công khai, điền cấu hình.
2. Chạy `node --env-file=.env server/server.mjs` trên máy chủ.
3. Đặt reverse proxy HTTPS. ALLOWED_ORIGIN là origin Pages của bạn, không phải URL Apps Script.
4. Điền URL API và token truy cập trong Cấu hình. Token chỉ dùng phiên hiện tại.
5. Dán văn bản rồi bấm AI để đề xuất thông tin; AI mẫu trả danh sách gợi ý để người dùng gắn trường thủ công.

Adapter hiện dùng token riêng do quản trị cấp; phù hợp thử nghiệm cá nhân. Trước khi nhiều người dùng cần bổ sung đăng nhập, giới hạn lượt gọi, giám sát chi phí và quyền truy cập. Không cài khóa AI vào app.js/Code.gs hoặc GitHub công khai. Chưa có OCR ảnh và PDF trong adapter.

## 6. Giới hạn và kế hoạch tiếp theo

- IndexedDB chỉ lưu trên trình duyệt; chế độ Drive cần mở đúng link Apps Script. Không tự đồng bộ hai chế độ.
- Tải dữ liệu/mẫu một lần khi mở; chỉ lưu khi thao tác Lưu / xuất / xem dữ liệu. Không gọi Drive khi thêm dòng hoặc gõ sản phẩm.
- Khi lưu Drive, client gửi snapshot và bytes mẫu; máy chủ chỉ tạo DOCX nếu hash thay đổi. Mẫu lớn/nhiều mẫu có thể gặp giới hạn Apps Script; nên giữ mẫu dưới 10 MB và dùng ảnh nén.
- Template custom chỉ nhận .docx. .doc phải Save as DOCX trước.
- Bảng kê là CSV, chưa phải Word có số trang. PDF tự động cần worker LibreOffice, kiểm tra font và phân trang.
- Chưa có kho hợp đồng/lịch sử phát hành; dữ liệu hiện tại là bản nháp, kết quả tải về có JSON để lưu vết. Kế tiếp bổ sung lưu hồ sơ theo ID và phiên bản.
- Chưa kiểm thử OAuth và Drive trên tài khoản thật vì chưa có deployment của người dùng. Hàm lưu/tải Drive được kiểm thử mô phỏng; iframe bridge chưa kiểm thử trên deployment thật; phải chạy kiểm tra tại bước triển khai.

## Kiểm thử

Mở bằng web server localhost. Test trình duyệt trong `tests/smoke.cjs` kiểm tra giá, nhập Excel/XML, giữ dữ liệu, lặp bảng Word và xuất ZIP. Xem `TEST_RESULTS.md` cho kết quả lần đóng gói.

## Mã nguồn và thư viện

Mã ứng dụng viết riêng cho bộ hồ sơ này. JSZip trong vendor có giấy phép MIT/GPL; xem vendor/JSZIP-LICENSE.txt. Không chứa khóa API hoặc file khách hàng gốc.
