---
title: "Ngày 49 - T5: Text-to-Text Transfer Transformer"
weight: 4
chapter: false
pre: "<b> 1.10.4. </b>"
---

**Ngày:** 2025-11-13 (Thứ Năm)  
**Trạng Thái:** "Hoàn Thành"  

---

# **T5: Mô Hình Transfer Learning Tối Ưu**

Công bố bởi Google vào năm 2019, T5 (Text-to-Text Transfer Transformer) thống nhất TẤT CẢ các tác vụ NLP thành một khung.

Hiểu biết chính: **Mọi tác vụ NLP có thể được diễn đạt dưới dạng text-to-text.**

---

# **Cuộc Cách Mạng T5**

## Trước T5: Các Mô Hình Riêng Theo Tác Vụ

```
Phân Tích Cảm Xúc:
Đầu Vào: "I love this movie!"
Đầu Ra: Positive (phân loại)
Mô Hình: bert-sentiment-specific

Dịch Máy:
Đầu Vào: "I love this movie!"
Đầu Ra: "J'adore ce film!"
Mô Hình: marianmt-en-fr

Tóm Tắt:
Đầu Vào: "The movie was great... [1000 từ]"
Đầu Ra: "Great movie."
Mô Hình: bart-summarization

Trả Lời Câu Hỏi:
Đầu Vào: "What is the capital?" + Đoạn Wikipedia
Đầu Ra: "Paris" (trích xuất span)
Mô Hình: bert-qa-specific

Vấn Đề: Các loại đầu vào/đầu ra khác nhau, các mô hình khác nhau!
```

## Sau T5: Khung Thống Nhất

```
Phân Tích Cảm Xúc:
Đầu Vào:  "sentiment: I love this movie!"
Đầu Ra: "positive"

Dịch Máy:
Đầu Vào:  "translate English to French: I love this movie!"
Đầu Ra: "J'adore ce film!"

Tóm Tắt:
Đầu Vào:  "summarize: The movie was great... [1000 từ]"
Đầu Ra: "Great movie."

Trả Lời Câu Hỏi:
Đầu Vào:  "question: What is the capital? context: Paris is..."
Đầu Ra: "Paris"

Giải Pháp: MỘT mô hình, MỘT kiến trúc, MỘT quy trình huấn luyện!
Chỉ cần thay đổi tiền tố tác vụ!
```

---

# **Kiến Trúc T5**

## Transformer Encoder-Decoder

```
T5 (Cả Hai Lưỡng Chiều Đầy Đủ):

Đầu Vào: "translate English to French: Hello world"
       ↓
Encoder (12 lớp transformers lưỡng chiều)
├─ Lớp 1: Self-attention trên tất cả các token
├─ Lớp 2: Self-attention trên tất cả các token
├─ ...
├─ Lớp 12: Self-attention trên tất cả các token
└─ Đầu Ra: Đại diện ngữ cảnh
       ↓
Decoder (12 lớp, có thể chú ý đến encoder)
├─ Lớp 1: Self-attention (che), Encoder-attention
├─ Lớp 2: Self-attention (che), Encoder-attention
├─ ...
├─ Lớp 12: Self-attention (che), Encoder-attention
└─ Đầu Ra: "Bonjour le monde"

Sự Khác Biệt Chính So Với BERT:
├─ BERT: Encoder only (hiểu)
├─ T5: Encoder + Decoder (hiểu và tạo)
└─ GPT: Decoder only (tạo)
```

## Kích Thước Mô Hình

```
T5-small: 60 triệu tham số
├─ Encoder: 6 lớp, kích thước ẩn 512
├─ Decoder: 6 lớp, kích thước ẩn 512
└─ Nhanh, dấu chân bộ nhớ nhỏ

T5-base: 220 triệu tham số
├─ Encoder: 12 lớp, kích thước ẩn 768
├─ Decoder: 12 lớp, kích thước ẩn 768
└─ Cân bằng tốt

T5-large: 770 triệu tham số
├─ Encoder: 24 lớp, kích thước ẩn 1024
├─ Decoder: 24 lớp, kích thước ẩn 1024
└─ Hiệu suất cao

T5-3B và T5-11B: Thậm chí lớn hơn
├─ Được sử dụng cho các tác vụ rất khó khăn
└─ Hiệu suất ấn tượng trên mọi thứ
```

