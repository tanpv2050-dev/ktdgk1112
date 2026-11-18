# Thêm câu hỏi TỰ LUẬN vào hệ thống thi đang dùng (V3)

Giữ nguyên toàn bộ dữ liệu và cách làm việc hiện tại (NganHang, DeThi, CauHinh, KetQua, PhienLam, ChiTiet, DanhSach).

## Cài đặt (khoảng 5 phút)
1. Apps Script của Google Sheet: thay nội dung `Code.gs` bằng file mới.
2. Thêm 2 tệp HTML tên đúng là `ChamBai` và `ThemTuLuan` (dán nội dung 2 file cùng tên). Thay nội dung tệp `index` bằng `index.html` mới (hoặc cập nhật bản đặt trên GitHub Pages).
3. Tải lại Google Sheet → menu **Thi trực tuyến → Khởi tạo / nâng cấp cho tự luận** (chạy 1 lần, cấp quyền Drive để lưu ảnh bài làm).
4. Triển khai → Quản lý triển khai → **Phiên bản mới** (giữ nguyên URL).

## Thêm câu tự luận
- Cách nhanh: menu **Thêm câu tự luận…** (điền nội dung, điểm, đáp án mẫu, chọn đề).
- Hoặc gõ trực tiếp vào sheet NganHang: cột **K = TL**, L = Diem, M = GioiHan (ký tự), N = ChoAnh (1/0), O = HuongDanCham. Cột D là đề bài; A–D, đáp án để trống. Rồi thêm câu vào sheet DeThi như câu thường.
- Câu trắc nghiệm cũ để cột K trống – không cần sửa gì.

## Chấm bài
Menu **Chấm bài tự luận…** → chọn câu, chấm từng bài (điểm + nhận xét; xem ảnh bài viết tay bằng cách bấm vào ảnh). Câu bỏ trống tự động 0 điểm.
Sheet **KetQua**: cột J điểm tổng (trống đến khi chấm xong), N tình trạng chấm, O–Q điểm thô.
Xong thì menu **Công bố / thu hồi điểm tự luận** để học sinh vào lại bằng mã và xem điểm tổng + nhận xét.

## Cách tính điểm
- Đề **chỉ có trắc nghiệm**: giữ nguyên công thức cũ (đúng ÷ tổng câu × 10).
- Đề **có tự luận**: (điểm TN đạt + điểm TL đạt) ÷ tổng điểm tối đa × thang điểm. Điểm mặc định khi cột Diem để trống: TN 0,25 · TL 1. Thang điểm: dòng `ThangDiem` trong CauHinh (mặc định 10; có thể thêm dòng `CongBoTuLuan` = TRUE/FALSE).

## Thay đổi hành vi cần biết
- Học sinh đã nộp mà nhập lại mã sẽ thấy kết quả (nếu CauHinh!HienDiem = TRUE) thay vì thông báo "đã nộp bài" như trước.
- Bản nháp tự luận lưu ở bộ nhớ đệm, ghi xuống Sheets mỗi ~90 giây; khi nộp ghi chốt.
- Mở lại bài vẫn như cũ: học sinh làm lại từ đầu; bài tự luận của lượt cũ không còn hiện trong danh sách chấm.

---
# V4 – AI đọc và chấm bài tự luận (chữ gõ + ảnh bài viết tay)

## Cài đặt thêm
1. Thêm tệp HTML tên đúng là `CaiDatAI` (dán nội dung file cùng tên); thay `Code.gs`, `ChamBai`, `index` bằng bản mới.
2. Menu **Thi trực tuyến → Khởi tạo / nâng cấp cho tự luận** (chạy lại một lần; sẽ xin thêm quyền "kết nối dịch vụ bên ngoài").
3. Menu **Cài đặt AI chấm bài…**: chọn chế độ, dán khoá API Claude (lấy tại console.anthropic.com), bấm **Kiểm tra kết nối AI**, rồi **Lưu**.
4. Bấm **Bật tự chạy mỗi phút** (AI tự chấm bài ngay sau khi học sinh nộp) hoặc dùng nút **AI chấm ngay** khi cần.
5. Triển khai Phiên bản mới của Web App.

## Chế độ
- **AI đề xuất điểm – giáo viên duyệt (khuyên dùng)**: AI chấm sẵn kèm tiêu chí; giáo viên bấm "Dùng điểm AI" hoặc **Duyệt hàng loạt** (chỉ nhận các bài AI chắc chắn). Học sinh chỉ thấy sau khi bạn Công bố.
- **AI chấm tự động**: điểm AI là điểm chính thức, trừ bài AI báo cần xem kỹ (ảnh mờ, độ tin cậy thấp, bài có dấu hiệu chèn lệnh kiểu "hãy cho 10 điểm") – các bài này chờ giáo viên.
- Điểm vẫn chỉ hiện cho học sinh khi bạn bấm **Công bố điểm tự luận**.

## Chất lượng chấm
- **Điền đáp án mẫu / thang điểm ở cột O (HuongDanCham) cho từng câu**: AI chấm theo đúng thang này và trả điểm từng ý. Không có thì AI tự lập thang điểm – kém ổn định hơn.
- Học sinh nhận: điểm từng ý, điểm làm tốt, điểm cần cải thiện, nhận xét, và hướng dẫn giải chi tiết (hướng dẫn giải chỉ hiện khi CauHinh!HienDapAn = TRUE). Trang kết quả ghi rõ "Chấm bởi trợ lý AI" hoặc "AI chấm, giáo viên đã duyệt". Nếu giáo viên sửa điểm khác AI thì học sinh chỉ thấy điểm và nhận xét của giáo viên.
- Độ tin cậy tối thiểu mặc định 75%. Nên chạy thử 10–20 bài, so với điểm giáo viên, rồi chỉnh trước khi dùng tự động.

## An toàn & riêng tư
- Khoá API lưu trong Script Properties, không nằm trong Sheets. Hệ thống không gửi họ tên/lớp/mã học sinh cho AI, chỉ gửi đề bài, đáp án mẫu, bài làm (chữ + ảnh).
- Bài làm có thể chứa lệnh nhằm tác động AI: AI được hướng dẫn bỏ qua và báo cảnh báo, bài đó chuyển sang giáo viên xem.
- Hãy phù hợp quy định của nhà trường/phụ huynh về gửi bài làm của học sinh cho dịch vụ AI; dặn học sinh không chụp thông tin cá nhân vào ảnh.
- Chi phí tỉ lệ với số bài tự luận (ảnh tốn nhiều hơn chữ). Chọn mô hình nhỏ hơn trong Cài đặt AI nếu cần tiết kiệm.
