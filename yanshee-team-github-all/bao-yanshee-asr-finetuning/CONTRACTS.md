# Quy ước bàn giao chung — v1

Phạm vi hiện tại: tiếng nói → ASR → hiểu một lệnh → kiểm tra → YanAPI → robot.
LLM và lập kế hoạch nhiều bước nằm ngoài mốc đầu tiên.

## Dữ liệu
Manifest CSV: `id,audio_path,text,duration_sec,source_dataset,source_revision,original_split,split,speaker_id,region,session_id,is_synthetic`.
`region`: north / central / south / unknown. Thiếu thông tin phải ghi unknown, không tự suy đoán.
Đường dẫn audio tương đối với thư mục dữ liệu. Text là lời thực sự được nói, Unicode NFC, giữ dấu tiếng Việt.
Âm thanh đưa vào ASR: mono 16 kHz; giữ bản gốc. Đoạn quá dài phải được cắt và căn transcript tương ứng.
Chỉ train được cập nhật trọng số; validation chọn checkpoint; test dùng khi chốt cấu hình.
Giữ split chính thức, kiểm tra trùng giữa nguồn; dữ liệu tự thu chia theo người nói và phiên thu.
TTS chỉ ở train, gắn is_synthetic=true. Không lọc test theo lỗi Teacher, không dùng nhãn giả làm đáp án test.
Lưu nguồn, phiên bản, điều kiện truy cập và giấy phép; không tải lại dữ liệu lên GitHub khi chưa có quyền phân phối.

## Hợp đồng ASR
Input: file WAV mono 16 kHz. Output JSON UTF-8:
`{"schema_version":"1.0","utterance_id":"u001","text":"tiến hai bước","model_id":"checkpoint-id","latency_ms":null}`.
latency_ms phải là số đo thực hoặc null, không điền số ước lượng như kết quả.
Không yêu cầu điểm confidence nếu runtime không cung cấp điểm đã được kiểm chứng.

## Hợp đồng lệnh
Output: `schema_version`, `utterance_id`, `status`, `action`, `parameters`, `reason`.
status: accepted / rejected / needs_clarification. Nếu không accepted thì action=null, parameters={}.
Danh mục action trong kho chỉ là bản nháp; Khoa phải xác nhận hành động và giới hạn từ robot thực.
Không tự thực thi kế hoạch hoặc mã sinh tự do. Parser và bộ thực thi kiểm tra lại allowlist, kiểu dữ liệu, giới hạn.

## Đóng gói model
Bàn giao model + processor/tokenizer + generation config nếu áp dụng; phiên bản thư viện; lệnh chạy;
file WAV thử và output mong đợi; kết quả validation; nguồn model; giới hạn đã biết; mã checksum artifact.
Không commit checkpoint, token đăng nhập hoặc audio cá nhân lên repo. Dùng artifact storage phù hợp và ghi link trong HANDOFF.md.
Mỗi runtime phải xác nhận trên đúng máy đích; tốc độ GPU hoặc CPU máy bàn không đại diện Raspberry Pi.

## Đánh giá
WER/CER thô và sau chuẩn hóa báo cáo riêng; cố định quy tắc trước khi xem test.
Không thay hàng loạt từ nghe sai thành Yanshee để che lỗi trong chỉ số thô.
Đo riêng miền, từ khóa, đúng toàn lệnh (intent và tất cả tham số), kích hoạt nhầm trên câu ngoài lệnh/im lặng.
Độ trễ: từ lúc người nói kết thúc đến lúc robot bắt đầu hành động; báo cáo p50/p95, số mẫu, warm/cold và thiết bị.
RAM đỉnh, RTF, tỷ lệ lỗi thực thi; tách lỗi ASR/parser/runtime/robot.
Không tự động coi benchmark có sẵn là độc lập với pretraining; ghi rõ known/unknown overlap.
