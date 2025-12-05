---
title: "Ngày 48 - Mục Tiêu Pre-training của BERT"
weight: 3
chapter: false
pre: "<b> 1.10.3. </b>"
---

**Ngày:** 2025-11-12 (Thứ Tư)  
**Trạng Thái:** "Hoàn Thành"  

---

# **Cách BERT Thực Sự Học**

BERT không phải là một dự đoán may mắn. Các nhà khoa học Google đã thiết kế hai tác vụ pre-training cụ thể khiến nó học được sự hiểu biết lưỡng chiều. Những tác vụ này **rất đơn giản một cách xuất sắc**.

---

# **Tác Vụ Pre-training 1: Masked Language Modeling (MLM)**

## Ý Tưởng Cốt Lõi

**Ẩn các từ ngẫu nhiên và dự đoán chúng.**

```
Gốc:  "The quick brown fox jumps over the lazy dog"
Che:  "The quick [MASK] fox jumps over the lazy dog"
Tác Vụ:   Dự đoán [MASK] là gì!
Câu Trả Lời:   "brown"

Tại Sao Nó Hoạt Động:
├─ Mô hình phải hiểu bối cảnh từ CẢ HAI bên
├─ Để dự đoán "brown", cần biết:
│  ├─ Bối cảnh trái: "The quick"
│  └─ Bối cảnh phải: "fox jumps over the lazy dog"
└─ Buộc học lưỡng chiều!
```

## Chiến Lược Che Phủ

Không phải tất cả các token được che phủ theo cách giống nhau:

```
Khi chúng ta chọn một token để che:

80% thời gian: Thay thế bằng token [MASK]
├─ "The quick [MASK] fox"
├─ Mô hình phải học từ
└─ Trường hợp bình thường

10% thời gian: Thay thế bằng từ ngẫu nhiên
├─ "The quick apple fox"  (thay vì "brown")
├─ Mô hình học để sửa đầu vào rườm rà
└─ Huấn luyện mạnh mẽ

10% thời gian: Giữ từ gốc
├─ "The quick brown fox"
├─ Mô hình học đại diện dù sao
└─ Giúp ổn định fine-tuning
```

## Toán Học

```
Đối với mỗi vị trí token i trong 15% từ vựng bị che:

P(mask token tại vị trí i) = 0.15

Loss_MLM = -log(P(từ_chính_xác | bối_cảnh))

Đối với vị trí bị che với từ "brown":
├─ Mô hình đầu ra: [0.02, 0.05, 0.88, 0.03, ...]  (xác suất cho tất cả các từ)
│                                  ↑ vị trí của "brown"
├─ Từ chính xác: "brown" (vị trí 3)
├─ Loss = -log(0.88) = 0.128
└─ Điều này đẩy xác suất cao hơn!

Tổng Hợp: Tổng bình phương loss trên tất cả 15% token bị che
```

## Ví Dụ Đi Bộ

```
Câu: "The bank was robbed by masked men"
Token:   [The, bank, was, robbed, by, masked, men]
Che 15%: [The, bank, [MASK], robbed, by, masked, men]

Bước 1: Tokenize và nhúng tất cả các token
├─ Input IDs: [101, 1996, 2924, 103, 2001, 10122, 2039, 2095, 2273, 102]
├─ Token embeddings: 7 x 768 vectơ chiều
└─ Thêm embeddings vị trí: vị trí 0, 1, 2, 3, ...

Bước 2: Xử lý qua 12 lớp transformer
├─ Lớp 1: Self-attention trên tất cả các vị trí
├─ Lớp 2: Self-attention với các đại diện cập nhật
├─ ...
└─ Lớp 12: Đại diện ngữ cảnh cuối cùng

Bước 3: Dự đoán token bị che
├─ Đầu ra cho vị trí 2 (was): [0.02, 0.05, 0.88, 0.03, ...]
├─ argmax = 0.88 → Word ID 3 → "was"
├─ Chính Xác! ✓
└─ Loss: nhỏ

Bước 4: Backprop gradient qua tất cả 12 lớp
└─ Cập nhật tất cả các tham số để làm cho dự đoán này có khả năng xảy ra
```

---

# **Tác Vụ Pre-training 2: Next Sentence Prediction (NSP)**

## Ý Tưởng Cốt Lõi

**Cho hai câu, dự đoán nếu chúng liên tiếp.**

```
Câu A: "The bank was robbed yesterday."
Câu B: "The police are investigating."
Câu Hỏi:   Đây có phải là câu liên tiếp không?
Câu Trả Lời:     Có (IsNext) ✓

Câu A: "The cat sat on the mat."
Câu B: "I enjoy pizza for lunch."
Câu Hỏi:   Đây có phải là câu liên tiếp không?
Câu Trả Lời:     Không (NotNext) ✗

Tại Sao Nó Giúp:
├─ Mô hình học hiểu mối quan hệ giữa các câu
├─ Quan trọng cho các tác vụ như trả lời câu hỏi
├─ Giúp với sự hiểu biết ngữ nghĩa
└─ Nhưng: Nghiên cứu gần đây cho thấy điều này thực sự ít quan trọng hơn MLM!
```

