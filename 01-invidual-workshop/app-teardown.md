# App Teardown Result: MoMo - Moni AI Chatbot

## 1. Sản phẩm đã dùng thử

* **Sản phẩm**: MoMo — Moni (Trợ thủ tài chính AI)
* **Cách truy cập**: Ứng dụng MoMo → Tab "Moni" (chatbot)

> **Mô tả ảnh (evidence):**
>
> * Người dùng nhập: *"Tôi tiêu tiền quá nhiều vào ăn uống mỗi tháng, bạn có thể giúp tôi phân tích không?"*
> * AI trả lời: *"Chi tiêu ăn uống của bạn trong tháng này là 2,5 triệu đồng."*
> * Không có câu hỏi làm rõ (follow-up question), không gợi ý phân tích sâu hơn, không xác nhận lại nhu cầu thực sự của người dùng.

---

## 2. Promise vs Reality

| Yếu tố                                       | Mô tả                                                                                                                                                                                                                                 |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Product hứa gì?**                          | "Moni là trợ lý tài chính thông minh, giúp bạn hiểu chi tiêu, tối ưu ngân sách và đưa ra gợi ý cá nhân hóa." (theo mô tả trong ứng dụng)                                                                                              |
| **User nào được hứa sẽ được giúp?**          | Người dùng MoMo muốn kiểm soát chi tiêu, đặc biệt là những người muốn hiểu tiền của họ đang được chi vào đâu nhưng không quen với việc tự phân loại giao dịch.                                                                        |
| **Bạn kỳ vọng AI làm được task nào?**        | Hiểu intent đằng sau câu hỏi. Người dùng không chỉ muốn biết số tiền đã chi mà còn muốn được phân loại chính xác, nhận diện các mẫu hành vi chi tiêu và nhận được gợi ý cắt giảm những khoản không cần thiết.                         |
| **Khi dùng thật, điểm gãy xuất hiện ở đâu?** | AI hiểu sai intent. Hệ thống chỉ dựa trên từ khóa "ăn uống" mà không nhận ra đây là một yêu cầu phân tích hành vi chi tiêu tương đối mơ hồ. AI cần xác định thêm phạm vi phân tích hoặc đặt câu hỏi làm rõ trước khi đưa ra kết luận. |

---

## 3. 4 Paths Analysis

| Path                    | Câu hỏi trả lời                                                                               | Quan sát từ ảnh / trải nghiệm                                                                                                                                                                            |
| ----------------------- | --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Happy Path**          | Khi AI đúng và tự tin, user thấy gì?                                                          | AI trả lời đúng số tiền đã chi cho ăn uống. Tuy nhiên người dùng chỉ nhận được dữ liệu, chưa nhận được insight hay lời khuyên hữu ích.                                                                   |
| **Low-confidence Path** | Khi AI không chắc, hệ thống có hỏi lại, hiển thị lựa chọn hoặc chuyển sang hỗ trợ khác không? | Không. AI trả lời một cách chắc chắn (deterministic) ngay cả khi intent còn mơ hồ. Không có cơ chế hỏi làm rõ.                                                                                           |
| **Failure Path**        | Khi AI sai, user biết bằng cách nào và sửa thế nào?                                           | Người dùng khó nhận ra AI đang hiểu sai. Họ chỉ cảm thấy câu trả lời chưa đáp ứng nhu cầu. Không có cơ chế phản hồi hoặc chỉnh sửa hướng phân tích.                                                      |
| **Correction Path**     | Khi user sửa, correction có được lưu lại hay không?                                           | Không có cơ chế ghi nhớ hoặc học từ phản hồi của người dùng. Nếu người dùng nói "Tôi muốn phân tích riêng chi tiêu nhà hàng", hệ thống không thể hiện việc lưu lại sở thích này cho các lần sử dụng sau. |

---

## 4. Finding Written as Product Decision

> Khi người dùng hỏi về chi tiêu trong một danh mục rộng (ví dụ: "ăn uống", "đi lại", "giải trí"), AI chỉ trả lời bằng tổng số tiền đã chi trong danh mục đó mà không phân tích sâu intent của người dùng.
>
> Hậu quả là người dùng không thể rút ra insight về hành vi chi tiêu thực sự và không nhận được các gợi ý tối ưu hóa phù hợp.
>
> Lỗi thuộc layer: **Intent Understanding + UX Recovery**.
>
> Đề xuất: Bổ sung **Low-confidence Path**. Khi phát hiện intent mơ hồ hoặc danh mục quá rộng, AI nên hỏi thêm 1–2 câu làm rõ và đưa ra các lựa chọn phổ biến để người dùng chọn nhanh.

---

## 5. Sketch: As-is / To-be

### As-is (Hiện tại)

```text
[User]
  ↓
"Tôi tiêu tiền quá nhiều vào ăn uống mỗi tháng, bạn có thể giúp tôi phân tích không?"

[Moni AI]
  ↓
Phát hiện từ khóa "ăn uống"
  ↓
Truy xuất tổng chi tiêu trong danh mục
  ↓
"Chi tiêu ăn uống tháng này là 2,5 triệu đồng"
  ↓
Kết thúc hội thoại

[User]
  ↓
Không nhận được insight hoặc hướng hành động tiếp theo
```

### To-be (Đề xuất)

```text
[User]
  ↓
"Tôi tiêu tiền quá nhiều vào ăn uống mỗi tháng, bạn có thể giúp tôi phân tích không?"

[Moni AI]
  ↓
Phát hiện intent còn mơ hồ
  ↓
"Để tôi phân tích chính xác hơn, bạn muốn xem chi tiêu ăn uống ở:
[1] Nhà hàng
[2] Mua thực phẩm
[3] Cà phê, ăn vặt
[4] Toàn bộ"

[User]
  ↓
Chọn [1] Nhà hàng

[Moni AI]
  ↓
"Chi tiêu nhà hàng tháng này là 1,8 triệu đồng,
tăng 22% so với tháng trước.

Nếu giảm 1–2 lần ăn ngoài mỗi tuần,
bạn có thể tiết kiệm khoảng 400.000 đồng/tháng."

  ↓
"Bạn có muốn tôi ghi nhớ sở thích phân tích này cho những lần sau không?
[Có] [Không]"
```

---
