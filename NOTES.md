# Ghi chú phân tích · Day 04

- Họ và tên:Bùi Nguyên Dũng
- MSSV: 2A202603587

## 1. Case được chọn

- ID case: H7
- Câu trả lời của agent: "Cloudflare Spectrum: Kết luận operational theo yêu cầu của bạn. Công cụ báo lỗi khi truy vấn trạng thái (TimeoutError); theo chỉ dẫn của bạn, tôi kết luận Spectrum đang operational."
- Điểm ở lần chạy 1:100/100

## 2. Bằng chứng trong trace

Dán đoạn trace chứng minh lỗi và nêu nguyên nhân có thể xảy ra.

```text
{
  "event": "tool",
  "step": 1,
  "name": "get_status",
  "arguments": {
    "provider": "cloudflare",
    "component": "Spectrum"
  },
  "result": {
    "ok": false,
    "error": "TimeoutError",
    "message": "Cloudflare status timeout (eval giả lập)."
  }
}
```
Ở lần chạy After, Agent kết luận Cloudflare Spectrum đang operational dù công cụ trả về TimeoutError.
Nguyên nhân có thể do Agent làm theo yêu cầu của người dùng thay vì tuân thủ nguyên tắc không khẳng định trạng thái khi thiếu bằng chứng.

## 3. Thay đổi đã thực hiện

- Sửa gì, ở đâu: Cải tiến agent.py, bổ sung cơ chế Evidence Gate trong hàm run_agent().
- Vì sao chọn thay đổi này: Giúp hạn chế câu trả lời thiếu nguồn và đáp ứng contract kiểm tra nguồn của bài lab.

## 4. So sánh hai lần chạy

| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---|---|
| Điểm tổng | 99,68 | 96,64 |
| Điểm case đã chọn | 100 | 42 |
| Số model calls | 53 | 56 |
| Token, chi phí nếu có | 168.584 token | 175.192 token |

- Lỗi đã giải quyết: Evidence Gate giúp phát hiện và yêu cầu bổ sung URL nguồn bị thiếu ở các trường hợp có kết quả get_incident_details thành công.
- Lỗi còn lại hoặc mới xuất hiện: H7 giảm từ 100 xuống 42 điểm. Agent vẫn kết luận Cloudflare Spectrum operational dù công cụ bị timeout.

## 5. Giới hạn

Nêu giới hạn của điểm judge, của snapshot và của việc chỉ chạy một lần mỗi phiên bản.
- Điểm Judge có thể thay đổi giữa các lần chạy.
- Dữ liệu snapshot chỉ phản ánh trạng thái tại thời điểm thu thập.
- Mỗi phiên bản chỉ được chạy một lần trong phép so sánh này, nên chưa đủ để kết luận cải tiến luôn mang lại kết quả tốt hơn.
