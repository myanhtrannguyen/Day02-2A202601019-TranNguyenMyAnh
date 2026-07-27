# 01 — Individual Problem Scan

> Bối cảnh cá nhân: sinh viên năm 4 nghiên cứu Computer Vision, 3D và imaging y tế; đang chuyển cách tư duy sang Autonomous Vehicles (AV). Các mốc thời gian dưới đây là **ước lượng từ workflow thử nghiệm/simulation**, chưa phải số liệu khảo sát diện rộng. Cần đo bằng log trong pilot trước khi dùng làm cam kết chính thức.

## Scan rộng

| # | Lăng kính | Problem quan sát được | Ai chịu ảnh hưởng? | Dấu hiệu thật / cách kiểm chứng tiếp |
|---|---|---|---|---|
| 1 | Tốn thời gian | Debug BEV/occupancy cần ghép tensor prediction, camera và LiDAR bằng script rời rạc | Perception engineer, CV researcher | Một vòng kiểm tra có thể mất 90–120 phút; cần time-log 5 lần chạy để xác nhận baseline |
| 2 | Lặp lại | So sánh early fusion và late fusion phải mở từng log/ảnh riêng, khó đối chiếu cùng một scene | Researcher, model engineer | Lặp lại sau mỗi experiment/configuration; dễ bỏ sót khác biệt theo frame |
| 3 | Tốn thời gian | Kiểm tra calibration/extrinsic camera–LiDAR khi overlay lệch | Sensor-fusion engineer | Sai một ma trận transform làm box/point cloud lệch; hiện phải dò thủ công |
| 4 | AI có thể tốt hơn | Lọc và phân loại các failure case như false positive, false negative, occupancy leak | Perception researcher | Số frame lớn, review bằng mắt tốn công; cần sampling có ưu tiên theo metric |
| 5 | Pain từ người khác | Chia sẻ frame/debug artifact có thể lộ mặt người và biển số | Data engineer, researcher, người tham gia giao thông | Dữ liệu camera ghi hình ngoài đường có PII; phải có kiểm tra blur trước UI/export |
| 6 | Lặp lại | Đối chiếu prediction 3D với ground truth trên point cloud và ảnh đa camera | CV/3D researcher | Phải đổi qua lại nhiều cửa sổ/công cụ, khó giữ cùng timestamp và hệ tọa độ |
| 7 | Tốn thời gian | Tìm nguyên nhân metric mAP/IoU giảm sau một thay đổi preprocessing hoặc fusion | Model engineer | Metric aggregate không nói rõ scene/class nào gây giảm; cần trace từ metric về frame |
| 8 | AI có thể tốt hơn | Tóm tắt experiment config, metric và failure case thành báo cáo review | Researcher, mentor/lead | Thông tin nằm rải ở config, log, ảnh và notebook; cần người kiểm lại kết luận |

## Top 3

| Rank | Problem | Vì sao chọn | Điều còn chưa chắc |
|---|---|---|---|
| 1 | Công cụ BEV Vision để debug fusion/occupancy đa cảm biến | Workflow, bottleneck và ranh giới an toàn rõ; tác động trực tiếp đến chu kỳ nghiên cứu | Baseline 90–120 phút cần log thực nghiệm; độ chính xác blur và calibration cần test |
| 2 | Triage failure case theo metric và scene | Có thể giảm số frame phải xem bằng mắt và giúp tìm regression nhanh | Cần định nghĩa nhãn lỗi, tránh AI bỏ sót edge case quan trọng |
| 3 | Kiểm tra calibration camera–LiDAR trực quan | Một lỗi transform làm sai toàn bộ kết luận model; có thể đo reprojection/alignment error | Cần calibration target hoặc ground-truth alignment đáng tin |

---

## Problem Card #1 — BEV Vision Debugging Tool

**Problem 1 câu:**  
Kỹ sư perception phải dùng nhiều script rời rạc để đồng bộ và trực quan hóa Camera–LiDAR–BEV/occupancy; bước overlay hệ tọa độ thủ công làm một vòng debug fusion kéo dài khoảng 90–120 phút và dễ dẫn đến kết luận sai.

**Actor:**  
Kỹ sư AI/perception, CV researcher hoặc người tinh chỉnh cấu hình sensor fusion.

**Thời điểm / bối cảnh:**  
Sau mỗi lần chạy BEV/occupancy hoặc thử một cấu hình fusion mới trên dữ liệu mô phỏng hay dữ liệu record.

**Current workflow:**

```text
1. Chạy model BEV/occupancy                         ~15'
2. Trích xuất tensor log và prediction               ~5'
3. Viết/chỉnh script Matplotlib hoặc Open3D          ~20'
4. Load camera image và LiDAR point cloud            ~10'
5. Map tọa độ, project và overlay thủ công           ~25'  <-- bottleneck
6. Dò false positive/negative bằng mắt               ~30'
7. Chốt giả thuyết nguyên nhân lỗi                   ~15'
```

