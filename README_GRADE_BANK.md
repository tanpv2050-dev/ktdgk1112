# KTDGGK-V4 – Grade Bank + Hybrid Word Import (V1.0)

## 1. Phạm vi tích hợp

Bản tích hợp bổ sung module quản lý ngân hàng theo khối 10/11/12 và nhập đề Word theo quy trình Hybrid:

1. Lưu tệp `.docx` gốc vào Google Drive.
2. Phân tích văn bản trong cửa sổ quản trị (Mammoth.js chạy trên trình duyệt).
3. Lưu câu hỏi vào sheet `ImportDraft`, chưa tham gia kỳ thi.
4. Giáo viên kiểm tra/chỉnh JSON, nhập mã đề và bấm **Duyệt và nhập ngân hàng**.
5. Chỉ sau khi duyệt, câu hỏi mới được ghi vào `NganHang`, liên kết vào `DeThi` và có metadata khối/môn/đợt.

Các cột hiện có A:O của `NganHang` và A:D của `DeThi` được giữ nguyên. Các sheet mới:

- `ExamDocuments`: hồ sơ Word gốc, hash, metadata, trạng thái.
- `ImportDraft`: dữ liệu câu hỏi chờ duyệt.
- `ExamSets`: mã đề và phân loại Grade.
- `QuestionMeta`: metadata câu hỏi, hash, phiên bản.
- `QuestionUsageHistory`: lịch sử câu hỏi đã được dành cho từng ca thi.

## 2. Cài đặt

**Khuyến nghị:** sao lưu Google Spreadsheet và dự án Apps Script trước khi thay thế.

1. Mở dự án Apps Script đang gắn với Google Sheet `ktdggk-v4`.
2. Thay toàn bộ nội dung `Code.gs` bằng `Code.gs` trong gói này.
3. Tạo file HTML mới tên chính xác `QuanLyDe` (không thêm `.html` trong tên Apps Script) và dán nội dung `QuanLyDe.html`.
4. Giữ nguyên các file HTML hiện có: `index`, `ChamBai`, `ThemTuLuan`, `CaiDatAI`.
5. Lưu dự án, quay lại Google Sheets và tải lại trang.
6. Mở menu **Thi trực tuyến → Ngân hàng đề theo khối / Nhập Word Hybrid…**. Lần đầu, module tự tạo các sheet metadata.
7. Kiểm tra quyền Google Drive khi được yêu cầu; tệp Word gốc được lưu trong thư mục `KTDGGK_Exam_Word_Originals`.
8. Triển khai Web App phiên bản mới nếu đang dùng URL `/exec` để học sinh làm bài. URL triển khai có thể giữ nguyên.

## 3. Quy trình vận hành

### Nhập đề

- Chọn khối, môn, học kỳ, năm học, đợt kiểm tra, tên đề.
- Chọn tệp `.docx`, bấm **Đọc Word và phân tích câu hỏi**.
- Kiểm tra JSON: mỗi câu trắc nghiệm phải có đủ A/B/C/D và đáp án đúng; câu tự luận phải có rubric/hướng dẫn chấm phù hợp.
- Bấm **Lưu Word gốc + bản nháp**.
- Nhập mã đề duy nhất, ví dụ `11_TIN_HKI_2026_GK01`, rồi bấm **Duyệt và nhập ngân hàng**.

### Thiết lập số câu

Giá trị `CauHinh!SoCau` hiện vẫn là số câu dùng chung của hệ thống cũ. Khi nhập bộ đề có số câu khác cấu hình, cần điều chỉnh cấu hình trước kỳ thi hoặc đảm bảo mỗi mã đề có đủ số câu theo cấu hình. Không nên đưa mã đề chưa đủ câu vào ca thi chính thức.

### Tổ chức nhiều ca – chống lặp

Đối với mã đề được nhập qua Grade Bank:

