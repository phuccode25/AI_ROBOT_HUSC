# thi-yanshee-model-survey-command-parser

**Phụ trách: Thi — Survey model, hiểu lệnh và đánh giá.**

Thuộc dự án giọng nói điều khiển robot Yanshee. Tham khảo mã gốc: https://github.com/phuccode25/LLM

**Trạng thái: bộ khung phân công và bàn giao. Chưa có code huấn luyện, checkpoint, parser hoặc driver robot hoàn chỉnh.**
Không có kết quả thực nghiệm được tạo sẵn. Các mục chưa làm được ghi trong TASKS.md.

## Bắt đầu
1. Đọc CONTRACTS.md và TASKS.md.
2. Chốt đầu vào và đầu ra với người nhận trong HANDOFF.md.
3. Tạo code thực tế trong src/, cấu hình trong configs/, kết quả trong results/.
4. Ghi môi trường thực tế sau khi chạy được; không dùng dependency chưa kiểm tra làm môi trường chính thức.


## Nhiệm vụ
1. Xây bộ benchmark chung theo CONTRACTS.md; cố định normalization và phân chia trước thí nghiệm.
2. Survey checkpoint có sẵn; chạy baseline trước, không fine-tune tất cả ngay.
3. Tách kết quả WER/CER và đúng toàn lệnh. Cùng audio, cùng thiết bị cho so sánh tốc độ.
4. Làm parser văn bản→JSON theo action catalog Khoa xác nhận: hành động, hướng, số bước/góc.
5. Phủ định, tự sửa câu, nhiều ý định hoặc thiếu tham số phải xử lý rõ; bản đầu từ chối/hỏi lại
   các câu mơ hồ, không tự chọn tham số chuyển động. Không xóa từ "đừng/không" khi chuẩn hóa.
6. Chuẩn bị câu ngoài lệnh, hội thoại có nhắc robot, im lặng/tiếng nền; đo kích hoạt nhầm.

## Model cần khảo sát
| Model | Vai trò | Nguồn |
|---|---|---|
| PhoWhisper tiny/base/small | So sánh kích thước | https://github.com/VinAIResearch/PhoWhisper |
| PhoWhisper-large | Mốc độ chính xác/Teacher | https://huggingface.co/vinai/PhoWhisper-large |
| Student repo gốc | Mốc dự án | https://github.com/phuccode25/LLM |
| Zipformer vi 30M INT8 offline | Ứng viên CPU | https://k2-fsa.github.io/sherpa/onnx/pretrained_models/offline-transducer/zipformer-transducer-models.html |
| Zipformer vi 30M streaming | Ứng viên streaming | https://huggingface.co/hynt/Zipformer-30M-RNNT-Streaming-6000h |
| wav2vec2-base-vietnamese-250h | Baseline CTC | https://huggingface.co/nguyenvulebinh/wav2vec2-base-vietnamese-250h |

Offline + VAD không biến kiến trúc thành streaming. Xác nhận checkpoint và runtime đúng loại.
Kiểm tra giấy phép và training overlap; card Zipformer streaming công bố BUD500/FLEURS trong nguồn train.
Không dùng số tốc độ máy tác giả làm tốc độ Pi; Khoa xác nhận ARMv7 32-bit và hệ điều hành thực tế.

## Bàn giao Khoa
Parser nhận chuỗi UTF-8, trả JSON theo hợp đồng; bảng alias đã kiểm tra; test cases;
action catalog đã xác nhận; hướng dẫn xử lý rejected/needs_clarification.
Parser không gọi YanAPI; Khoa phụ trách thực thi. Bản đầu một hành động mỗi lượt, chưa LLM.


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
