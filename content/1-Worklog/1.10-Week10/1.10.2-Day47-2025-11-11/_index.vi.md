---
title: "Ngày 47 - Kiến Trúc BERT"
weight: 2
chapter: false
pre: "<b> 1.10.2. </b>"
---

**Ngày:** 2025-11-11 (Thứ Ba)  
**Trạng Thái:** "Hoàn Thành"  

---

# **BERT: Bidirectional Encoder Representations from Transformers**

BERT là bước ngoặt thay đổi NLP mãi mãi. Công bố bởi Google vào năm 2018, nó cho thấy rằng **bối cảnh lưỡng chiều** quan trọng hơn mọi người nghĩ.

---

# **Cái Gì Khác Biệt Về BERT?**

## Sự Phát Triển

```
Word2Vec (2013)
├─ Embedding: "cat" → [0.2, -0.5, 0.8, ...]
├─ Vấn Đề: Cùng embedding ở mọi nơi, không bối cảnh
└─ Phương Pháp: Đơn giản, tĩnh

↓

ELMo (2015)
├─ BiLSTM: Xử lý trái VÀ phải
├─ Vấn Đề: Vẫn tuần tự, chậm hơn
└─ Phương Pháp: Lưỡng chiều nhưng không song song

↓

GPT (2018)
├─ Transformer: Chú ý song song!
├─ Vấn Đề: Chỉ nhìn TRÁI (trái sang phải)
│         "The bank was robbed" → Không thể sử dụng "robbed" để hiểu "bank"
└─ Phương Pháp: Đơn chiều, hạn chế

↓

BERT (2018) ⭐
├─ Transformer: Song song VÀ lưỡng chiều!
├─ Giải Pháp: Nhìn CẢ HAI bên của mỗi từ
│            "The bank was robbed" → Sử dụng cả "The" và "was robbed" để hiểu "bank"
└─ Phương Pháp: Bối cảnh lưỡng chiều đầy đủ
```

---

# **Tổng Quan Kiến Trúc BERT**

## Thông Số Mô Hình

```
BERT-base:
├─ Lớp Transformer: 12
├─ Kích thước ẩn: 768
├─ Đầu chú ý: 12 (768/12 = 64 mỗi đầu)
├─ Kích thước feed-forward: 3072
├─ Tổng tham số: 110 Triệu
├─ Thời gian huấn luyện: 4 ngày trên 16 TPUs
└─ Dữ liệu huấn luyện: 3.3 tỷ từ từ Wikipedia + BookCorpus

BERT-large:
├─ Lớp Transformer: 24
├─ Kích thước ẩn: 1024
├─ Tổng tham số: 340 Triệu
└─ Chậm hơn nhiều nhưng hiệu suất cao hơn
```

## Kiến Trúc Encoder-Only

```
BERT vs Transformer:

Transformer (Đầy Đủ):
Input → Encoder (6 lớp) → Decoder (6 lớp) → Output
        (Có thể nhìn tất cả) (Che, không thể nhìn tương lai)

BERT (Encoder Only):
Input → Encoder (12 lớp) → Output
        (Chú ý lưỡng chiều đầy đủ!)

        
Khác Biệt Chính:
├─ GPT = Encoder + Decoder nhưng decoder là đơn chiều
├─ BERT = Encoder only, lưỡng chiều đầy đủ
└─ T5 = Encoder + Decoder, cả hai lưỡng chiều
```

---

# **Cơ Chế Self-Attention (Ôn Lại)**

Nhớ lại từ Ngày 43:

```
Đối với mỗi từ, tạo ba vectơ:
├─ Query (Q): "Tôi cần thông tin gì?"
├─ Key (K): "Tôi cung cấp thông tin gì?"
└─ Value (V): "Đại diện thực của tôi là gì?"

Điểm Chú Ý = softmax(Q·K^T / √d) · V

Ví Dụ: Xử lý "bank" trong "The bank was robbed"

Query cho "bank": [0.1, 0.2, 0.3, ...]
Keys cho tất cả các từ:
├─ "The" key:  [0.2, 0.1, 0.1, ...]  → Điểm: 0.7
├─ "bank" key: [0.1, 0.2, 0.3, ...]  → Điểm: 0.95 (cao! self-attention)
├─ "was" key:  [0.5, 0.2, 0.1, ...]  → Điểm: 0.8
└─ "robbed" key: [0.3, 0.3, 0.1, ...]  → Điểm: 0.85

Chú Ý: "robbed" giúp hiểu "bank"!
Đây là lý do tại sao BERT là lưỡng chiều.
```

---

# **BERT vs GPT: Sự Khác Biệt Chính**

