# 03 — Individual Reflection

## Đóng góp của tôi trong nhóm

| Hoạt động | Tôi đã làm gì? | Kết quả / ảnh hưởng |
|---|---|---|
| Hội tụ candidate | Tham gia điều phối việc so sánh các candidate theo actor, workflow, evidence, impact, khả năng pilot và mức phù hợp Rule/Workflow/Agent. | Nhóm chọn Aggregator & Centralized Deadline Tracker dựa trên pain lặp lại, workflow rõ và khả năng đo trong pilot. |
| Challenge và validation | Đặt trọng tâm vào bằng chứng của baseline, quyền truy cập dữ liệu và câu hỏi liệu Rule/process đã đủ hay chưa. | Nhóm không xem các con số ước lượng là kết quả; Problem Statement được bổ sung baseline 7 ngày, audit lỗi và phạm vi pilot nhỏ. |
| Gom cụm / chọn candidate | Cùng nhóm so sánh các candidate theo actor, workflow, evidence, impact, khả năng pilot và mức phù hợp Rule/Workflow/Agent. | Nhóm chọn Aggregator & Centralized Deadline Tracker thay vì chọn theo độ “AI” hoặc theo chuyên môn cá nhân. |
| Validation / research | Góp ý cách ghi rõ giả định, nguồn gốc dữ liệu và giới hạn của các số liệu; đối chiếu các giải pháp hiện có để nhận ra phần API/filter đã có thể dùng Rule. | Nhóm xác định khoảng trống phù hợp cho AI là xử lý text phi cấu trúc, không phải thay thế toàn bộ workflow. |
| Workflow nhóm | Rà soát bước handoff từ nguồn thông báo đến candidate task và điểm người dùng cần approve/edit/dismiss. | Future workflow có source link, fallback khi AI không chắc và controlled write sau phê duyệt. |
| Problem Statement và boundary | Chuẩn hóa actor, workflow, bottleneck, metric và các điều không làm: không đọc nguồn chưa cấp quyền, không tự gửi tin nhắn/đổi deadline/tạo task chính thức. | Scope rõ hơn, giảm rủi ro privacy và tránh biến solution thành một agent tự trị quá sớm. |
| Rule / Workflow / Agent và decision | Lập luận rằng Rule phù hợp với sync, filter, dedup và write guard; AI chỉ trích xuất candidate task; người dùng quyết định trước khi ghi. | Nhóm chọn **Workflow** và quyết định **Go với pilot nhỏ**, còn Agent tự trị là **Not Yet**. |

## Tôi dùng AI như thế nào

| Phase | Tôi dùng AI để làm gì? | AI hữu ích ở đâu? | AI sai / hời hợt ở đâu? | Tôi sửa gì bằng nhận định của mình? |
|---|---|---|---|---|
| Hội tụ candidate | Gợi ý khung so sánh các candidate theo workflow, impact và feasibility. | Giúp nhóm nhìn rõ các tiêu chí lựa chọn thay vì chọn ý tưởng nghe “AI” nhất. | AI có thể chấm điểm cao cho một ý tưởng nhưng không có evidence từ người dùng. | Nhóm ưu tiên pain được nhiều thành viên nêu, workflow lặp lại và khả năng pilot nhỏ. |
| Validation | Rà soát giả định baseline và cách diễn đạt metric. | Nhắc nhóm phân biệt số liệu tự ước lượng với số liệu đã đo. | AI dễ biến 15–30 phút/ngày thành kết quả chắc chắn. | Nhóm ghi baseline là giả thuyết, thêm 7 ngày đo baseline và audit lỗi trước khi kết luận. |
| Workflow | Gợi ý cách tách bước deterministic, bước AI và human review. | Làm rõ điểm AI nên nằm ở trích xuất text phi cấu trúc, còn sync/filter/dedup nên là Rule. | Có xu hướng đề xuất tự động hóa nhiều hơn mức pilot cần thiết. | Tôi giữ read-only sync, approval bắt buộc và fallback về review thủ công. |
| Research | Hỗ trợ lập danh sách hướng cần kiểm tra và tóm tắt câu hỏi so sánh các nền tảng. | Giúp tìm đúng câu hỏi: tool hiện có giải quyết phần nào và còn khoảng trống nào. | Tóm tắt có thể bỏ qua khác biệt về OAuth scope, policy hoặc giới hạn quyền truy cập. | Nhóm chỉ giữ các link chính thức, ghi rõ rủi ro quyền và không coi tính năng của tool là bằng chứng cho impact của nhóm. |
| Problem Statement | Phản biện các field còn mơ hồ, nhất là metric và boundary. | Nhắc nhóm liên kết problem → workflow → metric → boundary. | Có thể đề xuất metric đẹp nhưng không có baseline hoặc ground truth. | Tôi bổ sung cách đo cụ thể: baseline 7 ngày, pilot 14 ngày, audit source link và write action. |
| Rule / Workflow / Agent | So sánh mức tự động hóa và các failure mode. | Làm rõ Agent không phải lựa chọn mặc định. | Đề xuất Agent nghe linh hoạt nhưng kéo theo scope, permission và rủi ro khó kiểm soát. | Nhóm hạ về Workflow có đường đi cố định và human approval. |
| Decision | Dùng AI để kiểm tra xem lập luận Go/Not Yet có nhất quán không. | Giúp phát hiện quyết định cần đi kèm điều kiện pilot và cách đo. | AI không thể thay nhóm đánh giá pain thật hay chấp nhận rủi ro. | Tôi giữ quyết định Go có điều kiện: chỉ pilot hai nguồn, read-only và không có write action chưa duyệt. |