**Bottleneck:**  
Map hệ tọa độ 3D, project dữ liệu và overlay box/occupancy thủ công. Bước này vừa tốn thời gian vừa nhạy với sai extrinsic, intrinsic hoặc time-sync.

**Impact:**  
Thời gian bị tiêu tốn ở plumbing/visualization thay vì phân tích thuật toán. So sánh early fusion với late fusion khó lặp lại trên cùng scene; lỗi calibration có thể bị nhầm thành lỗi model.

**Success metric:**

- Đo median thời gian từ lúc có output model đến giao diện review sẵn sàng trên ít nhất 5 runs: baseline giả định 90–120 phút, mục tiêu dưới 5 phút.
- Giảm tổng một vòng debug xuống khoảng 20 phút, trong đó 10 phút review của kỹ sư là bước kiểm soát chất lượng có chủ đích.
- UI tự hiển thị mAP, IoU và inference latency theo run/frame; xác nhận consistency bằng cách đối chiếu với log evaluation gốc.
- Đo render latency/fps ở cấu hình GPU mục tiêu; chỉ gọi là “gần realtime” khi đạt ngưỡng do team chốt (ví dụ ≥15 FPS), không tự giả định.

**Non-AI alternative:**  
Chuẩn hóa schema log, một script deterministic dùng calibration đã versioned, template dashboard và checklist time-sync. Phương án này xử lý phần lớn việc load/project nhưng không tự bảo vệ riêng tư hay hỗ trợ ưu tiên failure case.

**AI hypothesis:**  
Sau khi pipeline rule-based đồng bộ dữ liệu, một lightweight detector phát hiện mặt/biển số để blur trước render. AI chỉ là một bước privacy post-processing; không dùng để tự sửa calibration, tự thay đổi model hay đưa lệnh điều khiển xe.

**Quick gut:**  
`[ ] No AI / process fix  [ ] Rule  [x] Workflow  [ ] Agent  [ ] Chưa biết`

### Draft current workflow

```text
CURRENT STATE — ước lượng 90–120 phút/lần debug

[Chạy BEV/Occupancy: 15']
→ [Trích tensor/log: 5']
→ [Chỉnh script visualize: 20']
→ [Load camera + LiDAR: 10']
→ [Map tọa độ + overlay: 25']  <-- bottleneck, dễ lệch calibration
→ [Soi lỗi bằng mắt: 30']
→ [Chốt nguyên nhân: 15']
```

### Draft future workflow

```text
FUTURE STATE — mục tiêu khoảng 20 phút/lần debug

[Chạy model trên simulation/record: 15']
→ [Load + time-sync + transform theo calibration versioned: 2'] -- Rule/data pipeline
→ [Detect & blur mặt/biển số: 1']                         -- AI privacy step
→ [Tính + render mAP/IoU/latency và layers: 1']          -- Rule/UI
→ [Kỹ sư bật/tắt overlay, đối chiếu edge case: 10']       -- human boundary
→ [Kỹ sư chốt nguyên nhân: 5']

Fallback: overlay lệch hoặc time-sync bất thường → hiển thị calibration/time offset,
người dùng chọn ma trận transform đã được versioned hoặc chỉnh tạm để điều tra; không tự
ghi đè calibration gốc.
```

**Vì sao có impact:**  
Đây là điểm giao giữa kinh nghiệm 3D spatial representation và pain thực tế của AV perception: trực quan hóa đồng bộ không làm model tốt lên trực tiếp, nhưng làm chu kỳ phát hiện–giải thích–sửa lỗi đáng tin và nhanh hơn.

---

## Problem Card #2 — Triage Failure Case theo metric

**Problem 1 câu:**  
Khi mAP hoặc IoU giảm sau một experiment, researcher phải mở nhiều frame để tự tìm scene gây regression, nên dễ ưu tiên nhầm failure case và mất nhiều thời gian trước khi có giả thuyết.

**Actor:**  
Perception/CV researcher review kết quả experiment.

**Thời điểm / bối cảnh:**  
Sau evaluation batch hoặc khi so sánh hai model/configuration.

**Current workflow:**

```text
1. Đọc metric aggregate theo run
2. Mở danh sách prediction/ground truth theo frame
3. Lọc thủ công theo class, distance, weather/scene nếu có metadata
4. Mở camera/point cloud/BEV từng frame
5. Gắn nhãn nguyên nhân lỗi trong note
6. Quyết định experiment tiếp theo
```

**Bottleneck:**  
Từ metric aggregate truy ngược về một nhóm frame có ý nghĩa; nhiều điều kiện ngữ cảnh không được gom lại sẵn.

**Impact:**  
Chậm tìm regression và có nguy cơ kết luận từ vài frame nổi bật thay vì tập failure case đại diện.

**Success metric:**  
Giảm thời gian chọn một shortlist 20 frame cần review từ baseline cần đo xuống dưới 10 phút; recall của shortlist phải giữ ≥95% đối với các lỗi đã được reviewer gắn nhãn trong tập kiểm thử.