## Che Phủ Chú Ý

### GPT (Trái sang Phải, Kiểu Decoder)

```
Xử lý: "The bank was robbed"

Vị Trí 1 ("The"):
├─ Có thể chú ý đến: "The" ✓
├─ Có thể chú ý đến: "bank" ✗ (che, tương lai)
├─ Có thể chú ý đến: "was" ✗ (che, tương lai)
└─ Có thể chú ý đến: "robbed" ✗ (che, tương lai)

Vị Trí 4 ("robbed"):
├─ Có thể chú ý đến: "The" ✓
├─ Có thể chú ý đến: "bank" ✓
├─ Có thể chú ý đến: "was" ✓
└─ Có thể chú ý đến: "robbed" ✓

Vấn Đề: "The" không thể sử dụng "robbed" để hiểu ý nghĩa của nó!
```

### BERT (Lưỡng Chiều, Kiểu Encoder)

```
Xử lý: "The bank was robbed"

Vị Trí 1 ("The"):
├─ Có thể chú ý đến: "The" ✓
├─ Có thể chú ý đến: "bank" ✓
├─ Có thể chú ý đến: "was" ✓
└─ Có thể chú ý đến: "robbed" ✓

Vị Trí 4 ("robbed"):
├─ Có thể chú ý đến: "The" ✓
├─ Có thể chú ý đến: "bank" ✓
├─ Có thể chú ý đến: "was" ✓
└─ Có thể chú ý đến: "robbed" ✓

Lợi Ích: "The" CÓ THỂ sử dụng "robbed" để hiểu bối cảnh!
Đại diện tốt hơn cho TẤT CẢ các từ!
```

---

# **Tại Sao Lưỡng Chiều Tốt Hơn**

## Ví Dụ Hiểu Biết Ngôn Ngữ

```
Câu: "I went to the bank to deposit money"
Từ: "bank" (tổ chức tài chính)

Xử Lý GPT (Trái sang Phải):
├─ Có thể nhìn: "I went to the"
├─ Không thể nhìn: "to deposit money" (tương lai!)
├─ Hiểu biết bối cảnh: 30% (không đầy đủ)

Xử Lý BERT (Lưỡng Chiều):
├─ Có thể nhìn: "I went to the" + "to deposit money"
├─ Bối cảnh đầy đủ: 100%
├─ Hiểu biết bối cảnh: 90% (tốt hơn nhiều!)
```

## Đơn Nghĩa Hóa Nghĩa Của Từ

```
"I went to the bank to deposit money"      → Tổ chức tài chính
"The river bank was very scenic"           → Đất dọc theo sông
"The bank was robbed yesterday"             → Công ty/tổ chức

GPT: Chỉ nhìn bối cảnh bên trái
├─ "I went to the" (không đầy đủ)
├─ Rủi ro: Nghĩa sai!

BERT: Nhìn bối cảnh đầy đủ
├─ "I went to the _____ to deposit money"
├─ Tự động đơn nghĩa hóa! ✓
```

---

# **Đại Diện Đầu Vào Của BERT**

## Embeddings Token

```
BERT sử dụng WordPiece tokenization:

Đầu Vào: "The bank was robbed"

Tokenization:
├─ "The" → [101] (Token CLS thêm vào đầu)
├─ "bank" → [1998]
├─ "was" → [2003]
├─ "robbed" → [6861]
├─ "[SEP]" → [102] (Token phân tách thêm vào cuối)

Token thô: [101, 1998, 2003, 6861, 102]
```

## Ba Loại Embeddings

```
BERT kết hợp ba loại embedding:

Token Embedding: "bank" → [0.2, -0.5, 0.8, ...]
├─ Đại diện từ cụ thể
├─ Học được trong pre-training

Segment Embedding: Câu A vs Câu B → [0.1, 0.2, ...]
├─ Đánh dấu câu nào mỗi token thuộc về
├─ Cho các tác vụ cặp câu (Next Sentence Prediction)

Positional Embedding: Vị Trí 2 → [0.5, 0.1, ...]
├─ Mã hóa vị trí trong chuỗi
├─ 0, 1, 2, 3, 4...

Final Embedding = Token + Segment + Positional
                = [0.2, -0.5, 0.8, ...] + [0.1, 0.2, ...] + [0.5, 0.1, ...]
                = [0.8, -0.2, 1.3, ...]
```

---

# **So Sánh: BERT vs GPT vs Transformer Gốc**

