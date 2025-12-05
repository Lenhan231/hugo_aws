---
title: "Ngày 46 - Cơ Bản Transfer Learning"
weight: 1
chapter: false
pre: "<b> 1.10.1. </b>"
---

**Ngày:** 2025-11-10 (Thứ Hai)  
**Trạng Thái:** "Hoàn Thành"  

---

# **Transfer Learning: Tại Sao Quan Trọng**

Transfer learning là **kỹ thuật QUAN TRỌNG NHẤT** trong NLP hiện đại. Nó là lý do tại sao một mô hình được huấn luyện trên Wikipedia có thể được fine-tune trong vài ngày thay vì vài tháng.

---

# **Phương Pháp Cổ Điển vs Transfer Learning**

## Học Máy Cổ Điển

```
Tác Vụ: Dự đoán xếp hạng bộ phim từ đánh giá

Bước 1: Thu thập dữ liệu có gắn nhãn (bộ phim + xếp hạng)
Bước 2: Khởi tạo mô hình ngẫu nhiên
Bước 3: Huấn luyện từ đầu trong vài tuần/tháng
Bước 4: Triển khai

Vấn Đề: Chậm và yêu cầu rất nhiều dữ liệu có gắn nhãn
```

## Phương Pháp Transfer Learning

```
Tác Vụ: Dự đoán xếp hạng bộ phim từ đánh giá

Bước 1: Sử dụng mô hình được huấn luyện trước (huấn luyện trên 800GB văn bản)
Bước 2: Fine-tune trên tập dữ liệu nhỏ của bạn (100 ví dụ)
Bước 3: Huấn luyện trong vài giờ/ngày
Bước 4: Triển khai với hiệu suất tốt hơn!

Lợi Ích: Nhanh và hoạt động với dữ liệu nhỏ
```

---

# **Hai Hình Thức Transfer Learning**

## 1. Transfer Learning Dựa Trên Tính Năng

**Khái Niệm:** Sử dụng các embeddings được huấn luyện trước làm các tính năng cố định

**Ví Dụ:**
```
Tác Vụ Pre-training: Học word2vec embeddings từ Wikipedia

Bước 1: Huấn luyện Word2Vec trên Wikipedia
  "The cat sat on the mat"
  Học: embedding["cat"] = [0.2, -0.5, 0.8, ...]

Bước 2: Sử dụng các embeddings này cho một tác vụ khác
  Tác Vụ: Phân tích cảm xúc trên các đánh giá sản phẩm
  
  Đánh giá: "Great product!"
  Nhúng: "Great" → [0.1, 0.3, 0.9, ...]
         "product" → [0.5, -0.2, 0.1, ...]
  
  Cấp embeddings cho bộ phân loại đơn giản
```

**Ưu Điểm:**
- Đơn giản để triển khai
- Suy luận nhanh (embeddings được tính toán trước)
- Hoạt động với các mô hình hạ nguồn nhỏ

**Nhược Điểm:**
- Embeddings cố định không thích nghi với miền mới
- Bối cảnh không được nắm bắt tốt ("bank" có cùng embedding ở mọi nơi)

---

## 2. Fine-tuning Transfer Learning

**Khái Niệm:** Sử dụng trọng số mô hình được huấn luyện trước và cập nhật chúng cho tác vụ mới

**Ví Dụ:**
```
Tác Vụ Pre-training: Dự đoán từ tiếp theo trên Wikipedia (Language Modeling)

Bước 1: Huấn luyện transformer trên 800GB dữ liệu Wikipedia
  Mô hình học: các mô hình ngôn ngữ, kiến thức thế giới, ngữ pháp

Bước 2: Fine-tune trên tác vụ hạ nguồn
  Đánh giá bộ phim → Dự đoán Xếp hạng
  
  Tùy chọn A: Cập nhật tất cả các lớp
    ├─ Lớp cuối: Fine-tune (hầu hết là chuyên ụng cho tác vụ)
    ├─ Lớp giữa: Fine-tune (một số chuyên ụng cho tác vụ)
    └─ Lớp đầu: Fine-tune (ngôn ngữ chung)
    
  Tùy chọn B: Đóng băng một số lớp, huấn luyện những lớp mới
    ├─ Lớp 1-10: FROZEN (giữ trọng số được huấn luyện trước)
    ├─ Lớp 11-12: Fine-tune
    └─ Đầu phân loại mới: Huấn luyện từ đầu
```

