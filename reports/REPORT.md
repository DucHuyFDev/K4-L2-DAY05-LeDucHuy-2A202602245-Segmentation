# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: chưa cung cấp
- Ngày / CVAT local: 2026-09-18 / CVAT local
- Công cụ đã dùng: CVAT Polygon và Brush; kiểm tra cấu trúc bằng `scripts/inspect_submissions.py`

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: chưa có thông tin xác nhận về object đầu tiên.
- Class và quy tắc tôi dùng để chọn biên: với object xe, chọn đúng class vehicle và vẽ theo phần nhìn thấy; không đoán phần bị che.
- Nếu dùng gợi ý sau đó: chưa có thông tin về gợi ý tự động hoặc hành động sửa.
- Nếu không dùng gợi ý: chưa có thông tin xác nhận.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: đóng gói tier, tên file trong `submissions/`.
- Lỗi thuộc loại: khác.
- Bằng chứng tôi nhìn thấy: hai file được đặt tên `easy_segment.zip` và `medium_segment.zip`, trong khi hợp đồng yêu cầu `easy_semantic.zip` và `medium_instance.zip`.
- Quy tắc và hành động sửa: đổi tên đúng theo mã task, giữ nguyên nội dung ZIP.
- Sau sửa đã Save và export lại chưa? không cần export lại; đã đổi tên và chạy QC lại, cả hai đều `OK`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): chưa có metric hoặc điểm tự đánh giá. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 | `cp1_holes`, chiếc motorcycle phía trước | Khe, nan bánh và khoảng trống trong khung xe có thể bị hiểu là lỗ; quy tắc checkpoint yêu cầu giữ chúng trong mask | Gán một mask `motorcycle` theo phần xe nhìn thấy và không khoét các khe bên trong. |
| 2 | `cp2_slice`, các xe đỗ sát nhau | Các xe cùng class đứng gần nhau có thể bị gộp thành một vùng | Tách mỗi xe thành một annotation `car` riêng, không gộp dù cùng class. |
| 3 | `cp5_occlusion`, người che một phần motorcycle | Có thể tách phần xe ở hai phía người thành hai object hoặc tô nối qua người | Giữ các phần nhìn thấy thuộc cùng một instance `motorcycle`, không tô lên `person` và không đoán phần bị che. |