## Bài học của tôi

Điều tôi học rõ nhất là một vấn đề tốt cho AI không nhất thiết là vấn đề kỹ thuật phức tạp nhất. Khi nhóm so sánh các candidate, Aggregator & Centralized Deadline Tracker nổi bật vì pain lặp lại hằng ngày, actor rõ, workflow có hai bottleneck cụ thể — discovery và normalization — và có thể kiểm chứng bằng pilot. Vì vậy, chọn candidate là chọn bài toán có bằng chứng và workflow tốt hơn, không phải chọn ý tưởng nghe “AI” nhất.

Nhóm có lúc có nguy cơ solution-first, nhất là khi nghĩ đến một trợ lý có thể tự đọc mọi nền tảng rồi tự tạo task. Việc vẽ workflow khiến nhóm thấy phần lớn luồng đi là cố định: đồng bộ nguồn, lọc dữ liệu có cấu trúc, gộp trùng và ghi log. AI chỉ thực sự có ích ở bước đọc thông báo ngôn ngữ tự nhiên để tạo *candidate task*. Khi một deadline hoặc action còn mơ hồ, hệ thống phải nói “chưa xác định” và trả lại quyết định cho người dùng, thay vì tự suy đoán.

Tôi cũng thay đổi cách nhìn về metric. Con số 15–30 phút/ngày và 3–5 lượt kiểm tra lại trong group report mới là baseline ước tính, không phải bằng chứng đủ mạnh để hứa hẹn kết quả. Đóng góp quan trọng của tôi là nhấn mạnh cần phân biệt giả thuyết với dữ liệu đã đo: log baseline, ground truth, precision/recall và audit các write action. Nhờ vậy, quyết định Go của nhóm không phải là cam kết sản phẩm thành công, mà là quyết định thử một pilot có kiểm soát.

Khó nhất khi viết Problem Statement là giữ đồng thời hai điều: mô tả đủ cụ thể để đo được, nhưng không lẫn giải pháp vào phần problem. Tôi học được rằng boundary không phải phần phụ ở cuối bài; nó quyết định AI được phép làm gì, ai chịu trách nhiệm và pilot có an toàn hay không.

## Nếu làm lại

Tôi sẽ làm hai việc sớm hơn: (1) thu time-log hoặc phỏng vấn nhanh nhiều người dùng trước khi chấm điểm candidate, và (2) challenge mạnh hơn giả định rằng mọi notification đều nên trở thành task. Tôi cũng sẽ yêu cầu một tập tin nhắn được gắn nhãn nhỏ ngay từ đầu để đo precision/recall của candidate task, đặc biệt với deadline quan trọng. Như vậy, nhóm có thể quyết định Go / Not Yet dựa trên evidence tốt hơn thay vì chủ yếu dựa vào trải nghiệm tự báo cáo.
