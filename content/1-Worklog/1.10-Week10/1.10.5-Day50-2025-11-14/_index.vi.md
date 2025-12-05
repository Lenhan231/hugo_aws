---
title: "Ngày 50 - Fine-tuning & Ứng Dụng Thực Tế"
weight: 5
chapter: false
pre: "<b> 1.10.5. </b>"
---

**Ngày:** 2025-11-14 (Thứ Sáu)  
**Trạng Thái:** "Hoàn Thành"  

---

# **Tuần 10 Hoàn Thành: Từ Lý Thuyết Đến Sản Xuất**

Bây giờ bạn hiểu transfer learning, BERT và T5. Ngày cuối cùng này bao gồm **cách thực sự triển khai các mô hình này.**

---

# **Fine-tuning: Nghệ Thuật và Khoa Học**

## Bức Tranh Lớn

```
Mô Hình Được Huấn Luyện Trước (ví dụ: BERT-base)
├─ Đã biết tiếng Anh
├─ Đã hiểu ngữ pháp
├─ Đã có một số kiến thức thế giới
└─ Có thể được thích ứng với các tác vụ cụ thể!

Thêm Classification Head
├─ Đơn Giản: Lấy đại diện token [CLS]
├─ Thêm: Dense layer (768 → hidden_size)
├─ Thêm: Classification layer (hidden_size → num_classes)
└─ Kết Quả: Mô hình riêng theo tác vụ

Fine-tune trên dữ liệu của bạn
├─ Đánh giá bộ phim: 5,000 ví dụ
├─ Thời gian huấn luyện: 2-3 giờ trên GPU đơn
├─ Kết Quả: Độ chính xác 94-96%
└─ Triển khai!
```

---

# **Chiến Lược Fine-tuning**

## Chiến Lược 1: Fine-tuning Đầy Đủ

**Cập nhật tất cả các tham số**

```python
# Pseudocode
pretrained_model = load_bert_base()
# Không có lớp đóng băng!
for epoch in range(3):
    for batch in training_data:
        logits = pretrained_model(batch)
        loss = classification_loss(logits, batch.labels)
        loss.backward()
        optimizer.step()
        
# Kết Quả: Hiệu suất tốt nhất
# Thời Gian: 3+ giờ trên GPU
# Bộ Nhớ: Yêu cầu gradient cho tất cả 110M tham số
```

**Khi Nào Sử Dụng:**
- Tập dữ liệu lớn (10,000+ ví dụ)
- Tính toán đủ (GPU/TPU)
- Tác vụ rất khác so với pre-training

---

## Chiến Lược 2: Đóng Băng Lớp

**Đóng băng các lớp sớm, fine-tune các lớp sau**

```python
# Đóng băng 10 lớp đầu tiên
for param in model.bert.encoder.layer[:10].parameters():
    param.requires_grad = False

# Fine-tune lớp 11-12 và phần đầu phân loại
for param in model.bert.encoder.layer[10:].parameters():
    param.requires_grad = True

# Kết Quả: Nhanh, hiệu suất tốt
# Thời Gian: 1-2 giờ trên GPU
# Bộ Nhớ: Chỉ gradient cho 2 lớp + head
```

**Khi Nào Sử Dụng:**
- Tập dữ liệu trung bình (1,000-10,000 ví dụ)
- Tính toán hạn chế
- Tác vụ hơi khác so với pre-training

---

## Chiến Lược 3: Progressive Unfreezing

**Dần dần mở khóa các lớp từ trên xuống dưới**

```
Epoch 1: Chỉ fine-tune phần đầu phân loại
├─ Đóng Băng: Tất cả 12 lớp
├─ Huấn Luyện: Classification head
└─ Tỷ Lệ Học: 1e-3

Epoch 2: Mở Khóa Lớp Cuối Cùng 1
├─ Đóng Băng: Lớp 0-10
├─ Huấn Luyện: Lớp 11 + head
└─ Tỷ Lệ Học: 1e-4

Epoch 3: Mở Khóa 2 Lớp Cuối Cùng
├─ Đóng Băng: Lớp 0-9
├─ Huấn Luyện: Lớp 10-11 + head
└─ Tỷ Lệ Học: 1e-4

...Tiếp tục cho đến khi tất cả mở khóa

Kết Quả: Thường hiệu suất tốt nhất!
Thời Gian: 5+ giờ, nhưng đáng giá cho các tác vụ quan trọng
```