---

# **Hệ Thống Tiền Tố Tác Vụ**

Thiên tài của T5: **Các tiền tố tác vụ đơn giản hướng dẫn mô hình**

```
Ví Dụ Tiền Tố Tác Vụ:

1. Phân Loại (Cảm Xúc):
   Đầu Vào:  "sentiment: I loved this movie!"
   Đầu Ra: "positive"

2. Dịch Thuật:
   Đầu Vào:  "translate English to French: Hello"
   Đầu Ra: "Bonjour"

3. Tóm Tắt:
   Đầu Vào:  "summarize: Tài liệu dài... [500 từ] ...kết thúc"
   Đầu Ra: "Tóm tắt các điểm chính"

4. Trả Lời Câu Hỏi:
   Đầu Vào:  "question: Who wrote Hamlet? context: Shakespeare..."
   Đầu Ra: "Shakespeare"

5. Paraphrase:
   Đầu Vào:  "paraphrase: The cat is on the mat."
   Đầu Ra: "A feline rests on the floor mat."

6. Entailment:
   Đầu Vào:  "nli: A person is riding a bike. A man cycles."
   Đầu Ra: "entailment" hoặc "neutral" hoặc "contradiction"

7. Zero-shot Classification:
   Đầu Vào:  "znli: I liked it. premise: The speaker liked something."
   Đầu Ra: "entailment"

8. Lựa Chọn Nhiều:
   Đầu Vào:  "multiple_choice: Paris is the capital of?
           (A) France (B) Germany (C) Italy"
   Đầu Ra: "A"

Tại Sao Nó Hoạt Động:
├─ Cùng một mô hình học tất cả các mẫu này
├─ Tiền tố tác vụ hoạt động như một hướng dẫn
├─ Có thể mở rộng quy mô cho bất kỳ tác vụ text-in/text-out nào
└─ Thanh lịch!
```

---

# **T5 Pre-training: SPAN CORRUPTION**

T5 giới thiệu một mục tiêu pre-training mới: **Span Corruption**

## Cách Nó Hoạt Động

```
Gốc: "The quick brown fox jumps over the lazy dog."

Quy Trình Span Corruption:

Bước 1: Chọn ngẫu nhiên các spans để hỏng hóc
├─ Chọn 15% của các token
├─ Nhóm thành các spans
└─ Ví Dụ: [quick brown] và [lazy]

Bước 2: Thay thế các spans bằng các token sentinel
├─ "The [X] fox jumps over the [Y] dog."
├─ [X] là một phần giữ chỗ độc đáo cho span đầu tiên
├─ [Y] là một phần giữ chỗ độc đáo cho span thứ hai

Bước 3: Dự đoán các spans bị mất
Đầu Vào:   "The [X] fox jumps over the [Y] dog."
Đầu Ra:  "[X] quick brown [Y] lazy [Z]"
         (bao gồm token bắt đầu [Z])

Tác Vụ: Tái cấu trúc các spans bị hỏng hóc!
```

## Tại Sao Span Corruption > MLM

```
BERT's MLM:
├─ Mask các token riêng lẻ: "The [MASK] brown [MASK] jumps"
├─ Dự đoán từng cái một cách độc lập
├─ Vấn Đề: Dễ hơn hỏng hóc văn bản thực tế
└─ Các token không tương quan

T5's Span Corruption:
├─ Hỏng hóc các spans cấp độ từ: "The [X] [Y] fox jumps"
├─ Tạo toàn bộ spans
├─ Lợi Ích: Khó hơn, thực tế hơn
└─ Các spans có tương quan (quan trọng cho việc tạo!)

Kết Quả: T5 tốt hơn với các tác vụ tạo!
```