1. Trước ca đầu tiên, chọn **Thi trực tuyến → Thiết lập mã ca thi hiện tại…** và đặt mã, ví dụ `CA_2026_11A1_SANG`.
2. Giữ nguyên mã ca cho toàn bộ học sinh trong cùng ca và trong suốt thời gian ca thi diễn ra.
3. Trước ca tiếp theo trong cùng đợt kiểm tra môn học, đổi sang mã mới, ví dụ `CA_2026_11A2_CHIEU`.
4. Hệ thống ghi nhận các câu hỏi được dành cho ca đầu; ca sau sẽ bị chặn nếu dùng lại câu hỏi đã có trong lịch sử của cùng khối/môn/học kỳ/đợt/năm học.
5. Vì vậy, các ca khác nhau phải được chuẩn bị bằng các mã đề có bộ câu hỏi không trùng nhau.

Mã ca được ghi vào `CauHinh` dưới khóa `MaCaThi`. Không thay đổi mã ca khi học sinh trong ca vẫn đang làm bài.

## 4. Giới hạn parser hiện tại

- Chỉ nhận `.docx`; tệp `.doc` phải chuyển đổi sang `.docx`.
- Nhận diện tiêu đề dạng `Câu 1.` / `Câu 2:` và phương án dạng `A.`, `B.`, `C.`, `D.`.
- Parser lấy văn bản; không đảm bảo tự động trích xuất chính xác hình vẽ, ảnh, bảng phức tạp, công thức toán, sơ đồ, định dạng nhiều cột hoặc câu Đúng/Sai. Giáo viên phải đối chiếu Word gốc.
- Câu không nhận diện được bốn phương án sẽ được phân loại tạm là tự luận để tránh tự tạo đáp án trắc nghiệm sai; cần giáo viên chỉnh `type` trong JSON.
- Thư viện Mammoth được tải từ CDN jsDelivr nên cửa sổ nhập Word cần kết nối Internet.
- Module không tự động tạo nhiều mã đề hoán vị. Mỗi lần duyệt tạo một mã đề; các mã đề khác nhau phải có bộ câu hỏi khác nhau nếu sử dụng cho các ca liên tiếp.

## 5. Bảo toàn hệ thống cũ

- Không đổi tên sheet cũ và không dịch chuyển cột hiện tại.
- Không thêm API nhập/duyệt vào danh sách `API_OK`; chức năng quản trị dùng token phiên `gv_`.
- Tệp Word và câu hỏi nháp không được đưa vào đề thi cho đến khi giáo viên duyệt.
- Các đề cũ không có metadata Grade vẫn hoạt động theo cơ chế cũ.

## 6. Kiểm thử bắt buộc trước khi dùng thật

1. Sao lưu bản Google Sheet và Apps Script.
2. Nhập một tệp Word mẫu nhỏ, kiểm tra sheet mới và tệp Drive.
3. Dùng JSON có một câu TN và một câu TL; thử lỗi thiếu đáp án A/B/C/D để xác nhận hệ thống chặn.
4. Duyệt đề và kiểm tra dòng trong `NganHang`, `DeThi`, `ExamSets`, `QuestionMeta`.
5. Thử lớp 11 chọn mã đề khối 12: hệ thống phải từ chối.
6. Đặt `MaCaThi` cho ca 1, cho một học sinh bắt đầu; thử đổi ca 2 nhưng dùng mã đề trùng câu: hệ thống phải chặn.
7. Đổi ca 2 sang bộ câu hỏi không trùng: hệ thống cho phép.
8. Thử một đề cũ không có metadata để xác nhận luồng cũ vẫn hoạt động.
9. Kiểm tra làm bài, nộp bài, chấm tự luận, AI (nếu bật), công bố điểm.

**Không đưa vào kỳ thi chính thức trước khi hoàn thành các bước kiểm thử trên bằng tài khoản giáo viên và học sinh thử nghiệm.**