## Định Dạng Đầu Vào

```
Các token đặc biệt của BERT cho cặp câu:

[CLS] Câu A [SEP] Câu B [SEP]
 ↑                 ↑                ↑
Token cho          Phân Tách        Kết Thúc
phân loại          giữa             chuỗi
(dùng cho NSP)    câu

Segment IDs:
[0     0  0  0 0     0    1  1  1  1 1   1]
 CLS   The bank was robbed SEP police are investigating

Token A = 0 (phân đoạn đầu tiên)
Token B = 1 (phân đoạn thứ hai)

Đại diện token CLS dùng cho phân loại NSP!
```

## Nhiệm Vụ

```
NSP là một vấn đề phân loại nhị phân:

Đầu Vào: [CLS] ... [SEP] ... [SEP]
       ↓
Mô hình đầu ra: [0.1, 0.9]
               ↑     ↑
              NotNext  IsNext
       
Nếu nhãn là IsNext: Loss = -log(0.9) = 0.105
Nếu nhãn là NotNext: Loss = -log(0.1) = 2.303

Mô hình học cách phân biệt các câu liên tiếp!
```

---

# **Hàm Loss Tổng Hợp**

BERT huấn luyện **cả hai** mục tiêu đồng thời:

```
Loss_total = Loss_MLM + Loss_NSP

Ví Dụ Lặp Lại:
├─ Batch chứa 32 cặp câu
├─ Đối với mỗi cặp:
│  ├─ 15% của token bị che → MLM loss
│  ├─ Thứ tự câu được gắn nhãn → NSP loss
│  └─ Cả hai loss được tính toán
├─ Trung bình các loss: 2.15 (MLM) + 1.45 (NSP)
├─ Tổng loss: 3.60
└─ Backprop cập nhật tất cả các tham số

Tại Sao Cả Hai?
├─ MLM buộc hiểu biết lưỡng chiều
├─ NSP buộc ngữ nghĩa ở cấp độ câu
└─ Cùng Nhau: Kiến thức ngôn ngữ phong phú!

Lưu Ý: Các biến thể hiện đại (RoBERTa) loại bỏ NSP
      (Phát hiện rằng MLM một mình là đủ!)
```

---

# **Embeddings Đầu Vào của BERT (Xem Xét Kỹ Lưỡng)**

## Ba Embeddings Được Kết Hợp

```
Embedding của BERT = Token + Segment + Position

Ví Dụ: "The quick brown fox"
Vị Trí: 0    1     2      3

Token Embedding:
├─ "The" (ID=1996):    [0.2, -0.5, 0.1, ...]
├─ "quick" (ID=2522):  [0.1,  0.3, -0.2, ...]
├─ "brown" (ID=2829):  [-0.1, 0.2, 0.4, ...]
└─ "fox" (ID=4397):    [0.3,  0.1, -0.3, ...]

Segment Embedding (cùng câu):
├─ Vị Trí 0: [0.1, 0.2, 0.3, ...]  (Phân Đoạn A)
├─ Vị Trí 1: [0.1, 0.2, 0.3, ...]  (Phân Đoạn A)
├─ Vị Trí 2: [0.1, 0.2, 0.3, ...]  (Phân Đoạn A)
└─ Vị Trí 3: [0.1, 0.2, 0.3, ...]  (Phân Đoạn A)

Positional Embedding:
├─ Vị Trí 0: [0.0, 0.5, -0.1, ...]
├─ Vị Trí 1: [1.0, 0.3,  0.2, ...]
├─ Vị Trí 2: [0.5, -0.1, 0.5, ...]
└─ Vị Trí 3: [-0.3, 0.6, 0.1, ...]

Final Embedding (tổng cả ba):
├─ Vị Trí 0: [0.3,  0.7,  0.3, ...]
├─ Vị Trí 1: [1.1,  0.8,  0.0, ...]
├─ Vị Trí 2: [0.5,  0.3,  0.9, ...]
└─ Vị Trí 3: [0.0,  0.7, -0.2, ...]
```

## Chi Tiết Mã Hóa Vị Trí

```
BERT sử dụng mã hóa vị trí hình sin:

PE(pos, 2i)     = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1)   = cos(pos / 10000^(2i/d_model))

Trong Đó:
├─ pos = vị trí trong chuỗi (0, 1, 2, ...)
├─ i = chỉ số chiều (0, 1, 2, ...)
├─ d_model = 768 (kích thước ẩn)

Đối với vị trí 0:
├─ PE(0, 0) = sin(0) = 0.0
├─ PE(0, 1) = cos(0) = 1.0

Đối với vị trí 1:
├─ PE(1, 0) = sin(1 / 10000^0) = sin(1) = 0.841
├─ PE(1, 1) = cos(1 / 10000^0) = cos(1) = 0.540

Mẫu: Vị trí khác → Mã hóa khác!
Không thể thay đổi hai vị trí và giữ mã hóa giống nhau!
```

---

# **Dữ Liệu Pre-training và Quy Mô**

