# TEAM — Day04, K4-L3B

**Làm nhóm.** Mỗi người tự viết và commit phần INDIVIDUAL của mình.

## Thông tin bài nộp

- Tên nhóm: 67
- Người đại diện / MSSV: Lưu Nguyên Khôi / 2A202602547
- Tên repo: K4-L3-DAY04-67-PromptEngineeringToolCalling
- URL repo, nhánh nộp, commit chốt: https://github.com/w1n10/K4-L3-Day04-67-PromptEngineeringToolCalling.git, nhánh main
- Deadline áp dụng và link thông báo đổi hạn nếu có:

## Thành viên

| Họ và tên | MSSV | GitHub | Vai trò và công việc | File/commit/PR |
|---|---|---|---|---|
| Phùng Quang Minh Huy | 2A202602610 | huy20 | viết test case, tool prototype | `pc_seller_data/*`, `data/eval_*.json`, commit `db81bb7` |
| Nguyễn Minh Hiếu | 2A202602669 | Hieub26 | viết tools, data | `starter_v0/tools/*`, `artifacts/tools.yaml`, commit `23bc66d` |
| Chu Thùy Dương | 2A202602660 | chuchubeingchaotic | viết system prompt và so sánh v0, v1, v2, v3 | `artifacts/system_prompt.md`, commit `b6df6b1`, `b021274`, PR #1 |
| Lưu Nguyên Khôi | 2A202602547 | w1n10 | Thiết kế UI | `starter_v0/web/*`, `transcripts/*`, commit `ead11cd`, `1d38f27`, `d9b3cf3` |

## Nhận xét chung

- **Kết quả và bằng chứng**:
  - Nhóm chọn đề tài trợ lý bán máy tính (Northstar PC), đã hoàn thành 4 vòng thử nghiệm từ v0 đến v3.
  - Điểm số cải thiện qua từng vòng: **v1** đạt 70.0% (21/30), lên **v2** đạt 91.67% (22/24 ca đo được), và **v3** đạt 100% trên các ca đo được (27/27 pass).
  - Minh chứng lưu đầy đủ tại thư mục `starter_v0/runs/` và bảng `version_log.csv`.
- **Thay đổi hiệu quả nhất**:
  - Phân định rõ giữa text trả lời và lệnh gọi tool: cấm model sinh JSON giả vờ gọi tool (`action: called_tool`) ở các câu hội thoại nhiều lượt.
  - Chặn ranh giới đặt hàng: khi khách có ý định mua máy/đặt đơn thì bắt buộc gọi `clarify(response_type="yes_no")` để hỏi xác nhận trước, cấm tự tra giá hay tạo đơn ngay.
  - Chuẩn hóa tham số cho `clarify`: luôn truyền đủ `response_type` (`text`, `choice`, `yes_no`) đúng ngữ cảnh.
- **Giới hạn còn lại**:
  - Key Gemini Free Tier bị giới hạn 15 requests/phút nên khi chạy benchmark 30 câu liên tục dễ bị lỗi 429 ở vài câu cuối.
  - Dữ liệu sản phẩm và tồn kho hiện tại là file JSON tĩnh, chưa gắn database động.
- **Cách phân công và tích hợp**:
  - Chia việc rõ ràng theo 4 phần: Dữ liệu & test case (Huy), Tool Python (Hiếu), Prompt & Eval (Dương), Web UI (Khôi).
  - Làm việc qua các branch riêng (`huy`, `Hieu`, `chuthuyduong`) rồi tạo Pull Request merge vào `main` sau khi đã tự test chạy thử.

## INDIVIDUAL

### Phùng Quang Minh Huy — 2A202602610

- **Phần việc và file/commit/PR**: Tạo dữ liệu mẫu bán PC (catalog, khách hàng, chính sách) và viết bộ 30 test case cơ bản kèm 12 case an toàn. Files: `pc_seller_data/*`, `data/eval_*.json`. Commit: `db81bb7`.
- **Quyết định, khó khăn và cách xử lý**: Khó khăn là thiết kế các câu test sao cho bao quát cả trường hợp bình thường lẫn ca biên (đổi ý, thiếu linh kiện). Mình đã tổng hợp khoảng 50 linh kiện phổ biến và chia các case theo độ khó để kiểm tra kỹ năng của agent.
- **Điều đã học**: Hiểu tầm quan trọng của bộ benchmark kiểm thử; có bộ test tốt thì mới biết prompt của nhóm tiến bộ hay thụt lùi ở điểm nào.
- **AI/công cụ đã dùng và cách kiểm tra**: Dùng ChatGPT để hỗ trợ gợi ý dữ liệu linh kiện thực tế; kiểm tra tính hợp lệ bằng cú pháp JSON.
- **Thời điểm đã tự nộp URL repo chung trên VLearn**: 

