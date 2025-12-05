---
title: "Tuần 10 - Transfer Learning, BERT & T5"
weight: 10
chapter: false
pre: "<b> 1.10. </b>"
---

**Tuần:** 2025-11-10 to 2025-11-14  
**Trạng Thái:** "Hoàn Thành"  

---

## Tóm Tắt Tuần 10

Tuần cuối cùng này tập trung vào **transfer learning** và các mô hình tiên tiến (BERT và T5). Chúng ta sẽ học cách pre-training trên các tập dữ liệu lớn không được gắn nhãn cải thiện hiệu suất tác vụ hạ nguồn. Từ masked language modeling đến multi-task learning, tuần này tổng hợp mọi thứ thành các hệ thống NLP sẵn sàng sản xuất.

### Các Chủ Đề Chính

#### Cơ Bản Transfer Learning
- Tiếp cận dựa trên tính năng vs fine-tuning
- Pre-training và các tác vụ hạ nguồn
- Dữ liệu có gắn nhãn vs không gắn nhãn
- Lợi ích: đào tạo nhanh hơn, dự đoán tốt hơn, tập dữ liệu nhỏ hơn

#### Kiến Trúc BERT
- Bidirectional Encoder Representations
- Các transformer đa lớp lưỡng chiều
- 12 lớp, 12 attention heads, 110M tham số (BERT-base)
- Kiến trúc encoder-only

#### Mục Tiêu Pre-training
- Masked Language Modeling (MLM): che 15% tokens, dự đoán chúng
- Next Sentence Prediction (NSP): dự đoán nếu các câu theo sau nhau
- Kết hợp loss: MLM + NSP

#### Đầu Vào BERT
- Token embeddings
- Segment embeddings (câu A vs B)
- Positional embeddings
- Token [CLS] để phân loại
- Token [SEP] để phân tách

#### Mô Hình T5
- Text-to-Text Transfer Transformer
- Kiến trúc Encoder-Decoder
- Multi-task training với các tiền tố tác vụ
- Tiên tiến nhất trên 10+ benchmarks

#### Ứng Dụng Fine-tuning
- Phân Tích Cảm Xúc
- Named Entity Recognition (NER)
- Question Answering
- Phát Hiện Paraphrase
- Tương Tự Ngữ Cảnh
- Phân Loại Văn Bản

### Chia Nhỏ Theo Ngày

| Ngày | Tập Trung | Chủ Đề |
|------|-----------|--------|
| 46 | Transfer Learning | Feature-based, Fine-tuning, Pre-training objectives |
| 47 | BERT Architecture | Design, Pre-training, Bidirectionality |
| 48 | Pre-training Tasks | MLM, NSP, Loss functions, Input representation |
| 49 | T5 Model | Encoder-Decoder, Multi-tasking, Task prefixes |
| 50 | Fine-tuning & Apps | Downstream tasks, Practical applications |

### Mục Tiêu Học Tập

- ✅ Hiểu các nguyên tắc transfer learning
- ✅ Nắm bắt bối cảnh lưỡng chiều của BERT
- ✅ Triển khai masked language modeling
- ✅ Học multi-task training với T5
- ✅ Fine-tune models cho các tác vụ hạ nguồn

### So Sánh Mô Hình

| Mô Hình | Loại | Pre-training | Bối Cảnh | Tham Số |
|--------|------|--------------|---------|---------|
| **Word2Vec** | Static | Skip-gram/CBOW | Fixed window | N/A |
| **ELMo** | Dynamic | Language modeling | Bi-directional LSTM | 94M |
| **GPT** | Causal | Language modeling | Uni-directional | 117M |
| **BERT** | Masked | MLM + NSP | Bi-directional | 110M |
| **T5** | Multi-task | Masked span | Both directions | 220M-11B |

### Những Hiểu Biết Chính

✅ **Pre-training quan trọng:** Các mô hình được huấn luyện trên 800GB văn bản vượt trội hơn rất nhiều so với những mô hình được huấn luyện từ đầu
✅ **Lưỡng chiều giúp:** Bối cảnh lưỡng chiều của BERT cải thiện sự hiểu biết
✅ **Multi-task learning hoạt động:** T5 xử lý nhiều tác vụ với một mô hình
✅ **Fine-tuning hiệu quả:** Mất vài ngày thay vì vài tháng
✅ **Dữ liệu không gắn nhãn là mạnh:** Pre-training không sử dụng nhãn, chỉ văn bản thô

---

## Điều Kiện Tiên Quyết

- Hiểu biết hoàn chỉnh về kiến trúc transformer (Tuần 9)
- Quen thuộc với các tác vụ NLP (phân loại, NER, QA)
- Kiến thức PyTorch hoặc TensorFlow rất hữu ích

## Các Bước Tiếp Theo

- Triển khai BERT fine-tuning cho tác vụ của bạn
- Thử nghiệm với T5 cho các vấn đề text-to-text
- Sử dụng thư viện Hugging Face transformers
- Khám phá các mô hình lớn hơn (RoBERTa, ELECTRA, XLNet)
- Triển khai các mô hình được huấn luyện trước trong sản xuất
