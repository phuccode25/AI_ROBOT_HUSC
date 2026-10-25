# khoa-yanshee-robot-deployment

**Phụ trách: Khoa — Tích hợp và triển khai robot.**

Thuộc dự án giọng nói điều khiển robot Yanshee. Tham khảo mã gốc: https://github.com/phuccode25/LLM

**Trạng thái: bộ khung phân công và bàn giao. Chưa có code huấn luyện, checkpoint, parser hoặc driver robot hoàn chỉnh.**
Không có kết quả thực nghiệm được tạo sẵn. Các mục chưa làm được ghi trong TASKS.md.

## Bắt đầu
1. Đọc CONTRACTS.md và TASKS.md.
2. Chốt đầu vào và đầu ra với người nhận trong HANDOFF.md.
3. Tạo code thực tế trong src/, cấu hình trong configs/, kết quả trong results/.
4. Ghi môi trường thực tế sau khi chạy được; không dùng dependency chưa kiểm tra làm môi trường chính thức.


## Nhiệm vụ
1. Xác nhận phần cứng, kiến trúc OS, RAM trống, Python/compiler, microphone và phiên bản YanAPI.
   Repo gốc mô tả Pi 3B ARMv7 32-bit nhưng phải kiểm tra thiết bị hiện tại.
2. Xác nhận action catalog: lệnh nào có API, tham số/đơn vị, giới hạn, điều kiện trạng thái và kết quả trả về.
3. Dựng pipeline bằng model gốc ngay, không chờ Teacher/Student hoàn thành.
4. Thu âm liên tục, VAD/endpointing, nạp ASR một lần và giữ RAM; ghi thời điểm các giai đoạn.
5. Nối parser của Thi; kiểm tra JSON/action catalog độc lập trước khi gọi YanAPI.
6. Hàng đợi thực thi: không phát lại cùng utterance_id; không chạy lệnh chuyển động đồng thời;
   xác định timeout, hành vi dừng và phản hồi khi robot/API lỗi. Không tự retry chuyển động mù.
7. Tích hợp Student Phúc bàn giao; so sánh với các ứng viên Thi chọn trên Pi.
8. Thu các bản lỗi kèm transcript được kiểm tra và metadata, bàn giao Bảo để cải thiện fine-tune.
9. Tạo script khởi động, cấu hình microphone/model, feedback âm thanh, log và demo nhiều lượt liên tiếp.

## Luồng triển khai
Microphone → VAD → ASR → parser Thi → kiểm tra ràng buộc → YanAPI → kết quả/feedback/log.
Bản đầu có thể dùng nút nhấn để nói; thêm chế độ nghe liên tục sau khi kiểm soát được kích hoạt nhầm.
Phản hồi âm thanh có thể dùng câu thu sẵn. Tránh để robot nhận tiếng loa của chính nó thành lệnh mới.
Giới hạn phạm vi audio phải thống nhất với cấu hình export; không tự giảm audio context gây mất lời.

## Hỗ trợ fine-tune cho Bảo
- Danh sách tên/lệnh thực tế và cách phát âm tên robot.
- Bản thu từ microphone robot: quiet/noise, near/far; người nói/phiên thu được ghi rõ.
- Danh sách lỗi ASR, không trộn lẫn lỗi parser hoặc YanAPI.
- Train mới không dùng âm thanh thuộc test đã khóa.

## Đo trên thiết bị
Độ trễ endpointing, ASR, parser, API, từ cuối lời nói đến bắt đầu hành động; p50/p95;
RAM đỉnh, RTF, cold/warm start, số threads, CPU temperature nếu có, nguồn điện và số lần thử.
Xác nhận runtime cài được trên OS hiện tại trước khi chọn model. Nếu không chạy được, báo kết quả
blocked/incompatible thay vì suy đoán tốc độ. Không thay benchmark GPU cho số đo Pi.


## Cấu trúc
- CONTRACTS.md: hợp đồng dữ liệu và giao tiếp chung.
- TASKS.md: checklist công việc và nghiệm thu.
- HANDOFF.md: mẫu bàn giao phiên bản.
- configs/: cấu hình mẫu, chưa tối ưu.
- data/: manifest mẫu, không chứa audio.
- src/: nơi bổ sung mã thực thi.
- results/: bảng kết quả trống, không phải kết quả chạy.
- docs/: thiết kế, lỗi và biên bản thí nghiệm.

## Nghiệm thu
Có lệnh chạy tái hiện, input/output mẫu, môi trường, kết quả đo, giới hạn và người nhận xác nhận.
Không đưa token, dữ liệu lớn hoặc checkpoint vào Git. Quyền sử dụng model/dataset phụ thuộc nguồn gốc;
gói khung này chưa gán giấy phép thay cho nhóm.
