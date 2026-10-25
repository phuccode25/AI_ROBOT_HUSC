# Bốn kho GitHub cho nhóm Yanshee

Đây là bốn bộ khung công việc; chưa phải hệ thống hoặc model hoàn chỉnh.

| Kho đề xuất | Phụ trách |
|---|---|
| bao-yanshee-asr-finetuning | Bảo |
| phuc-yanshee-knowledge-distillation | Phúc |
| thi-yanshee-model-survey-command-parser | Thi |
| khoa-yanshee-robot-deployment | Khoa |

## Đưa lên GitHub
1. Tạo repository trống với tên đề xuất.
2. Giải nén ZIP tương ứng. Upload nội dung bên trong vào thư mục gốc của repository.
3. Đưa cả .gitignore và .github nếu dùng Git; giao diện kéo thả có thể bỏ qua file ẩn, cần kiểm tra lại.
4. Mời đúng thành viên và dùng TASKS.md hoặc Issues để theo dõi.

## Phụ thuộc
Bảo → Teacher → Phúc → Student → Khoa.
Thi → benchmark và parser → Khoa.
Khoa → action catalog, âm thanh thực tế, log → Bảo/Phúc/Thi.

## Mốc
M1: baseline điều khiển một lệnh trên robot, chưa cần model mới.
M2: Teacher cải thiện trên validation ba miền.
M3: Student có so sánh CE/KD và trước/sau nén.
M4: so sánh ứng viên trên robot; chọn theo đúng lệnh, độ trễ và RAM.
M5: khóa cấu hình, đánh giá test và demo; công bố giới hạn còn lại.

Chưa ấn định deadline, điểm số hoặc ngưỡng nghiệm thu chưa được cả nhóm thống nhất.