```
                BERT          GPT              Transformer
                ────          ───              ─────────────
Encoder-Decoder Encoder only  Encoder+Decoder  Encoder+Decoder
Loại Chú Ý      Lưỡng chiều   Đơn chiều        Hỗn hợp
Tác Vụ Pre-train MLM + NSP    Language Model   Không (N/A)
Hướng            Cả hai        Trái sang phải   Decoder che
Tốt Nhất Cho     Hiểu Biết     Tạo Văn Bản     Dịch
Hiệu Suất        Tốt Nhất (90%) Tốt (85%)      Thay Đổi (thay đổi%)
Tốc Độ          Trung Bình    Nhanh           Chậm Nhất
Tham Số         110M/340M     117M/345M       ~100M
```

---

# **Hiệu Suất Của BERT Trên Các Tác Vụ NLP**

## Kết Quả Benchmark GLUE

```
Trước BERT (Tiên Tiến Nhất 2017):
├─ Cảm Xúc: 92.1% độ chính xác
├─ Tương Tự Văn Bản: 80.5% Spearman
├─ Tên Thực Thể: 91.6% F1
└─ Trả Lời Câu Hỏi: 78.2% F1

BERT-base:
├─ Cảm Xúc: 94.9% độ chính xác (+2.8%)
├─ Tương Tự Văn Bản: 85.8% Spearman (+5.3%)
├─ Tên Thực Thể: 96.4% F1 (+4.8%)
└─ Trả Lời Câu Hỏi: 84.2% F1 (+6.0%)

Cải Thiện: 4-6% trên toàn bộ!
Đối với một hệ thống được triển khai, đó là rất lớn!
```

---

# **Tại Sao BERT Trở Thành Rất Nổi Tiếng**

1. **Fine-tuning Đơn Giản:** Thêm một đầu phân loại, fine-tune tất cả các lớp
2. **Hiệu Suất Mạnh:** Vượt qua tất cả các phương pháp trước đó bằng các biên độ lớn
3. **Trọng Số Được Huấn Luyện Trước:** Có thể tải xuống trọng số, sử dụng ngay lập tức
4. **Đa Ngôn Ngữ:** BERT được huấn luyện trên 104 ngôn ngữ (mBERT)
5. **Có Thể Chuyển Giao:** Hoạt động cho hầu hết mọi tác vụ NLP

---

# **Những Hạn Chế của BERT**

⚠️ **Lưỡng chiều = Không thể tạo văn bản trực tiếp**
```
Vấn Đề: Bạn không thể làm "cho 'The bank', tạo phần còn lại"
Lý do: Mỗi token có thể nhìn thấy tương lai!
Sẽ sụp đổ tác vụ tạo văn bản.
Giải Pháp: Fine-tune cho phân loại, không phải tạo văn bản
```

⚠️ **Cửa sổ bối cảnh cố định: 512 token tối đa**
```
Các tài liệu dài không phù hợp!
Giải Pháp: BERT phân cấp hoặc RoBERTa (4096 token)
```

⚠️ **Tính toán tốn kém**
```
110M tham số = 440MB chỉ cho trọng số
Suy luận chậm hơn các mô hình nhỏ hơn
Giải Pháp: Chưng cất (DistilBERT, 40% tham số, 60% tốc độ)
```

---

# **Các Biến Thể của BERT**

```
BERT
├─ BERT-base: Gốc, 110M tham số
├─ BERT-large: Lớn hơn, 340M tham số
├─ mBERT: 104 ngôn ngữ, đa ngôn ngữ
├─ DistilBERT: Chưng cất, 40M tham số, nhanh hơn
└─ RoBERTa: Huấn luyện cải thiện, 125M tham số

Tất cả dựa trên cùng kiến trúc encoder-only
```

---

# **Các Lợi Ích Chính**

✅ **Bối cảnh lưỡng chiều quan trọng:** Cái nhìn sâu sắc về bước ngoặt BERT
✅ **Kiến Trúc Encoder-Only:** Hoàn hảo để hiểu
✅ **Trọng số được huấn luyện trước là vàng:** Không huấn luyện từ đầu!
✅ **Self-attention với bối cảnh đầy đủ:** Đại diện tốt hơn
✅ **Fine-tuning Đơn Giản:** Thêm đầu + huấn luyện 1-3 epoch

---

# **Điều Gì Sắp Tới**

- **Ngày 48:** Cách BERT học được sự hiểu biết này (MLM + NSP)
- **Ngày 49:** T5 - Mở rộng quy mô đến encoder-decoder
- **Ngày 50:** Fine-tuning BERT cho các tác vụ thực tế

Vẻ đẹp của BERT: Transformer lưỡng chiều cho thấy rằng **nhìn thấy mọi thứ cùng một lúc** là chìa khóa để hiểu ngôn ngữ.
