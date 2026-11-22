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

---

## V5 – Nhập đề từ Word (nhận đáp án theo màu) + câu Đúng/Sai

### 1. Cài đặt
1. Chép đè `Code.gs`, `index.html`; **thêm file mới `NhapDeWord.html`** (Apps Script: Thêm tệp → HTML → đặt tên `NhapDeWord`, dán nội dung).
2. Triển khai lại Web App (Phiên bản mới). Mở lại Google Sheets → menu **Thi trực tuyến → Nhập đề từ file Word (nhận đáp án theo màu)…**

### 2. Cách nhập đề
- Chọn file `.docx`, nhập **Mã đề** (vd `12A`) và **Bài** (số), kiểm tra bảng xem trước (câu có cảnh báo được tô vàng), sửa đáp án nếu cần rồi bấm **Lưu**.
- Quy tắc nhận diện:
  - Câu mới bắt đầu bằng `Câu 1.` / `Câu 1:`; `PHẦN I` = trắc nghiệm, `PHẦN II` = đúng/sai, `PHẦN III` = tự luận (không có tiêu đề phần thì tự đoán theo cấu trúc).
  - **Trắc nghiệm**: phương án A–D (viết dạng đoạn văn, nằm trong bảng, hay nhiều phương án trên một dòng đều được); phương án nào có **chữ tô màu** (đỏ, xanh…) là đáp án. Chữ đen/xám/trắng không tính.
  - **Đúng/Sai**: ý a–d; ý nào **tô màu = Đúng**, ý không tô = Sai.
  - Ghi chú dạng `(H.Đ)` tự được bỏ khỏi nội dung.
  - Câu không có phương án → tự luận. Có “(2 điểm)” trong nội dung thì lấy làm điểm câu.
- Cảnh báo cần xem: không có / có nhiều hơn một phương án tô màu; thiếu ý; câu có hình ảnh hoặc công thức (chỉ nhập phần chữ).
- Nhập lại cùng một file không bị nhân đôi: câu trùng nội dung được dùng lại.
- ID tự đặt: `TN001…`, `DS001…`, `TL001…`.

### 3. Câu Đúng/Sai (Loai = `DS` ở cột K của NganHang)
- Cột D = lời dẫn, E–H = 4 ý a–d, **cột I = đáp án dạng 4 ký tự D/S** (vd `SSDD`: a Sai, b Sai, c Đúng, d Đúng). Cột L = điểm cả câu (để trống = 1).
- Cách tính (theo quy chế): đúng 1 ý = 10%, 2 ý = 25%, 3 ý = 50%, 4 ý = 100% điểm câu.
- Điểm bài = tổng điểm đạt / tổng điểm tối đa × ThangDiem (nếu đề có câu Đúng/Sai hoặc tự luận; đề thuần trắc nghiệm giữ công thức cũ). Mặc định mỗi câu TN = 0,25 điểm khi cột L trống.
- Ý của câu Đúng/Sai luôn giữ nguyên thứ tự a–d (không xáo).

---

# V6 – Thi đa môn · Kỳ thi · Đăng nhập theo Kì thi + Lớp + Môn

## 1. Cài đặt (thay toàn bộ, ~10 phút)
1. **Sao lưu** file Google Sheets (Tệp → Tạo bản sao).
2. Trong Apps Script: **xoá toàn bộ** nội dung `Code.gs` cũ, dán `Code.gs` mới. Thêm/ghi đè các file HTML: `index`, `NhapDeWord`, `QuanLyDe`, `QuanLyKyThi`, `TaiKhoanHS`, `ChamBai`, `ThemTuLuan`, `CaiDatAI`.
   (Lưu ý: dán đúng **một** bản `Code.gs`. Bản cũ của thầy/cô có nhiều hàm bị định nghĩa trùng nên dễ lỗi.)