---

# **Lựa Chọn Siêu Tham Số**

## Tỷ Lệ Học

```
Hướng Dẫn Chung:

Đối Với Fine-tuning Đầy Đủ:
├─ Bắt Đầu Với: 5e-5 (rất nhỏ!)
├─ Thử: 2e-5, 3e-5, 5e-5, 1e-4
└─ Không Sử Dụng: Tỷ Lệ Học > 1e-4 (quên thảm họa)

Đối Với Đóng Băng Lớp:
├─ Lớp Đóng Băng: Không Tỷ Lệ Học (không cập nhật)
├─ Lớp Fine-tuned: 1e-4 - 1e-3
└─ Phần Đầu Phân Loại: Có thể sử dụng cao hơn một chút

Tại Sao Lại Nhỏ?
├─ Trọng số được huấn luyện trước đã tốt
├─ Không muốn phá hủy kiến thức
├─ Những thay đổi nhỏ an toàn hơn
```

## Kích Thước Lô

```
Tác Động Kích Thước Lô:

Các Lô Nhỏ (8-16):
├─ Ưu Điểm: Hoạt động với bộ nhớ hạn chế
├─ Nhược Điểm: Gradient ồn ào hơn, không ổn định
├─ Sử Dụng Khi: GPU Nhỏ (< 8GB VRAM)

Các Lô Trung Bình (32):
├─ Ưu Điểm: Cân Bằng Tốt
├─ Nhược Điểm: Sử Dụng Bộ Nhớ Trung Bình
├─ Sử Dụng Khi: GPU Tiêu Chuẩn (8-16GB VRAM)

Các Lô Lớn (64-256):
├─ Ưu Điểm: Đào Tạo Ổn Định, Khái Quát Hóa Tốt
├─ Nhược Điểm: Yêu Cầu Nhiều Bộ Nhớ Hoặc Tích Lũy Gradient
├─ Sử Dụng Khi: TPUs, 24GB+ VRAM, Hoặc Tích Lũy Gradient
```

## Số Lượng Epoch

```
Tác Vụ Phân Loại:
├─ Điển Hình: 3-5 epoch
├─ Tại Sao: Mô hình hội tụ nhanh
└─ Giám Sát: Dừng sớm nếu xác thực ngừng cải thiện

Tác Vụ Tạo (T5):
├─ Điển Hình: 10-20 epoch
├─ Tại Sao: Tác vụ phức tạp hơn, hội tụ chậm hơn
└─ Giám Sát: Điểm BLEU xác thực

Nguyên Tắc Ngón Tay:
├─ Nhiều Dữ Liệu Hơn → Ít Epoch Hơn (ví dụ: 2 epoch cho 100K ví dụ)
├─ Ít Dữ Liệu Hơn → Nhiều Epoch Hơn (ví dụ: 5 epoch cho 1K ví dụ)
```

---

# **Ứng Dụng Fine-tuning Phổ Biến**

## 1. Phân Tích Cảm Xúc

```
Tác Vụ: Phân loại bài đánh giá là dương/âm

Dữ Liệu: 5,000 bài đánh giá bộ phim có nhãn
├─ 80%: Huấn luyện (4,000)
├─ 20%: Xác thực (1,000)

Fine-tuning:
├─ Mô Hình: BERT-base
├─ Epoch: 3
├─ Kích Thước Lô: 32
├─ Tỷ Lệ Học: 2e-5
├─ Thời Gian Huấn Luyện: 30 phút

Kết Quả:
├─ Độ Chính Xác: 94.2%
├─ Độ Chính Xác: 94.5%
├─ Gợi Lại: 93.9%
└─ Tốt hơn nhiều so với huấn luyện từ đầu (78%)!
```

