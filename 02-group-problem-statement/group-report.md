# Group Report — Day 02

**Candidate problem được chọn:** Aggregator & Centralized Deadline Tracker

**Mức giải pháp:** Workflow — Rule để đồng bộ/lọc dữ liệu, AI để trích xuất thông tin phi cấu trúc, người dùng xác nhận trước khi ghi

**Quyết định:** Go với pilot nhỏ; Not Yet đối với Agent tự trị và tích hợp toàn bộ nền tảng

## Thành viên nhóm

| STT | Họ và tên | Mã học viên | Vai trò trong nhóm |
|---:|---|---|---|
| 1 | Trần Nguyễn Mỹ Anh | 2A202601019 | Điều phối, chuẩn hóa Problem Statement và boundary |
| 2 | Lương Quốc Khánh | 2A202601713 | Problem owner, baseline, metric và kế hoạch validation |
| 3 | Hoàng Đức Anh | 2A202601223 | Workflow trước/sau và đánh giá khả thi kỹ thuật |
| 4 | Nguyễn Thu Huyền | 2A202601027 | Research phương án thay thế, risk và human review |

---

# 02 — Group Problem Statement

## 1. Group convergence

### 1.1. Đầu vào từ bốn bài cá nhân

Mỗi thành viên đưa ba Problem Cards vào vòng hội tụ, tổng cộng 12 candidates.

| Thành viên | Candidate 1 | Candidate 2 | Candidate 3 |
|---|---|---|---|
| Trần Nguyễn Mỹ Anh | BEV Vision Debugging Tool | Triage failure case theo metric | Calibration Alignment Check |
| Lương Quốc Khánh | Sàng lọc paper nghiên cứu | Theo dõi thí nghiệm AI | Theo dõi task và deadline đa nền tảng |
| Hoàng Đức Anh | Daily Standup Reminder & Auto-Drafting | Tóm tắt yêu cầu Lab/Assignment | Aggregator & Centralized Deadline Tracker |
| Nguyễn Thu Huyền | Khảo sát và tổng hợp paper | Sàng lọc dataset lớn | Nhắc lịch daily và lịch họp định kỳ |

### 1.2. Gom cụm vấn đề

| Cluster | Candidate examples | Pattern chung |
|---|---|---|
| Task, deadline và follow-up | Deadline Tracker, Daily Standup Reminder, lịch họp định kỳ | Thông tin hành động nằm rải rác; người dùng phải nhớ, kiểm tra và nhập lại |
| Research discovery và synthesis | Sàng lọc paper, Literature Review, sàng lọc dataset | Tìm, đọc và chuẩn hóa lượng lớn thông tin trước khi ra quyết định |
| Experiment và debugging | BEV Debugging, failure-case triage, calibration check, experiment tracking | Log, metric và artifact kỹ thuật nằm ở nhiều nơi, khó truy vết |
| Learning và document processing | Tóm tắt lab/assignment, meeting notes | Đọc tài liệu dài rồi chuyển thành checklist hoặc nội dung hành động |

### 1.3. Nhật ký pitch và challenge

| Candidate | Tín hiệu ủng hộ từ bài cá nhân | Challenge chính | Kết luận sau challenge |
|---|---|---|---|
| Aggregator & Centralized Deadline Tracker | Hai thành viên độc lập mô tả cùng pain; tần suất hằng ngày; baseline ước tính 15–30 phút/ngày | Vấn đề do thông tin phân tán hay do chưa có quy trình quản lý thống nhất? API và quyền riêng tư có làm scope quá lớn không? | Giữ candidate nhưng thu hẹp MVP còn hai nguồn, read-only và bắt buộc xác nhận |
| Sàng lọc/Literature Review paper | Hai thành viên gặp pain; tác động thời gian lớn | Chất lượng shortlist và hallucination được đo thế nào? | Có giá trị nhưng quality metric khó xác nhận hơn trong pilot ngắn |
| Daily Standup Reminder | Workflow rõ, lặp lại hằng ngày, dễ làm pilot | Reminder cố định và template có đủ không; có thật sự cần AI không? | Phần lớn pain có thể giải bằng Rule, impact hẹp hơn Deadline Tracker |
| BEV Vision Debugging | Bottleneck và safety boundary rõ | Baseline 90–120 phút là ước lượng; domain hẹp và cần dữ liệu/công cụ chuyên biệt | Giữ cho bài toán chuyên ngành khác; không chọn làm bài chung |