---

### Nguyễn Minh Hiếu — 2A202602669

- **Phần việc và file/commit/PR**: Viết mã nguồn Python cho các tool nghiệp vụ (`check_price`, `search_catalog`, `check_compatibility`, `create_order`, v.v.) và khai báo schema trong `tools.yaml`. Files: `starter_v0/tools/*`, `artifacts/tools.yaml`. Commit: `23bc66d`.
- **Quyết định, khó khăn và cách xử lý**: Khó nhất là hàm `check_compatibility` khi khách chỉ hỏi 1-2 linh kiện thay vì cả bộ. Mình đã xử lý hàm linh hoạt hơn, chỉ so khớp những linh kiện khách đưa ra để tránh lỗi runtime.
- **Điều đã học**: Biết cách thiết kế schema công cụ rõ ràng để LLM bắt đúng tham số, hiểu cách mock data trả về cho agent.
- **AI/công cụ đã dùng và cách kiểm tra**: Dùng GitHub Copilot hỗ trợ code Python; tự viết script test thử từng tool độc lập xem có trả về kết quả đúng không.
- **Thời điểm đã tự nộp URL repo chung trên VLearn**:

---

### Chu Thuỳ Dương — 2A202602660

- **Phần việc và file/commit/PR**: Viết và cải tiến `system_prompt.md` qua các vòng v0-v3, chạy lệnh eval, phân tích nguyên nhân lỗi và ghi log vào `version_log.csv`. Files: `artifacts/system_prompt.md`, `artifacts/versions/*`, `artifacts/version_log.csv`. Commits: `b6df6b1`, `b021274`, PR #1.
- **Quyết định, khó khăn và cách xử lý**: Ở bản v1 model bị lỗi tự in chữ `called_tool` thay vì gọi tool; lên v2 lại bị lỗi tự ý đi tra giá linh kiện trước khi hỏi xác nhận mua. Mình đã bổ sung quy tắc cấm tra giá khi khách yêu cầu đặt đơn và bắt buộc gọi `clarify(response_type="yes_no")`, giúp v3 đạt 100% các ca đo được.
- **Điều đã học**: Hiểu sâu cơ chế function calling của LLM, biết cách đọc log eval để sửa prompt đúng trọng tâm thay vì sửa cảm tính.
- **AI/công cụ đã dùng và cách kiểm tra**: Dùng Gemini trong Antigravity để lọc nhanh các case fail; kiểm tra bằng lệnh `python run_eval.py`.
- **Thời điểm đã tự nộp URL repo chung trên VLearn**: 
---

### Lưu Nguyên Khôi — 2A202602547

- **Phần việc và file/commit/PR**: Phụ trách thiết kế Web UI chat, kết nối API và chạy lưu lại các file transcript demo. Files: `starter_v0/web/*`, `starter_v0/transcripts/*`. Commits: `ead11cd`, `1d38f27`, `d9b3cf3`, `dfe59d3`.
- **Quyết định, khó khăn và cách xử lý**: Ban đầu giao diện web chỉ hiện tin nhắn text mà không thấy được các tool đang chạy bên dưới. Mình đã sửa lại UI để hiển thị thêm phần log tool call kèm tham số, giúp cả nhóm dễ theo dõi và demo.
- **Điều đã học**: Biết cách dựng giao diện chat trực quan cho AI Agent và hiển thị các bước gọi tool minh bạch cho người dùng.
- **AI/công cụ đã dùng và cách kiểm tra**: Dùng Claude hỗ trợ viết nhanh CSS/JS; kiểm tra bằng cách mở web chat thử trực tiếp các kịch bản thực tế.
- **Thời điểm đã tự nộp URL repo chung trên VLearn**: 