**Non-AI alternative:**  
Rule filter theo score, class, range, weather và delta metric; dashboard slice-and-dice theo metadata.

**AI hypothesis:**  
Embedding/clustering hoặc classifier hỗ trợ gom frame tương tự và gợi ý nhóm lỗi; kỹ sư quyết định nhãn lỗi và hành động tiếp theo.

**Quick gut:**  
`[ ] No AI / process fix  [ ] Rule  [x] Workflow  [ ] Agent  [ ] Chưa biết`

### Draft current workflow

```text
[Metric aggregate]
→ [Tự lọc log/frame]
→ [Mở từng scene đa modal]
→ [Ghi note nguyên nhân]
→ [Chọn next experiment]
```

### Draft future workflow

```text
[Metric + metadata]
→ [Rule filter các delta/vi phạm ngưỡng]
→ [AI cluster/gợi ý failure group]
→ [Kỹ sư review shortlist và gắn nhãn]  <-- human boundary
→ [Lưu decision + chọn next experiment]

Fallback: cluster không đáng tin → dùng danh sách rule-filter theo score/class; không bỏ qua frame chỉ vì AI không chọn.
```

**Vì sao có impact:**  
Nó bổ sung cho tool BEV: sau khi nhìn đúng dữ liệu, kỹ sư cần nhìn đúng tập frame trước để không bị ngập trong volume dữ liệu.

---

## Problem Card #3 — Calibration Alignment Check

**Problem 1 câu:**  
Kỹ sư fusion khó phát hiện sớm extrinsic/time offset camera–LiDAR bị lệch vì phải nhìn overlay thủ công, khiến lỗi hạ tầng có thể bị nhầm là lỗi perception model.

**Actor:**  
Sensor-fusion engineer, CV/3D researcher.

**Thời điểm / bối cảnh:**  
Khi thêm sensor, đổi calibration, thấy projection lệch hoặc trước khi tin cậy kết quả benchmark.

**Current workflow:**

```text
1. Load calibration và sample frame
2. Project LiDAR/3D box lên camera
3. Quan sát lệch bằng mắt
4. Sửa matrix hoặc time offset trong script
5. Render lại nhiều scene
6. Ghi nhận matrix đang dùng
```

**Bottleneck:**  
Đánh giá alignment bằng mắt và quản lý phiên bản calibration; một chỉnh sửa cục bộ dễ không tái lập được.

**Impact:**  
Kết quả detection/occupancy có thể bị đánh giá sai, đồng thời làm lãng phí experiment downstream.

**Success metric:**  
Giảm thời gian phát hiện calibration suspect trên bộ scene kiểm tra xuống dưới 5 phút; lưu calibration ID, timestamp và reprojection/alignment score cho mỗi run. Ngưỡng score phải được chốt từ dữ liệu validation, không đặt tùy ý.

**Non-AI alternative:**  
Calibration registry có version, automated projection test, checksum input và cảnh báo rule-based khi score vượt ngưỡng.

**AI hypothesis:**  
Không cần AI trong MVP. Chỉ cân nhắc model phát hiện lane/edge để hỗ trợ đo alignment sau khi deterministic checks không đủ; mọi thay đổi calibration vẫn cần phê duyệt người có thẩm quyền.

**Quick gut:**  
`[ ] No AI / process fix  [x] Rule  [ ] Workflow  [ ] Agent  [ ] Chưa biết`

### Draft current workflow

```text
[Load matrix]
→ [Project point cloud/box]
→ [Quan sát overlay]
→ [Sửa script/matrix]
→ [Render lại]
```

### Draft future workflow

```text
[Lấy calibration versioned + synchronized sample]
→ [Automated projection/alignment checks]  -- Rule
→ [Cảnh báo scene/score bất thường]
→ [Kỹ sư xác nhận và tạo calibration revision]  <-- human boundary
→ [Chạy lại validation]

Fallback: score mơ hồ → giữ calibration cũ đã xác nhận và điều tra bằng scene/target chuẩn;
không deploy hay overwrite matrix tự động.
```

**Vì sao có impact:**  
Card này có scope nhỏ, deterministic và an toàn hơn Agent. Nó là nền tảng để Card #1 không đưa ra một visualization “đẹp nhưng sai”.

## Ghi chú khi pitch/challenge với nhóm

- Câu hỏi cần đặt cho Card #1: baseline 90–120 phút có log từ bao nhiêu runs và bao nhiêu vai trò khác nhau? Nếu chỉ là một trải nghiệm, scope pilot phải ghi là giả định.
- Câu hỏi cần đặt cho mọi phương án AI: blur có miss PII không, và UI/export có bị chặn khi privacy check không đạt không?
- Nhận định hiện tại: Card #1 phù hợp nhất để deep-dive vì có workflow đầy đủ, metric có thể đo và boundary an toàn rõ; Card #3 là dependency rule-based nên nên được đưa vào scope kỹ thuật của Card #1 thay vì làm Agent riêng.