### 1.4. Shortlist và score

Thang điểm cho mỗi tiêu chí: 1–5. Điểm “Pain có evidence” phản ánh bằng chứng hiện có trong bốn repository, không coi ước lượng cá nhân là khảo sát diện rộng.

| Candidate | Actor rõ | Workflow rõ | Pain có evidence | Impact đo được | Làm pilot nhỏ | So sánh R/W/A | Nhóm hiểu domain | Tổng |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Aggregator & Centralized Deadline Tracker | 5 | 5 | 4 | 5 | 4 | 5 | 5 | **33** |
| Sàng lọc/Literature Review paper | 5 | 5 | 4 | 4 | 4 | 4 | 5 | **31** |
| Daily Standup Reminder | 5 | 5 | 4 | 4 | 5 | 4 | 4 | **31** |
| BEV Vision Debugging | 5 | 5 | 3 | 4 | 3 | 5 | 4 | **29** |

Nhóm chọn: **Aggregator & Centralized Deadline Tracker**.

Vì sao chọn:

- Hai thành viên độc lập ghi nhận cùng một pain trên các tập nền tảng khác nhau.
- Workflow lặp lại hằng ngày, bottleneck và điểm can thiệp cụ thể.
- Có baseline ban đầu về thời gian và số lượt kiểm tra lại.
- Có thể tách rõ phần Rule, phần AI và phần người dùng phải quyết định.
- Có thể kiểm chứng bằng pilot read-only trước khi xin quyền ghi hoặc tích hợp sâu.

Vì sao chưa chọn các candidate khác:

- **Literature Review:** impact lớn nhưng khó đánh giá chất lượng shortlist và độ đúng của tổng hợp trong thời gian ngắn.
- **Daily Standup Reminder:** phần reminder có thể giải đủ tốt bằng lịch cố định và template; phạm vi pain hẹp hơn.
- **BEV Vision Debugging:** phù hợp một nhóm chuyên môn hẹp, cần dữ liệu đa cảm biến và môi trường đánh giá riêng.

---

## 2. Quick validation

### 2.1. Bằng chứng hiện có

| Nguồn | Mẫu | Tín hiệu xác nhận | Tín hiệu phản bác / chưa chắc | Nhóm sửa problem thế nào |
|---|---:|---|---|---|
| Bốn individual reports | 4 thành viên | 2/4 mô tả trực tiếp việc tổng hợp task/deadline đa nền tảng; 1/4 có pain gần kề về daily và lịch họp | Không phải tất cả thành viên đều gặp cùng mức độ pain | Actor của pilot là sinh viên/researcher tham gia nhiều khóa học hoặc dự án, không khái quát cho mọi sinh viên |
| Hai Problem Cards trực tiếp | 2 thành viên | Baseline ước tính 15–30 phút/ngày; 3–5 lượt kiểm tra/ngày; nguồn gồm LMS, Calendar, Slack/Discord, Notion, Gmail và GitHub | Các số liệu là self-estimate, chưa có time-log; “sót 1–2 deadline” chưa ghi rõ khoảng thời gian | Ghi baseline là giả thuyết; bắt buộc đo lại trước pilot |
| Challenge trong bài của Quốc Khánh | 1 thành viên | Pain nằm ở việc nối mẩu thông tin thành task có action, deadline và context | Có thể nguyên nhân chính là thiếu một quy trình cá nhân thống nhất, không phải thiếu AI | Pilot phải so sánh với phương án No AI: một task manager + khung giờ review cố định |
| Ghi chú metric trong bài của Mỹ Anh | 1 thành viên | Nhấn mạnh cần log thực nghiệm trước khi cam kết metric | Không được dùng ước lượng làm kết quả đã kiểm chứng | Thêm giai đoạn baseline 7 ngày và audit lỗi |

### 2.2. Insight sau validation

```text
Pain cốt lõi không phải “thiếu một chatbot quản lý lịch”.
Pain là việc người dùng phải biến các mẩu thông tin phân tán thành một task
có hành động, deadline, ngữ cảnh và nguồn gốc rõ ràng.
```

Trạng thái bằng chứng:

