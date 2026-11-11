# Ghi chú phân tích · Day 04

Họ và tên:Bùi Nguyên Dũng
MSSV: 2A202603587

## 1. Case được chọn

- ID case: A5
- Câu trả lời của agent:Cần trích nguyên văn từ eval_before.json
- Điểm ở lần chạy 1:57/100

## 2. Bằng chứng trong trace

Dán đoạn trace chứng minh lỗi và nêu nguyên nhân có thể xảy ra.

```text
Case A5 đạt 57/100.
Nhận xét của Judge cho thấy Agent chưa giải thích rõ giới hạn phạm vi dữ liệu incident.
Điều này có thể khiến người dùng hiểu nhầm rằng kết quả truy xuất bao phủ toàn bộ lịch sử sự cố.

Nguyên nhân có thể do Agent chưa diễn giải đầy đủ giới hạn dữ liệu từ công cụ trong câu trả lời cuối cùng.
```

## 3. Thay đổi đã thực hiện

- Sửa gì, ở đâu: Cải tiến agent.py, bổ sung cơ chế Evidence Gate trong hàm run_agent().
- Vì sao chọn thay đổi này: Giúp hạn chế câu trả lời thiếu nguồn và đáp ứng contract kiểm tra nguồn của bài lab.

## 4. So sánh hai lần chạy

| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---|---|
| Điểm tổng |  |  |
| Điểm case đã chọn |  |  |
| Số model calls |  |  |
| Token, chi phí nếu có |  |  |

- Lỗi đã giải quyết:
- Lỗi còn lại hoặc mới xuất hiện:

## 5. Giới hạn

Nêu giới hạn của điểm judge, của snapshot và của việc chỉ chạy một lần mỗi phiên bản.