**Ưu Điểm:**
- Trọng số thích nghi với tác vụ mới
- Hiệu suất tốt hơn so với dựa trên tính năng
- Hội tụ nhanh

**Nhược Điểm:**
- Chi phí tính toán (cần cập nhật tất cả các tham số)
- Rủi ro quên thảm họa

---

# **Dữ Liệu Pre-training vs Tác Vụ Hạ Nguồn**

## Sự Khác Biệt Về Quy Mô

```
Pre-training: 800 GB của văn bản
├─ Wikipedia: 13 GB
├─ Sách: 200 GB
├─ Web crawl: 500+ GB
└─ Tổng kiến thức được học: Khổng lồ

Downstream: 100 - 10,000 ví dụ
├─ Đánh giá bộ phim: 5,000 ví dụ
├─ Văn bản y tế: 1,000 ví dụ
└─ Phản hồi khách hàng: 500 ví dụ
```

**Hiểu Biết Chính:** Pre-training có 80,000x nhiều dữ liệu hơn các tác vụ hạ nguồn điển hình! Điều này giải thích tại sao transfer learning hoạt động rất tốt.

---

# **Mục Tiêu Pre-training**

### Mục Tiêu 1: Language Modeling

**Tác Vụ:** Dự đoán từ tiếp theo dựa trên các từ trước đó

```
Đầu Vào: "The quick brown"
Đầu Ra: "fox"

Loss: Cross-entropy giữa dự đoán và từ tiếp theo thực tế

Ví Dụ:
"Learning from deeplearning.ai is like watching the _____"
Mô hình dự đoán: "sunset"
Mục Tiêu: "sunset"
Loss = 0 (dự đoán hoàn hảo!)
```

**Tại Sao Nó Hoạt Động:**
- Mô hình học ngữ pháp, cú pháp, ngữ nghĩa
- Mô hình học các mô hình phổ biến
- Mô hình học kiến thức thế giới

### Mục Tiêu 2: Masked Language Modeling

**Tác Vụ:** Dự đoán các từ được che (ẩn)

```
Đầu Vào: "The quick [MASK] fox"
Đầu Ra: "brown"

Đây là những gì BERT sử dụng!

Phức Tạp Hơn:
"The [MASK] brown [MASK] fox [MASK]"
Dự đoán cả ba: "quick", "jumps", "here"
```

---

# **Lợi Ích của Transfer Learning**

## 1. Giảm Thời Gian Huấn Luyện

```
Huấn Luyện Cổ Điển:             Transfer Learning:
┌─────────────────────┐         ┌──────────┐
│ Pre-training: 3 mo  │         │Pre-train │ (đã làm rồi!)
│ (Từ đầu!)          │         │ (tái sử dụng) │
│                     │         └──────────┘
│ Fine-tuning: 1 mo   │         ┌──────────┐
│ (Trên dữ liệu tác vụ) │       │Fine-tune │ 1-7 ngày!
│                     │         │(trên tác vụ) │
│ Tổng cộng: 4 THÁNG │         └──────────┘
└─────────────────────┘         
                                Tổng cộng: ~1 TUẦN
```

Tăng Tốc Độ: **15-30x nhanh hơn!**

## 2. Cải Thiện Dự Đoán

```
Tập dữ liệu nhỏ (100 ví dụ) với phương pháp cổ điển:
├─ Mô hình overfits (ghi nhớ 100 ví dụ)
├─ Độ chính xác 60% trên tập test
└─ Về cơ bản vô dụng

Tập dữ liệu nhỏ (100 ví dụ) với transfer learning:
├─ Bắt đầu từ mô hình được huấn luyện trước (biết ngôn ngữ!)
├─ Fine-tune cẩn thận
├─ Độ chính xác 85% trên tập test
└─ Tốt hơn nhiều!

Cải Thiện: 25% tốt hơn!
```

## 3. Sử Dụng Các Tập Dữ Liệu Nhỏ Hơn

```
Cổ Điển: "Bạn cần 10,000 ví dụ có gắn nhãn"
Transfer: "100-1000 ví dụ có gắn nhãn là đủ!"

Tại Sao?
  Mô hình được huấn luyện trước đã biết ngôn ngữ
  Bạn chỉ cần dạy nó tác vụ cụ thể của bạn
  Cần ít dữ liệu hơn
```

---

# **Dữ Liệu Pre-training vs Fine-tuning**

## Dữ Liệu Có Gắn Nhãn vs Không Gắn Nhãn

