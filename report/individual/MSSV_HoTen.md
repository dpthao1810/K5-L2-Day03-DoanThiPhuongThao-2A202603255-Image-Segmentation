# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603255
- Ngày: 12/11/2026
- Công cụ đã dùng: cvat

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | chưa làm | 0 / 1 | 3 |
| cp4_curb | chưa làm | 0 / 1 | 3 |
| cp6_coverage | chưa làm | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: tôi chọn object đầu tiên xuất hiện trong ảnh và tự thực hiện annotation bằng công cụ Polygon trên CVAT, trước khi xem bất kỳ đề xuất tự động nào. Object nằm ở vị trí bên phải ảnh.
- Class và quy tắc tôi dùng để chọn biên: Tôi xác định class của object dựa trên đặc điểm nhận dạng trực quan. Khi vẽ mask, tôi bám theo đường viền thực tế của object, chỉ bao gồm các pixel thuộc về object và không lấy phần nền xung quanh. Với những vùng có ranh giới chưa rõ, tôi ưu tiên đường biên quan sát được thay vì tự suy đoán hoặc mở rộng mask quá mức. Mỗi object được tạo thành một instance riêng, kể cả khi có những object khác cùng class ở gần đó.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Gợi ý tự động có một số vùng biên phù hợp với object, tuy nhiên tôi vẫn kiểm tra lại toàn bộ mask. Tôi giữ những vùng được xác định chính xác và chỉnh sửa các phần bị thừa hoặc thiếu để mask khớp hơn với đường viền thực tế. Việc chỉnh sửa dựa trên nguyên tắc không bao gồm background và không bỏ sót phần object có thể quan sát được.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task medium_instance
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Biên
- Bằng chứng tôi nhìn thấy: Khi kiểm tra mask, tôi nhận thấy đường biên của object chưa khớp hoàn toàn với hình dạng thực tế. Một phần mask bị lệch ra ngoài object và bao gồm một số pixel thuộc background, khiến vùng segmentation chưa chính xác.
- Quy tắc và hành động sửa: Tôi đối chiếu mask với ảnh gốc, xác định lại ranh giới giữa object và background, sau đó chỉnh sửa Polygon để loại bỏ phần background bị lấy thừa và giữ lại toàn bộ phần object có thể quan sát được. Tôi cũng kiểm tra để đảm bảo object vẫn được giữ thành một instance riêng, không bị gộp với các object khác cùng class.
- Sau sửa đã Save và export lại chưa? Chưa sửa được do lỗi công cụ

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 Ảnh trong medium_instance, vùng hai vật thể cùng class nằm gần hoặc tiếp xúc nhau.| Có thể gộp hai vùng thành một mask vì chúng nằm sát nhau, hoặc tách thành hai instance riêng biệt. | Dựa vào đường viền và đặc điểm nhận dạng của từng vật thể. Hai vật thể khác nhau về mặt thực tế cần có mask riêng, dù cùng class. | Tôi quyết định tách thành hai instance riêng để đảm bảo không gộp nhầm các đối tượng cùng class. |
| 2 Ảnh trong cp1_holes, vùng kính hoặc khe nằm bên trong vật thể.| Có thể xem vùng kính/khe là phần background cần loại khỏi mask, hoặc giữ chúng nằm trong mask của vật thể. | Dựa trên quy tắc checkpoint: kính và khe nằm trong mask vật, không khoét tùy tiện. | Tôi quyết định giữ vùng kính/khe trong mask theo hướng dẫn checkpoint. |
| 3 Ảnh trong cp5_occlusion, vùng một vật thể bị vật khác che khuất.| Có thể chỉ gán phần nhìn thấy hoặc suy đoán và vẽ toàn bộ hình dạng của vật thể phía sau. | Dựa vào đường viền quan sát được và quy tắc một vật thể bị che vẫn được tính là một instance, không chia thành nhiều instance. | Tôi giữ một instance duy nhất cho vật thể bị che. Tôi muốn hỏi coach liệu checkpoint yêu cầu visible mask hay amodal mask đối với phần bị che khuất hoàn toàn. |