3. Quay lại Google Sheets, tải lại trang → menu **Thi trực tuyến** → **Khởi tạo / nâng cấp lên V6** (chạy 1 lần, chạy lại vẫn an toàn). Hệ thống sẽ:
   - tạo sheet `KyThi`, `DanhMucMon`; chuẩn hoá `QuyDinhMaDe`, `DeThi`;
   - chuyển các dòng `DeThi` bị lệch cột (kiểu `MaDe | TenDe | DanhSachCau | Khoi | MonHoc`) thành dòng chuẩn `MaDe | Stt | IDCau | HoanVi`;
   - suy ra khối/môn của từng mã đề cũ (từ chữ số đầu của mã, từ dòng cũ hoặc từ cột `Nhom` "Khối 12 - Tin học" của câu hỏi).
4. **Triển khai → Quản lý bản triển khai → Chỉnh sửa → Phiên bản mới → Triển khai.**

## 2. Quy trình thầy/cô dùng
| Bước | Menu | Việc làm |
|---|---|---|
| 1 | Tài khoản học sinh | Dán danh sách (Mã · Họ tên · Lớp), tạo mật khẩu, in phiếu phát cho học sinh |
| 2 | Nhập đề từ file Word | Chọn file, chọn **Khối + Môn**, chọn "Tự sinh mã đề mới" (hoặc thêm vào đề đã có) → xem trước → Lưu |
| 3 | Quản lý Mã đề | Nhân bản thành nhiều mã đề (đảo câu, hoán vị phương án), xem/bỏ câu, lưu trữ, xoá |
| 4 | Quản lý Kỳ thi | Tạo kỳ thi (VD GK1-2627), chọn khối, **nhiều môn**, lớp, giờ mở/đóng, thời lượng, mã đề |
| 5 | Chấm bài tự luận | Lọc theo Kỳ thi/Môn; Công bố điểm tự luận theo từng kỳ-môn ở trang Quản lý Kỳ thi |

## 3. Học sinh đăng nhập thế nào
Chọn **Kì thi → Lớp → Môn**, nhập **Mã đăng nhập + Mật khẩu** rồi bấm Bắt đầu. Danh sách môn chỉ hiện các môn của kỳ thi dành cho lớp đó; môn chưa đến giờ/chưa có đề bị khoá và có ghi lý do. Sai mật khẩu 5 lần: khoá mã đó 10 phút.
- Mỗi học sinh có **một phiên làm bài riêng cho từng Kì thi + Môn** (làm Toán không ảnh hưởng Tin học). Mở lại bài chỉ mở đúng môn.
- Khi KyThi trống, trang thi vẫn chạy kiểu cũ (không chọn kỳ/môn).

## 4. Mã đề 4 số
`[khối][môn][thứ tự 2 số]` – chữ số đầu: 1 = khối 10, 2 = khối 11, 3 = khối 12; chữ số thứ hai = mã môn trong `DanhMucMon`; hai số cuối = thứ tự. Ví dụ **3101** = Khối 12 · Tin học · đề 01. Môn thật của mã đề luôn lấy từ `QuyDinhMaDe`, nên thêm môn mới (kể cả khi trùng chữ số) không làm lẫn đề.

## 5. Hình ảnh trong đề
Hình ở câu hỏi, ở từng phương án A–D hoặc từng ý a–d được tách khi nhập Word, lưu vào Drive riêng tư (`ThiTrucTuyen_AnhTrongDe`) và ghi ID vào `NganHang` cột P (ảnh đề bài) và Q (ảnh phương án, JSON). Học sinh xem ảnh qua máy chủ, chỉ ảnh thuộc đề của mình. Công thức Word được đổi thành chữ (`(a)/(x^2)`, `√(x)`…). Hình EMF/WMF trình duyệt không đọc được: hãy chèn lại thành PNG/JPG.
**Sửa lỗi "đáp án bằng hình bị trống"**: bản cũ ghi ID ảnh phương án vào cột N nhưng không gửi cho trang thi. V6 gửi đủ, vẫn đọc được dữ liệu ảnh cũ ở cột M/N.

## 6. Bảo mật
- Mật khẩu giáo viên (`CauHinh!MatKhauGV`) **không nên nằm trong file chia sẻ**: hãy đổi ngay nếu đã gửi file cho người khác.
- Mật khẩu học sinh nằm ở `DanhSach!MaPIN`; không chia sẻ sheet này cho học sinh.