## 2. Nhận Dạng Tên Thực Thể (NER)

```
Tác Vụ: Xác định người, địa điểm, tổ chức trong văn bản

Ví Dụ:
"John Smith works at Google in New York."
Nhãn:  [B-PER, I-PER, O, O, B-ORG, O, B-LOC, I-LOC]

Thách Thức: Phân loại cấp token, không phải cấp câu

Giải Pháp:
├─ Nhận BERT token embeddings
├─ Thêm lớp tuyến tính cho mỗi token
├─ Giải mã bằng CRF (Trường Ngẫu Nhiên Có Điều Kiện)

Thời Gian Fine-tuning: 1-2 giờ
Hiệu Suất: Điểm F1 92%
```

## 3. Trả Lời Câu Hỏi

```
Tác Vụ: Tìm span trả lời trong một đoạn văn

Đầu Vào:
Câu Hỏi: "What is the capital of France?"
Đoạn Văn: "Paris is the capital and most populous city of France..."

Đầu Ra:
Câu Trả Lời: "Paris"

Cách Nó Hoạt Động:
├─ Mã Hóa câu hỏi + đoạn văn cùng nhau
├─ Đối Với Mỗi Token, Dự Đoán: "Đây có phải là bắt đầu của câu trả lời không?"
├─ Đối Với Mỗi Token, Dự Đoán: "Đây có phải là kết thúc của câu trả lời không?"
├─ Trích Xuất Span Giữa Khả Năng Cao Nhất Bắt Đầu Và Kết Thúc

Thời Gian Fine-tuning: 2-3 giờ
Hiệu Suất: Điểm F1 89% trên SQuAD
```

## 4. Tóm Tắt Văn Bản

```
Tác Vụ: Nén các tài liệu dài

Sử Dụng T5:

Đầu Vào:
"The quick brown fox jumps over the lazy dog. 
 This sentence contains all 26 letters of English alphabet.
 It's often used as a test string in computers."

Fine-tuning Với T5:
├─ Tiền Tố: "summarize:"
├─ Huấn Luyện Đầy Đủ: 10-20 epoch
├─ Kích Thước Lô: 16
├─ Tỷ Lệ Học: 5e-5

Đầu Ra:
"A pangram sentence commonly used in computing."

Hiệu Suất: Điểm ROUGE 35-40 (so với 20-25 baseline)
```

## 5. Tương Tự Văn Bản Ngữ Nghĩa

```
Tác Vụ: Đánh Giá Hai Câu Giống Nhau Bao Nhiêu (0-5)

Câu A: "The cat sat on the mat"
Câu B: "A feline rested on the rug"
Nhãn: 4.5 (rất tương tự)

Fine-tuning:
├─ Lấy Token [CLS] Từ Cả Hai Câu
├─ Mã Hóa Cùng Nhau
├─ Regression Head: Dense Layer Để Xuất Điểm (0-5)
├─ Loss: Lỗi Bình Phương Trung Bình (MSE)

Kết Quả:
├─ Tương Quan Với Phán Đoán Của Con Người: 0.88 (rất tốt!)
├─ Tương Quan Spearman: 87%
```

---

# **Những Cạm Bẫy Fine-tuning Cần Tránh**

## ❌ Cạm Bẫy 1: Tỷ Lệ Học Quá Cao

```
Vấn Đề: Mô hình quên kiến thức được huấn luyện trước!

Ví Dụ:
Tỷ Lệ Học: 1e-3
Sau 1 Epoch: Loss = 0.5 (tốt)
Sau 2 Epoch: Loss = 2.0 (khủng khiếp!)
Sau 3 Epoch: Loss = 5.0 (tệ hơn!)

Tại Sao: Các cập nhật lớn phá hủy trọng số hữu ích

Giải Pháp:
├─ Sử Dụng Tỷ Lệ Học Nhỏ Hơn 1-2 bậc
├─ Bắt Đầu Với 2e-5, Tăng Chỉ Nếu Hội Tụ Quá Chậm
└─ Giám Sát: Loss Xác Thực Nên Giảm
```