- **Đã có:** hai trải nghiệm độc lập, workflow cụ thể và baseline ước tính.
- **Chưa có:** time-log đủ dài, precision/recall của việc trích xuất deadline, số lần trễ hạn theo một cửa sổ thời gian cố định.
- **Hành động tiếp theo:** đo baseline bảy ngày trước khi coi metric hiện tại là số liệu chính thức.

---

## 3. Research giải pháp hiện có

Research snapshot: **27/07/2026**. Nhóm ưu tiên tài liệu chính thức của sản phẩm và nền tảng.

| Nguồn / tool | Link | Họ giải quyết phần nào? | Điểm mạnh | Khoảng trống / rủi ro | Bài học cho nhóm |
|---|---|---|---|---|---|
| Google Calendar + Google Tasks | [Google Calendar Help](https://support.google.com/calendar/) và [Calendar API overview](https://developers.google.com/workspace/calendar/api/guides/overview) | Một nơi hiển thị event/task và API cho event, calendar, ACL | Phù hợp làm centralized view; dữ liệu ngày giờ có cấu trúc | Không tự chuyển mọi tin nhắn ngoài Google thành task đáng tin cậy; quyền truy cập phải được giới hạn | Dùng làm destination/hub, không coi là toàn bộ solution |
| Slack Workflow Builder + Events API | [Workflow Builder](https://slack.com/help/articles/360035692513-Guide-to-Slack-Workflow-Builder) và [Events API](https://docs.slack.dev/apis/events-api/) | Trigger workflow từ hoạt động/tin nhắn và nhận event theo subscription | Hỗ trợ automation, connector và event-driven flow | Phụ thuộc OAuth scope, workspace policy và chỉ được thấy dữ liệu người dùng/app đã được cấp quyền | Chỉ đọc channel được chọn; không quét toàn bộ workspace hoặc DM |
| Notion Database Automation + AI Autofill | [Database automations](https://www.notion.com/help/database-automations) và [Notion AI for databases](https://www.notion.com/help/autofill) | Tự động hóa database; trích xuất key information hoặc deadline từ nội dung page | Hữu ích khi dữ liệu đã nằm trong Notion | Basic Autofill chỉ dùng nội dung của row/page; nguồn ngoài vẫn cần ingestion và kiểm soát quyền | Có thể dùng làm dashboard, nhưng vẫn cần source link và human approval |
| Zapier Paths, Webhooks và Email Parser | [Paths](https://zapier.com/features/paths), [Webhooks](https://zapier.com/features/webhooks), [Email Parser](https://zapier.com/features/parser) | Rule-based routing và chuyển dữ liệu giữa các app | Giải quyết tốt trigger, filter và chuyển dữ liệu có cấu trúc | Logic phức tạp, quyền truy cập và chi phí; parser template chưa giải quyết tốt ngôn ngữ mơ hồ | Dùng automation cho phần deterministic; AI chỉ cho text phi cấu trúc |
| GitHub Notifications | [GitHub Docs](https://docs.github.com/en/subscriptions-and-notifications/concepts/about-notifications) | Triage activity trong issue, PR, CI và repository | Có filter, saved state và lý do nhận notification | Chỉ giải quyết trong GitHub; notification không đồng nghĩa với task có deadline | Không kéo toàn bộ notification; chỉ lấy item được assign/mention hoặc có rule rõ |

Research takeaway:

```text
Không cần build Agent tự trị ngay.
Các nền tảng hiện có đã xử lý tốt phần trigger, API, filter và centralized view.
Khoảng trống phù hợp cho AI là text phi cấu trúc: nhận diện candidate task,
chuẩn hóa deadline/context và gộp thông báo trùng; người dùng vẫn phải xác nhận.
```

---

## 4. Workflow before/after

### 4.1. Current workflow

```text
CURRENT STATE — baseline giả thuyết 15–30 phút/ngày

[1 Mở LMS/Gmail: 3–5']
→ [2 Mở Google Calendar: 2–3']
→ [3 Lướt Slack/Discord tìm thông báo: 5–8']  <-- bottleneck
→ [4 Kiểm tra GitHub/Notion: 3–5']
→ [5 Nối các mẩu thông tin thành task: 3–5']   <-- bottleneck
→ [6 Gõ lại task/deadline vào note hoặc nhớ trong đầu: 2–5']
→ [7 Mở lại các nguồn 3–5 lần trong ngày]
```

Bàn giao hiện tại:

```text
Nguồn thông báo
→ người học tự đọc
→ bộ nhớ/note cá nhân
→ task hoặc calendar event
```

Hai điểm nghẽn:

1. **Discovery:** thông báo quan trọng bị trôi giữa nhiều nguồn.
2. **Normalization:** người dùng phải tự suy ra action, deadline, context và nhập lại.

### 4.2. Future workflow

```text
FUTURE STATE — mục tiêu ≤8 phút/ngày

[1 Read-only sync từ Calendar + 1 nguồn thông báo đã chọn: <1'] -- API/Rule
→ [2 Rule lọc ngày giờ, keyword và loại event rõ: <1']          -- Rule
→ [3 AI trích xuất candidate task + deadline + context: <1']    -- AI step
→ [4 Rule gộp trùng và gắn source link: <1']                    -- Rule
→ [5 Người dùng approve/edit/dismiss: 4–5']                     -- human boundary
→ [6 Chỉ item đã approve mới được ghi vào hub: <1']             -- controlled write
→ [7 Một daily review; mở nguồn gốc khi cần xác minh]
```

Schema tối thiểu của một candidate task:

```text
title
action
due_at hoặc "chưa xác định"
source_name
source_url/message_id
context_summary
confidence
status = pending_review | approved | dismissed
```

Fallback:

- Không chắc ngày/giờ hoặc action → đánh dấu `chưa xác định`, không tự tạo task.
- Không có quyền đọc nguồn → bỏ qua nguồn đó và hiển thị trạng thái thiếu dữ liệu.
- AI lỗi hoặc confidence thấp → hiển thị raw message để người dùng nhập thủ công.
- Pipeline dừng → người dùng quay về Calendar + checklist review cố định.

Bottleneck mới:

```text
Người dùng review candidate tasks.
```

Đây là bottleneck chấp nhận được vì nó giữ quyền quyết định và kiểm soát lỗi ở người dùng.

### 4.3. Before/after impact

| Metric | Trước — baseline giả thuyết | Sau kỳ vọng | Cách đo |
|---|---:|---:|---|
| Thời gian tổng hợp kế hoạch | 15–30 phút/ngày | ≤8 phút/ngày | Bấm giờ 7 ngày baseline và 14 ngày pilot |
| Lượt mở lại nền tảng để tìm task | 3–5 lượt/ngày | ≤1 lượt review tổng hợp/ngày; mở nguồn khi xác minh | Tally theo ngày |
| Task có source link | Chưa đo; nhiều item ghi tay | 100% item do hệ thống tạo | Audit cuối ngày |
| Deadline quan trọng bị bỏ sót bởi workflow | Chưa có baseline theo cửa sổ cố định | 0 trong 14 ngày pilot | So sánh với ground truth do người dùng lập cuối ngày |
| Precision của candidate task | Chưa có | ≥90% | Approved đúng / tổng candidate được tạo |
| Recall trên tập tin nhắn được gắn nhãn | Chưa có | ≥95%; deadline quan trọng mục tiêu 100% | So với tập ground truth do hai người dùng gắn nhãn |
| Hành động không được phê duyệt | Không áp dụng | 0 | Audit log mọi write action |

---

## 5. Problem Statement v0

| Field | Nội dung |
|---|---|
| **Actor** | Sinh viên/researcher tham gia đồng thời nhiều lớp học và dự án, phải theo dõi task/deadline trên nhiều nền tảng. |
| **Workflow** | Mỗi ngày người dùng mở LMS/Gmail, Calendar, Slack/Discord, GitHub/Notion; đọc thông báo; nối ngữ cảnh; ghi lại task và tự ưu tiên. |
| **Bottleneck** | Việc phát hiện thông báo quan trọng và chuyển các mẩu thông tin thành task có action, deadline, context và nguồn gốc rõ ràng. |
| **Impact** | Baseline ước tính 15–30 phút/ngày, 3–5 lượt kiểm tra lại và có nguy cơ bỏ sót deadline; số liệu cần đo lại bằng log. |
| **Success Metric** | Giảm thời gian xuống ≤8 phút/ngày, ≤1 lượt review tổng hợp, 100% task có source link, không có write action chưa được duyệt. |
| **Boundary** | Không đọc nguồn chưa được cấp quyền; không tự gửi email/tin nhắn; không xác nhận lịch; không đổi deadline; không tạo, sửa hoặc xóa task chính thức trước khi người dùng approve. |

---

## 6. Rule / Workflow / Agent

| Mức | Phương án | Khi nào đủ | Rủi ro / giới hạn | Chọn? |
|---|---|---|---|---|
| **No AI / Process fix** | Chọn một task manager, khung giờ review cố định và quy ước mọi deadline phải vào Calendar | Đủ nếu nhóm có thể tuân thủ một nguồn chính và số thông báo ít | Vẫn phải nhập tay từ nguồn ngoài; phụ thuộc kỷ luật của nhiều người | Dùng làm baseline đối chứng |
| **Rule** | Filter email/keyword, webhook, sync Calendar, template task | Đủ với thông báo có format và ngày giờ rõ | Bỏ sót câu tự nhiên mơ hồ; khó gộp context từ nhiều message | Dùng cho data sync, filter, dedup và write guard |
| **Workflow** | Read-only sync → Rule filter → AI extract → Rule dedup/source link → user approve → controlled write | Hợp vì đường đi cố định, AI chỉ xử lý text phi cấu trúc và có human review | AI có thể false positive, bỏ sót hoặc suy ra sai ngày | **Chọn** |
| **Agent** | Tự chọn nguồn, tự hỏi thêm, tự ưu tiên và tự tạo/đổi task/calendar | Chỉ đáng cân nhắc khi cần nhiều nhánh động và cơ chế quyền/rollback đã trưởng thành | Scope và permission lớn; khó kiểm soát hành động sai; chưa cần cho MVP | Chưa chọn |

Mức chọn:

```text
Workflow.
```

Vì sao:

- Data collection và các deadline có cấu trúc nên dùng API/Rule.
- AI chỉ có lợi thế ở tin nhắn ngôn ngữ tự nhiên, gộp context và chuẩn hóa candidate task.
- Người dùng review trước mọi write action nên rủi ro có thể rollback.
- Luồng xử lý cố định, chưa cần khả năng tự lập kế hoạch của Agent.

---

## 7. Problem Statement v1

| Field | Nội dung |
|---|---|
| **Actor** | Sinh viên/researcher đang tham gia nhiều lớp học hoặc dự án và theo dõi task/deadline qua Calendar cùng các kênh thông báo. |
| **Workflow** | Mở từng nền tảng → tìm thông báo có hành động → nối context → suy ra deadline → nhập task → kiểm tra lại trong ngày. |
| **Bottleneck** | Chuyển thông báo phân tán, đặc biệt là text phi cấu trúc, thành một candidate task có action, deadline và source link đáng tin cậy. |
| **Impact** | Baseline giả thuyết 15–30 phút/ngày và 3–5 lượt kiểm tra lại; có nguy cơ bỏ sót hoặc xử lý muộn. Baseline chính thức sẽ được đo trong 7 ngày. |
| **Success Metric** | ≤8 phút/ngày; ≤1 lượt review tổng hợp; 100% task có source link; precision ≥90%; recall ≥95%; 0 write action chưa duyệt. |
| **Boundary** | Pilot chỉ dùng Calendar và một nguồn thông báo được chọn; read-only mặc định; dữ liệu ngoài scope không được đọc; item mơ hồ phải gắn `chưa xác định`; người dùng approve trước khi ghi. |
| **AI intervention point** | Sau bước Rule lọc candidate message và trước bước người dùng review. |
| **Mức chọn** | Workflow: Rule sync/filter/dedup, AI extract text, người dùng approve, Rule kiểm soát write. |
| **Rủi ro & người thật kiểm tra** | False positive, bỏ sót, sai múi giờ/ngày tương đối, lộ dữ liệu và duplicate. Người dùng đối chiếu source link, chỉnh/sửa hoặc dismiss trước khi tạo task. |

Problem Statement một câu:

> Sinh viên/researcher tham gia nhiều lớp học và dự án đang mất ước tính 15–30 phút mỗi ngày để mở nhiều nền tảng và tự chuyển các thông báo rời rạc thành task có action, deadline và ngữ cảnh; nhóm đề xuất một Workflow read-only dùng Rule để lọc/đồng bộ, AI để tạo candidate task và bắt buộc người dùng xác nhận trước khi ghi, với mục tiêu giảm thời gian xuống ≤8 phút/ngày mà không có hành động chưa được phê duyệt.

---

## 8. Final decision

Decision:

```text
Go với Workflow pilot nhỏ.
Not Yet với Agent tự trị hoặc tích hợp toàn bộ nền tảng.
```

### Pilot nhỏ nhất

- **Người dùng:** Quốc Khánh và Đức Anh — hai thành viên có Problem Card trực tiếp.
- **Thời lượng:** 7 ngày đo baseline + 14 ngày chạy pilot.
- **Nguồn:** Google Calendar và một kênh thông báo được hai người dùng chủ động chọn/cấp quyền; giai đoạn đầu có thể dùng export hoặc copy-paste thay vì live integration.
- **Destination:** một centralized review list; chỉ khi approve mới tạo Google Task/Calendar event.
- **Ground truth:** cuối mỗi ngày, mỗi người tự lập danh sách task/deadline thực tế và đối chiếu với output.
- **Đối chứng No AI:** task manager duy nhất + khung giờ review cố định + rule/filter đơn giản.
- **Output cần đo:** thời gian, số lượt mở nguồn, precision, recall, source-link coverage, số item phải sửa và số write action chưa duyệt.

### Go / No-Go gate sau pilot

Go sang MVP tích hợp API nếu:

- Median thời gian review ≤8 phút/ngày và giảm ít nhất 40% so với baseline đo được.
- Precision ≥90%, recall ≥95% và không bỏ sót deadline được đánh dấu critical.
- 100% task có source link.
- Không có hành động ghi nào xảy ra trước khi người dùng approve.
- Hai người dùng đánh giá centralized view hữu ích hơn phương án No AI.

Giữ ở Rule/Process fix nếu:

- AI không tạo thêm lợi ích rõ so với filter, template và task manager duy nhất.
- Trên 30% candidate cần sửa lớn hoặc trên 20% candidate bị dismiss.
- Tiết kiệm thời gian dưới 20%.

Dừng hoặc rollback về read-only nếu:

- Có write action chưa được phê duyệt.
- Có truy cập dữ liệu ngoài source/channel đã cho phép.
- AI tự suy ra deadline nhưng không gắn cờ không chắc chắn hoặc không giữ source link.
- Workflow bỏ sót một deadline critical trong pilot; chỉ được tiếp tục sau khi root-cause và thêm guard.

### Decision rationale

- Problem có workflow, actor, bottleneck và metric đủ cụ thể để kiểm chứng.
- Hai thành viên độc lập gặp pain tương tự, nhưng nhóm không biến ước lượng thành số liệu đã được xác nhận.
- Phương án No AI và Rule được giữ làm baseline, không mặc định AI là đáp án.
- AI nằm ở một bước cần xử lý ngôn ngữ phi cấu trúc; mọi hành động có hậu quả đều thuộc human boundary.
- Scope pilot nhỏ, read-only trước, có exit criteria và rollback rõ.

---

## 9. Assumption và evidence log

| Claim | Trạng thái | Cần làm để xác nhận |
|---|---|---|
| Người dùng mất 15–30 phút/ngày | Ước lượng từ hai bài cá nhân | Time-log 7 ngày |
| Người dùng mở lại nền tảng 3–5 lần/ngày | Ước lượng cá nhân | Tally số lượt mở nguồn trong 7 ngày |
| Có nguy cơ/sự cố bỏ sót deadline | Anecdotal, chưa có cửa sổ đo thống nhất | Ghi số incident trong baseline và pilot |
| Rule không đủ cho text mơ hồ | Giả thuyết kỹ thuật | So sánh Rule-only với Rule + AI trên cùng tập message |
| Centralized view giúp giảm cognitive load | Giả thuyết người dùng | Survey ngắn cuối pilot và so sánh thời gian |
| Hai nguồn đủ tạo giá trị ban đầu | Giả thuyết scope | Đo coverage; chỉ thêm nguồn thứ ba nếu coverage thiếu có ý nghĩa |

---

*Group Report — Day 02 Lab v2*
