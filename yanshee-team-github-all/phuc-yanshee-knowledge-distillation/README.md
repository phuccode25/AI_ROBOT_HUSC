# phuc-yanshee-knowledge-distillation

**Phụ trách: Phúc — Chưng cất, nén và xuất Student.**

Thuộc dự án giọng nói điều khiển robot Yanshee. Tham khảo mã gốc: https://github.com/phuccode25/LLM

**Trạng thái: bộ khung phân công và bàn giao. Chưa có code huấn luyện, checkpoint, parser hoặc driver robot hoàn chỉnh.**
Không có kết quả thực nghiệm được tạo sẵn. Các mục chưa làm được ghi trong TASKS.md.

## Bắt đầu
1. Đọc CONTRACTS.md và TASKS.md.
2. Chốt đầu vào và đầu ra với người nhận trong HANDOFF.md.
3. Tạo code thực tế trong src/, cấu hình trong configs/, kết quả trong results/.
4. Ghi môi trường thực tế sau khi chạy được; không dùng dependency chưa kiểm tra làm môi trường chính thức.


## Nhiệm vụ
1. Tái lập Student hiện có trong repo gốc trước khi thay đổi.
2. Khởi tạo Student từ PhoWhisper-base; thử decoder 2 và 4 lớp, ghi rõ lớp được giữ.
3. Nạp Teacher Bảo bàn giao, đặt eval, tắt gradient Teacher. Student học nhãn chuẩn và logits Teacher.
4. Loss: L = lambda_ce * CE + lambda_kd * T^2 * KL(p_teacher || p_student).
   Kiểm tra tokenizer/vocabulary và prefix tương thích; hai model dùng cùng prefix nhãn khi tính loss;
   bỏ padding; chuẩn hóa loss theo số token hợp lệ. Không sao chép toàn bộ weight large sang base.
5. So sánh encoder đóng băng với mở các lớp cuối; không mặc định freeze là tốt nhất cho giọng địa phương.
6. Xuất model cho whisper.cpp. Ghi phiên bản/commit converter và runtime. Đo suy giảm trước/sau lượng tử hóa.
7. Phối hợp Khoa đo trên Pi thật, phân tích thời gian encoder/decoder và RAM.

## Ma trận thí nghiệm tối thiểu
| ID | Student | Teacher | Loss | Encoder |
|---|---|---|---|---|
| D0 | base decoder 2 | không | CE | freeze |
| D1 | base decoder 2 | large gốc | CE+KD | freeze |
| D2 | base decoder 2 | Teacher Bảo | CE+KD | freeze |
| D3 | base decoder 2 | Teacher Bảo | CE+KD | mở lớp cuối |
| D4 | base decoder 4 | Teacher Bảo | CE+KD | cấu hình chọn trên validation |

Giữ dữ liệu và ngân sách tương đương; ghi seed và compute. Không áp KL theo token này trực tiếp
sang Zipformer RNNT/CTC khác tokenizer/căn chỉnh. Model khác trước mắt là đối chứng.

## Bàn giao Khoa
Student HF + assets; artifact whisper.cpp đã kiểm tra; hash; script chuyển đổi; phiên bản runtime;
lệnh nhận dạng WAV mẫu; kết quả trước/sau nén; giới hạn độ dài audio. Không cam kết converter hiện hành
chạy model decoder rút gọn nếu chưa kiểm tra đầy đủ.


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