## ❌ Cạm Bẫy 2: Quá Ít Epoch

```
Vấn Đề: Mô Hình Không Thích Ứng Với Tác Vụ Mới

Ví Dụ:
Dữ Liệu: 5,000 ví dụ
Epoch: 1
Hiệu Suất: Độ chính xác 88%

Cùng Mô Hình Với 3 Epoch:
Hiệu Suất: Độ chính xác 94%!

Tại Sao: 1 Epoch = Mỗi Ví Dụ Thấy Một Lần
         Không đủ để học các mẫu riêng theo tác vụ

Giải Pháp:
├─ Sử Dụng Ít Nhất 3-5 Epoch
├─ Giám Sát Độ Chính Xác Xác Thực
├─ Dừng Sớm Khi Độ Chính Xác Xác Thực Ngừng Cải Thiện
```

## ❌ Cạm Bẫy 3: Overfitting Trên Dữ Liệu Nhỏ

```
Vấn Đề: Mô Hình Ghi Nhớ Thay Vì Khái Quát Hóa

Ví Dụ:
Dữ Liệu Huấn Luyện: 100 ví dụ
Độ Chính Xác Huấn Luyện: 99.8%
Độ Chính Xác Kiểm Tra: 72.0%

Mô Hình Ghi Nhớ!

Giải Pháp:
├─ Thêm Dropout: Bỏ 10-20% Neuron Ngẫu Nhiên
├─ Dừng Sớm: Dừng Khi Độ Chính Xác Xác Thực Tắc
├─ Tăng Cường Dữ Liệu: Tạo Thêm Ví Dụ Từ Các Ví Dụ Hiện Có
├─ Giảm Kích Thước Mô Hình: Sử Dụng BERT-small Thay Vì BERT-large
```

## ❌ Cạm Bẫy 4: Không Tinh Chỉnh Siêu Tham Số

```
Vấn Đề: Siêu Tham Số Mặc Định Không Tối Ưu

Ví Dụ:
Tỷ Lệ Học Mặc Định (1e-4): Độ chính xác 92%
Tỷ Lệ Học Tinh Chỉnh (3e-5): Độ chính xác 95%!

Giải Pháp:
├─ Thử 3-5 Tỷ Lệ Học Khác Nhau
├─ Thử 2-3 Kích Thước Lô Khác Nhau
├─ Thử 3-5 Epoch
├─ Chạy Tập Xác Thực Nhỏ Trên Mỗi Kết Hợp
└─ Chọn Kết Hợp Tốt Nhất Cho Huấn Luyện Đầy Đủ
```

---

# **Cân Nhắc Triển Khai**

## Kích Thước Mô Hình vs Tốc Độ

```
Đối Với Triển Khai Sản Xuất:

BERT-base (110M):
├─ Kích Thước Mô Hình: 440 MB
├─ Thời Gian Suy Luận: 100-150 ms mỗi ví dụ
├─ Độ Chính Xác Tốt
└─ Có thể Phù Hợp Trên Hầu Hết Các Máy Chủ

BERT-large (340M):
├─ Kích Thước Mô Hình: 1.3 GB
├─ Thời Gian Suy Luận: 300-500 ms mỗi ví dụ
├─ Độ Chính Xác Tốt Hơn
└─ Cần Phần Cứng Tốt Hơn

DistilBERT (40M):
├─ Kích Thước Mô Hình: 160 MB (60% nhỏ hơn!)
├─ Thời Gian Suy Luận: 30-50 ms (3x nhanh hơn!)
├─ Độ Chính Xác Hơi Thấp Hơn (97% của BERT)
└─ Hoàn Hảo Cho Các Thiết Bị Di Động/Edge!

Cây Quyết Định:
├─ Độ Chính Xác Tới Hạn? → Sử Dụng BERT-base Hoặc BERT-large
├─ Tốc Độ Tới Hạn? → Sử Dụng DistilBERT Hoặc Lượng Tử Hóa
├─ Cân Bằng? → Sử Dụng BERT-base
```