## Corpus

```
Corpus Pre-training BERT:

Wikipedia: 13 GB
├─ 2,500M từ
├─ Chất lượng cao, các chủ đề đa dạng
└─ Nhưng tương đối nhỏ!

BookCorpus: Không được phát hành công khai nhưng ~12 GB
├─ 800M từ
├─ Sách được viết bởi mọi người → Ngữ pháp tốt
└─ Tài liệu dài (tốt để học các phụ thuộc tầm xa)

Tổng: ~25 GB (3.3 tỷ từ pieces)
├─ Đây là nước xốt bí mật!
├─ Các mô hình hiện đại sử dụng 100-1000x nhiều dữ liệu hơn
└─ Dữ liệu Nhiều Hơn = Hiệu Suất Tốt Hơn (Định Luật Mở Rộng)
```

## Chi Tiết Huấn Luyện

```
Pre-training BERT:

Thời Gian: 4 ngày (BERT-base)
Phần Cứng: 16 thiết bị TPUv2
Kích Thước Lô: 256 chuỗi (512 token mỗi cái) = 131,072 token mỗi lô
Tỷ Lệ Học: 1e-4 với khởi động
Tối Ưu Hóa: Adam
Bước: ~1,000,000 bước

Chi Phí Tính Toán:
├─ 16 TPUs × 4 ngày = 64 TPU-ngày
├─ ~$100,000+ trong tính toán
└─ Đó là tại sao chúng ta sử dụng các mô hình được huấn luyện trước ngây!

Sau Khi Huấn Luyện:
├─ Mô hình có thể được sử dụng cho fine-tuning
├─ Chỉ cần thêm đầu phân loại!
└─ Không cần huấn luyện từ đầu bao giờ!
```

---

# **Tại Sao Những Tác Vụ Này Hoạt Động Rất Tốt**

## Lợi Ích MLM

✅ **Học lưỡng chiều:** Phải sử dụng cả hai bên
✅ **Tác vụ tự nhiên:** Tương tự như đọc con người
✅ **Linh Hoạt:** Có thể che bất kỳ tỷ lệ nào
✅ **Đủ khó:** Dự đoán không tầm thường

## Lợi Ích NSP

✅ **Hiểu biết Diễn Ngôn:** Học các mối quan hệ câu
✅ **Tác Vụ Cặp:** Giúp với QA và NLI
✅ **Định Dạng Phân Loại:** Dễ dàng trích xuất tính năng cho phân loại

## Kết Hợp

✅ **Học Đa Tác Vụ:** Tín Hiệu Giám Sát Phong Phú
✅ **Bổ Sung:** MLM + NSP dạy những thứ khác nhau
✅ **Hiệu Quả:** Một lần chạy huấn luyện, hai tín hiệu học

---

# **So Sánh: Các Mục Tiêu Pre-training Khác Nhau**

```
Word2Vec:
├─ Tác Vụ: Dự đoán các từ lân cận
├─ Kết Quả: Static embeddings
└─ Vấn Đề: Cùng embedding ở mọi nơi

ELMo:
├─ Tác Vụ: Lập Mô Hình Ngôn Ngữ Lưỡng Chiều
├─ Kết Quả: Embeddings nhạy cảm bối cảnh
└─ Vấn Đề: Vẫn xử lý tuần tự

GPT:
├─ Tác Vụ: Dự đoán từ tiếp theo (trái sang phải)
├─ Kết Quả: Tốt cho việc tạo
└─ Vấn Đề: Không thể nhìn thấy bối cảnh tương lai

BERT (MLM + NSP):
├─ Tác Vụ: Dự đoán Che + thứ tự câu
├─ Kết Quả: Hiểu biết lưỡng chiều sâu
└─ Lợi Ích: Cái tốt nhất của cả hai!

T5:
├─ Tác Vụ: Text-to-text với nhiều mục tiêu
├─ Kết Quả: Khung thống nhất cho tất cả các tác vụ
└─ Lợi Ích: Đơn Giản Hơn So Với Hai Loss Riêng Biệt
```

---

# **Các Lợi Ích Chính**

✅ **MLM là ngôi sao:** Quan trọng nhất cho hiệu suất
✅ **Tỷ lệ che 15%:** Được xác định theo kinh nghiệm là tối ưu
✅ **Chiến lược che:** 80-10-10 làm cho mô hình mạnh mẽ
✅ **NSP giúp nhưng là thứ yếu:** Chủ yếu cho các tác vụ được ghép
✅ **Pre-training tốn kém:** Nhưng có thể tái sử dụng!
✅ **Ý tưởng đơn giản, kết quả mạnh mẽ:** Thiên tài của BERT

---

# **Điều Gì Sắp Tới**

- **Ngày 49:** Cách T5 mở rộng quy mô ý tưởng này (và loại bỏ MLM+NSP)
- **Ngày 50:** Fine-tuning các mô hình được huấn luyện trước này

Các mục tiêu pre-training là **nền tảng** của NLP hiện đại. Mọi thứ hạ lưu phụ thuộc vào hai tác vụ đơn giản này!