## Công Thức Toán Học

```
Loss span corruption:

Đối với mỗi span s bị hỏng hóc có độ dài L:

Loss = -∑(log P(y_i | y_<i, x))

Trong Đó:
├─ y_i = token i của span bị hỏng hóc
├─ y_<i = các token được tạo trước đó
├─ x = đầu vào với các spans bị hỏng hóc
└─ Sum qua tất cả các token trong span

Về cơ bản đây là ngôn ngữ mô hình loss
Nhưng trên các spans thay vì các token riêng lẻ!
```

---

# **Dữ Liệu Pre-training T5**

## Corpus C4

T5 được huấn luyện trên **C4: Colossal Clean Crawled Corpus**

```
Thống Kê C4:
├─ Nguồn: 750 tỷ tài liệu web
├─ Kích Thước: 800 GB văn bản
├─ 200 tỷ tokens
├─ Xử Lý: Loại bỏ trùng lặp, lọc, làm sạch
├─ Ngôn Ngữ: Chủ yếu tiếng Anh (nhưng tồn tại các phiên bản đa ngôn ngữ)

Cách nó được tạo:
├─ Lấy Common Crawl (ảnh chụp web)
├─ Bộ Lọc: Xóa nội dung xấu
├─ Làm Sạch: Sửa HTML, loại bỏ boilerplate
├─ Loại Bỏ Trùng Lặp: Xóa các bản sao gần đúng
├─ Kết Quả: Corpus 800GB chất lượng cao!

So Sánh:
├─ BERT pre-training: 3.3 tỷ từ pieces (~13GB)
├─ T5 pre-training: 200 tỷ tokens (~800GB)
└─ T5 được huấn luyện trên 60x nhiều dữ liệu!
```

---

# **Pre-training Đa Tác Vụ**

T5 giới thiệu pre-training trên **nhiều tác vụ đồng thời**

```
Trong quá trình pre-training, T5 thấy các mục tiêu đa dạng:

50% Span Corruption (tác vụ chính):
├─ "The [X] fox jumps over [Y] dog"
├─ Dự đoán: "[X] quick brown [Y] lazy"

50% Các mục tiêu riêng theo tác vụ:
├─ Dịch (15%): "translate en to fr: Hello"
├─ Tóm Tắt (15%): "summarize: Article... "
├─ QA (15%): "question: What... context: ..."
├─ Các tác vụ khác (5%): Các tác vụ NLP khác nhau

Tại Sao?
├─ Tiếp xúc với mô hình để đa dạng tác vụ sớm
├─ Chuyển giao tốt hơn sang các tác vụ hạ nguồn
├─ Mô hình học cùng trọng số hoạt động cho nhiều thứ
└─ Dẫn đến các đại diện khái quát hơn
```

---

# **So Sánh T5 với BERT**

```
                T5              BERT
                ──              ────
Kiến Trúc       Enc-Dec         Enc only
Loại Che        Span            Token
Dữ Liệu Pre-train 800GB (C4)    13GB (Wiki+Books)
Quy Mô          220M-11B         110M-340M
Tạo Văn Bản     Xuất Sắc        Không Thể Tạo
Hiểu Biết       Tốt             Xuất Sắc
Thời Gian Huấn Luyện ~31 ngày (TPU) ~4 ngày (TPU)
Tốt Nhất Cho    Tạo Văn Bản    Hiểu Biết
Linh Hoạt       Text-to-text    Fine-tuning riêng theo tác vụ

Cái Nào Để Sử Dụng?
├─ Dịch Thuật: T5 (xử lý chuỗi tốt hơn)
├─ Phân Loại: BERT (nhanh hơn, đơn giản hơn)
├─ Tóm Tắt: T5 (tác vụ tạo)
├─ Cảm Xúc: BERT (tác vụ phân loại)
├─ Trả Lời Câu Hỏi: Cả hai hoạt động, nhưng T5 linh hoạt hơn
```