## Kỹ Thuật Tối Ưu Hóa

```
Trước Triển Khai:

1. Lượng Tử Hóa (8-bit Thay Vì 32-bit):
   ├─ Kích Thước Mô Hình: 1/4 Của Gốc
   ├─ Tốc Độ Suy Luận: Nhanh Hơn 2-4 Lần
   └─ Độ Chính Xác: 95-99% Của Độ Chính Xác Đầy Đủ

2. Chưng Cất Kiến Thức:
   ├─ Huấn Luyện Mô Hình Nhỏ Trên Đầu Ra Của Mô Hình Lớn
   ├─ Kích Thước: Nhỏ Hơn 10 Lần
   ├─ Tốc Độ: Nhanh Hơn 10 Lần
   └─ Độ Chính Xác: 98% Của Mô Hình Giáo Viên

3. Cắt Tỉa:
   ├─ Loại Bỏ Trọng Số Không Quan Trọng
   ├─ Kích Thước: Nhỏ Hơn 30-50%
   ├─ Tốc Độ: Nhanh Hơn 2-3 Lần
   └─ Độ Chính Xác: 98% Của Mô Hình Đầy Đủ

4. TorchScript/ONNX:
   ├─ Biên Dịch Mô Hình Cho Sản Xuất
   ├─ Tốc Độ: Nhanh Hơn 1.5-2 Lần
   └─ Không Phụ Thuộc Framework (TensorFlow, PyTorch, v.v.)
```

---

# **Ví Dụ Thế Giới Thực: Xây Dựng Bộ Phân Loại Cảm Xúc**

## Đường Ống Hoàn Chỉnh

```
Bước 1: Chuẩn Bị Dữ Liệu
├─ Tải: 5,000 bài đánh giá bộ phim có nhãn
├─ Tách: 80% huấn luyện, 20% kiểm tra
├─ Tokenize: Chuyển Đổi Thành Token BERT
└─ Dataloader: Tạo Các Lô Có Kích Thước 32

Bước 2: Tải Mô Hình Được Huấn Luyện Trước
├─ Tải Xuống: BERT-base Từ HuggingFace
├─ Thêm Classification Head: 768 → 2 (nhị phân)
└─ Di Chuyển Đến: GPU

Bước 3: Fine-tune
├─ Tối Ưu Hóa: AdamW (tốt nhất cho transformers)
├─ Tỷ Lệ Học: 2e-5
├─ Epoch: 3
├─ Vòng Lặp Huấn Luyện: Forward Pass → Loss → Backward → Cập Nhật

Bước 4: Đánh Giá
├─ Độ chính xác xác thực: 94.2%
├─ Độ chính xác kiểm tra: 93.8%
└─ Các Số Liệu Từng Lớp: Độ Chính Xác 94%, Gợi Lại 94%

Bước 5: Lưu & Triển Khai
├─ Lưu: model.pt, tokenizer, config.json
├─ Kiểm Tra Trên Đánh Giá Mới: "Best movie ever!" → Positive ✓
└─ Triển Khai Đến Sản Xuất!

Tổng Thời Gian: 2-3 giờ
Tổng Chi Phí: ~$2-5 Trên GPU Đám Mây
Hiệu Suất: Tiên Tiến Nhất!
```

---

# **Lợi Thế Transfer Learning**

## So Sánh

```
Baseline (Huấn Luyện Từ Đầu):
├─ Thời Gian Huấn Luyện: 2-4 tuần
├─ Dữ Liệu Được Yêu Cầu: 100,000+ ví dụ
├─ Độ Chính Xác: 82-85%
├─ Chi Phí: $1000-10000 Trong Tính Toán
└─ Khó: Yêu Cầu Chuyên Môn ML

Transfer Learning (BERT Fine-tuning):
├─ Thời Gian Huấn Luyện: 2-3 giờ
├─ Dữ Liệu Được Yêu Cầu: 100-1000 ví dụ
├─ Độ Chính Xác: 92-95%
├─ Chi Phí: $1-10 Trong Tính Toán
└─ Khó: Có thể sử dụng Thư Viện HuggingFace!

Tăng Tốc Độ: 200-300x nhanh hơn!
Giảm Dữ Liệu: Cần 100x ít dữ liệu!
Kết Quả Tốt Hơn: Độ chính xác cao hơn 10-15%!
```

