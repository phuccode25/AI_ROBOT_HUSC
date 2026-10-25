# bao-yanshee-asr-finetuning

**Phụ trách: Bảo — Dữ liệu và fine-tune Teacher.**

Thuộc dự án giọng nói điều khiển robot Yanshee. Tham khảo mã gốc: https://github.com/phuccode25/LLM

**Trạng thái: bộ khung phân công và bàn giao. Chưa có code huấn luyện, checkpoint, parser hoặc driver robot hoàn chỉnh.**
Không có kết quả thực nghiệm được tạo sẵn. Các mục chưa làm được ghi trong TASKS.md.

## Bắt đầu
1. Đọc CONTRACTS.md và TASKS.md.
2. Chốt đầu vào và đầu ra với người nhận trong HANDOFF.md.
3. Tạo code thực tế trong src/, cấu hình trong configs/, kết quả trong results/.
4. Ghi môi trường thực tế sau khi chạy được; không dùng dependency chưa kiểm tra làm môi trường chính thức.


## Nhiệm vụ
1. Chuẩn bị manifest, kiểm tra file hỏng/rỗng/trùng và thống kê giờ theo nguồn, split, miền.
2. Đo PhoWhisper-large gốc và PhoWhisper-base trên validation; giữ dự đoán để phân tích lỗi.
3. Chạy thử 20–50 giờ được lấy từ TRAIN để kiểm tra pipeline; sau đó mở rộng theo khả năng GPU.
4. Fine-tune PhoWhisper-large bằng CE. LoRA hoặc full fine-tune là thí nghiệm; nếu thích nghi âm học,
   phải ghi rõ encoder/adapters nào được cập nhật. Chọn checkpoint theo validation, không theo test.
5. So sánh dữ liệu chung; thêm ViMD; thêm VN-SLU; thêm TTS Yanshee. Theo dõi mỗi miền và lỗi từ khóa.
6. Nhận âm thanh lỗi do Khoa cung cấp; nghe và sửa nhãn trước khi đưa vào train. Giữ test độc lập.

## Dataset cần chuẩn bị
| Nguồn | Vai trò | Cách dùng |
|---|---|---|
| [ViMD](https://huggingface.co/datasets/nguyendv02/ViMD_Dataset) | Giọng Bắc–Trung–Nam | Dùng train; giữ valid/test riêng; bảo toàn region và speakerID |
| [BUD500](https://huggingface.co/datasets/linhtran92/viet_bud500) | Tiếng Việt đa chủ đề | Lấy mẫu train trước; cần chấp nhận điều kiện truy cập; không tự gán miền |
| [VN-SLU](https://github.com/tuyen-tran1/VN-SLU) | Cách nói lệnh trợ lý | Audio→transcript cho ASR; intent/entities giao Thi; nếu thiếu split phải tạo và ghi rõ |
| TTS Yanshee từ repo gốc | Tên riêng, câu lệnh thiếu | Chỉ train, gắn cờ synthetic; không dùng để chứng minh nghe người thật |
| Âm thanh robot từ Khoa | Thích nghi microphone, môi trường | Kiểm tra thủ công; quản lý người nói và phiên thu |
| [GigaSpeech 2](https://huggingface.co/datasets/speechcolab/gigaspeech2) | Mở rộng tùy chọn | Chỉ Vietnamese refined; kiểm tra chất lượng nhãn; chưa cần ở lượt đầu |

Không ấn định tỷ lệ trộn tối ưu trước thực nghiệm. Ghi số giờ thực và tần suất lấy mẫu từng nguồn.

## Bàn giao Phúc
Teacher đầy đủ hoặc model nền + adapter và cách hợp nhất đã xác minh; processor; generation config;
manifest phiên bản; quy tắc chuẩn hóa; bảng kết quả; chương trình inference từ WAV; môi trường chạy.


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