---

# **Hiệu Suất T5**

## Benchmark GLUE (Hiểu Biết)

```
Tác Vụ                              T5-base    Trước Đây Tốt Nhất
Cảm Xúc (SST-2)                     94.9%      92.1%
Tương Tự (STS-B)                    89.2%      87.1%
Suy Luận (RTE)                      93.5%      91.0%
Trả Lời Câu Hỏi (QNLI)              95.1%      93.2%
Paraphrase (MRPC)                   89.2%      87.1%

Cải Thiện Trung Bình: +2-3%
```

## Điểm BLEU (Tạo Văn Bản)

```
Tác Vụ                              T5-base    Trước Đây Tốt Nhất
Anh sang Đức                         28.4       23.1
Anh sang Pháp                        41.0       35.2
Anh sang Romanian                    34.2       29.1

T5 tốt hơn nhiều ở việc tạo!
```

---

# **Các Biến Thể T5**

```
T5
├─ T5-small (60M)
├─ T5-base (220M)
├─ T5-large (770M)
├─ T5-3B
├─ T5-11B
└─ mT5 (đa ngôn ngữ, 13 biến thể)

Mỗi cái tỷ lệ khác nhau:
├─ Nhỏ hơn = Nhanh hơn, ít bộ nhớ
└─ Lớn hơn = Hiệu suất tốt hơn, nhiều tham số hơn
```

---

# **Tại Sao T5 Quan Trọng**

✅ **Thống Nhất:** Một mô hình, một kiến trúc, nhiều tác vụ
✅ **Tính Đơn Giản Text-to-Text:** Không thiết kế các kiến trúc riêng theo tác vụ
✅ **Span Corruption:** Mục tiêu pre-training tốt hơn
✅ **Dữ Liệu Lớn:** Corpus 800GB cho thấy lợi ích mở rộng quy mô
✅ **Linh Hoạt:** Có thể giải quyết bất kỳ tác vụ tạo nào
✅ **Baseline Mạnh Mẽ:** Hiệu suất tốt nhất trên nhiều benchmark

---

# **Các Lợi Ích Chính**

1. **Tiền Tố Tác Vụ:** Cách đơn giản nhưng mạnh mẽ để hướng dẫn các mô hình
2. **Span Corruption:** Tốt hơn token masking cho việc tạo
3. **Dữ Liệu Pre-training Lớn:** Dữ liệu Hơn = Chuyển giao tốt hơn
4. **Encoder-Decoder:** Hoàn hảo cho các tác vụ seq2seq
5. **Khung Thống Nhất:** Đơn Giản Hóa Hệ Thống NLP

---

# **Sự Phát Triển của Transfer Learning**

```
Word2Vec (2013) → ELMo (2015) → BERT (2018) → T5 (2019) → GPT-3 (2020) → Hiện Tại

Quy Mô:        Đơn Giản → Trung Bình → Lớn → Rất Lớn → Khổng Lồ → Khổng Lồ Hơn?
Kiến Trúc:     Embeddings → BiLSTM → BiTransformer → Enc-Dec → Dec-only
Dữ Liệu:       Triệu → Tỷ → 13GB → 800GB → 570GB → Tỷ tỷ?
Hiệu Suất:     Cơ Bản → Tốt → Xuất Sắc → Tốt Hơn → Tuyệt Vời → Siêu Nhân?
```

---

# **Tiếp Theo: Fine-tuning**

- **Ngày 50:** Cách lấy T5 (hoặc BERT) và fine-tune cho các tác vụ cụ thể

T5 là **mô hình chung sinh rất tốt**. Chỉ với một tiền tố tác vụ và 100-1000 ví dụ, bạn có thể xây dựng các hệ thống hoạt động tốt hơn các mô hình chuyên dụng từ 2 năm trước!