---

# **Những Lợi Ích Chính**

✅ **Transfer learning là thực tế:** Hoạt động tốt cho các vấn đề thực tế
✅ **Fine-tuning rất đơn giản:** Thêm head + huấn luyện 3-5 epoch
✅ **Tỷ Lệ Học Quan Trọng:** Sử Dụng 1-2 bậc nhỏ hơn
✅ **Tránh Overfitting:** Giám Sát Xác Thực, Sử Dụng Dừng Sớm
✅ **Cân Nhắc Triển Khai:** Tối Ưu Hóa Cho Các Ràng Buộc Của Bạn
✅ **HuggingFace Là Bạn Của Bạn:** Sử Dụng Các Mô Hình Được Huấn Luyện Trước + Thư Viện

---

# **Hành Trình NLP Của Bạn**

Bạn Đã Học:

**Tuần 1-7 (Nền Tảng):** Cơ Bản, Xử Lý Văn Bản, Embeddings
**Tuần 8 (Nền Tảng NLP):** Cơ Bản Ngôn Ngữ, Tìm Kiếm Giọng Nói, Seq2seq, Chú Ý
**Tuần 9 (Transformers):** Self-Attention, Scaled Dot-Product, Cơ Chế Chú Ý, Triển Khai
**Tuần 10 (Transfer Learning):** Transfer Learning, BERT, MLM, T5, Fine-tuning

Từ Hiểu Biết Ngôn Ngữ Đến Xây Dựng Hệ Thống Sản Xuất!

---

# **Các Bước Tiếp Theo**

Để Trở Thành Chuyên Gia NLP:

1. **Xây Dựng Dự Án:** Fine-tune Các Mô Hình Trên Dữ Liệu Thực
2. **Thử Các Mô Hình Khác Nhau:** RoBERTa, ELECTRA, XLNet, GPT-2
3. **Khám Phá Các Kỹ Thuật Nâng Cao:** Prompt Tuning, In-context Learning, RAG
4. **Cập Nhật:** Đọc Các Bài Viết, Theo Dõi Nghiên Cứu (Papers with Code, Twitter)
5. **Đóng Góp:** Các Dự Án NLP Nguồn Mở

---

# **Tài Nguyên**

- **HuggingFace:** https://huggingface.co/ (Models & Thư Viện)
- **BERT Paper:** "BERT: Pre-training of Deep Bidirectional Transformers" (Devlin et al., 2018)
- **T5 Paper:** "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer" (Raffel et al., 2019)
- **Attention Paper:** "Attention Is All You Need" (Vaswani et al., 2017)

---

# **Chúc Mừng!**

Bạn Đã Hoàn Thành 10 Tuần Đào Tạo Toàn Diện NLP. Bây Giờ Bạn Hiểu:
- Transformers Hoạt Động Như Thế Nào (Toán & Triển Khai)
- Cách Transfer Learning Cho Phép Phát Triển Nhanh Chóng
- Cách Fine-tune Các Mô Hình Cho Các Tác Vụ Cụ Thể
- Cách Triển Khai Các Mô Hình Trong Sản Xuất

**Bây Giờ Hãy Xây Dựng Một Cái Gì Đó Tuyệt Vời!** 🚀

Tương Lai Của NLP Không Phải Là Xây Dựng Các Mô Hình Từ Đầu—Đó Là Áp Dụng Sáng Tạo Các Mô Hình Được Huấn Luyện Trước Để Giải Quyết Các Vấn Đề Thực Tế. Bạn Có Nền Tảng Để Làm Chính Xác Điều Đó.