```
Dữ Liệu Không Gắn Nhãn (dùng trong pre-training):
"The quick brown fox jumps over..."
(Không cần nhãn! Chỉ là văn bản thô)

Dữ Liệu Có Gắn Nhãn (dùng trong fine-tuning):
"The quick brown fox jumps over..." → Positive (nhãn)
(Yêu cầu chú thích thủ công)

Hiểu Biết: 
Dữ liệu không gắn nhãn >> Dữ liệu có gắn nhãn về số lượng
Pre-training có thể sử dụng hàng tỷ tokens!
```

---

# **Chiến Lược Transfer Learning**

### Chiến Lược 1: Fine-tuning Nhẹ
```
Đóng băng hầu hết các lớp, huấn luyện chỉ lớp cuối cùng
├─ Trường hợp sử dụng: Mô hình được huấn luyện trước lớn + tập dữ liệu nhỏ
├─ Lớp đóng băng: 1-11
├─ Lớp huấn luyện: 12 + phần đầu phân loại
├─ Thời gian huấn luyện: 1-2 ngày
└─ Rủi ro underfitting: Thấp
```

### Chiến Lược 2: Fine-tuning Đầy Đủ
```
Cập nhật tất cả các lớp
├─ Trường hợp sử dụng: Mô hình được huấn luyện trước trung bình + tập dữ liệu trung bình
├─ Lớp đóng băng: Không có
├─ Lớp huấn luyện: Tất cả 12 + phần đầu phân loại
├─ Thời gian huấn luyện: 3-7 ngày
└─ Rủi ro overfitting: Trung bình
```

### Chiến Lược 3: Progressive Unfreezing
```
Mở khóa các lớp dần dần
├─ Ngày 1: Mở khóa lớp cuối cùng, huấn luyện
├─ Ngày 2: Mở khóa 2 lớp cuối cùng, huấn luyện
├─ Ngày 3: Mở khóa 3 lớp cuối cùng, huấn luyện
├─ ...
├─ Thời gian huấn luyện: 7+ ngày
└─ Hiệu suất tốt nhất: Thường xuyên!
```

---

# **Khi Transfer Learning Thất Bại**

❌ **Không khớp miền:** Pre-training trên tiếng Anh, fine-tune trên tiếng Trung
❌ **Quên thảm họa:** Cập nhật tất cả trọng số quá tích cực
❌ **Fine-tuning quá nhiều:** Sử dụng tỷ lệ học quá cao
❌ **Trích xuất tính năng kém:** Tác vụ pre-train quá khác biệt so với downstream

---

# **Dòng Thời Gian Lịch Sử**

```
2013: Word2Vec
└─ Transfer learning thành công đầu tiên
└─ Embeddings từ đơn giản

2015: ELMo
└─ Bidirectional LSTM
└─ Embeddings nhận thức bối cảnh

2018: GPT
└─ Mô hình ngôn ngữ dựa trên Transformer
└─ Đơn chiều (trái sang phải)

2018: BERT
└─ Transformers lưỡng chiều!
└─ Masked language modeling
└─ Cải thiện hiệu suất khổng lồ

2019: T5
└─ Text-to-text transfer transformer
└─ Multi-task learning
└─ Tiên tiến nhất ở mọi nơi!

2020+: GPT-2, GPT-3, RoBERTa, ELECTRA, XLNet...
└─ Khám phá định luật mở rộng
└─ Mô hình lớn hơn → Hiệu suất tốt hơn
```

---

# **Các Lợi Ích Chính**

✅ **Transfer learning là tiêu chuẩn:** Hầu hết NLP ngày nay sử dụng nó
✅ **Pre-training rất quan trọng:** 800GB pre-training >> 10KB fine-tuning
✅ **Lợi thế tốc độ rất lớn:** Ngày thay vì tháng
✅ **Hiệu quả dữ liệu:** Hoạt động với ít dữ liệu có gắn nhãn hơn nhiều
✅ **Thích ứng miền:** Có thể chuyển giao trên các ngôn ngữ, miền, tác vụ

---

# **Những Gì Chúng Ta Sẽ Học Tiếp**

- **BERT:** Cách bối cảnh lưỡng chiều cải thiện sự hiểu biết
- **MLM:** Masked language modeling pre-training
- **T5:** Khung text-to-text cho tất cả các tác vụ NLP
- **Fine-tuning:** Các mẹo thực tế cho các tác vụ hạ nguồn

Nền tảng này (transfer learning) là lý do tại sao một người bây giờ có thể xây dựng các hệ thống NLP tiên tiến trong vài tuần!
