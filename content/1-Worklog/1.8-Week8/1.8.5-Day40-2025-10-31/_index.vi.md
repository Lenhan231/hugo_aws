---
title: "Ngày 40 - Nhận Dạng Ý Định Tìm Kiếm & Túi Từ"
weight: 5
chapter: false
pre: "<b> 1.8.5. </b>"
---

**Ngày:** 2025-10-31 (Thứ Sáu)  
**Trạng Thái:** "Xong"  

---

# **Công Cụ Nhận Dạng Ý Định Tìm Kiếm**

## Ví Dụ: Tìm Kiếm Bàn Cà Phê

Khi ai đó tìm kiếm **"glass coffee tables"** (bàn cà phê mặt kính), công cụ nhận dạng ý định thực hiện các bước sau:

### Bước 1: Phân Tích Truy Vấn
Hệ thống xác định các thuật ngữ chính:
- **"glass"** → Thuộc tính chất liệu
- **"coffee tables"** → Danh mục sản phẩm
- **"tables"** → Loại đối tượng

### Bước 2: Ánh Xạ Ngữ Nghĩa
Thuật ngữ **"glass"** được ánh xạ tới thuộc tính **'Top Material'** (Chất Liệu Bề Mặt) trong danh mục bàn cà phê thông qua sự hiểu biết ngữ nghĩa và các mô hình tìm kiếm lịch sử.

### Bước 3: Lọc & Xếp Hạng Kết Quả
Công cụ tìm kiếm chỉ dẫn truy vấn đến danh mục bàn cà phê với:
- **Top Material:** glass (được lọc)
- Kết quả xếp hạng theo tính liên quan, đánh giá và phổ biến

### Bước 4: Trả Về Kết Quả
Người dùng thấy các bàn cà phê cụ thể với mặt kính, cải thiện độ chính xác tìm kiếm và sự hài lòng của người dùng.

---

# **Nền Tảng Mô Hình Túi Từ (BoW)**

## Túi Từ là gì?

Mô hình **Túi Từ** là một kỹ thuật nền tảng trong NLP mà:
- Chuyển đổi văn bản thành **biểu diễn vectơ**
- Đếm **tần suất** của mỗi từ
- **Bỏ qua thứ tự từ** và ngữ pháp
- Tập trung vào **sự hiện diện và tần suất của từ**

## Quá Trình BoW:

### 1. Tokenization
Chia văn bản thành các từ riêng lẻ:
- "I love machine learning" → ["I", "love", "machine", "learning"]

### 2. Xây Dựng Từ Vựng
Tạo danh sách duy nhất của tất cả các từ trên các tài liệu

### 3. Tạo Vectơ
Biểu diễn mỗi tài liệu dưới dạng vectơ trong đó:
- Mỗi vị trí = một từ duy nhất
- Mỗi giá trị = tần suất hoặc sự hiện diện (0/1)

### 4. Ví Dụ Vectơ:
```
Tài Liệu: "machine learning is great"
Từ Vựng: [machine, learning, is, great, I, love]
Vectơ: [1, 1, 1, 1, 0, 0]
```

## Ưu Điểm:
- Đơn giản và dễ hiểu
- Tính toán nhanh
- Hoạt động tốt cho phân loại văn bản
- Hiệu quả tốt làm điểm cơ sở cho nhiều tác vụ NLP

## Hạn Chế:
- Mất thông tin về thứ tự từ
- Không nắm bắt ngữ nghĩa hoặc bối cảnh
- Xử lý các từ hiếm và phổ biến như nhau
- Gặp khó khăn với các từ đồng nghĩa

---

# **Tóm Tắt**

Tuần này bao gồm các nền tảng ngôn ngữ học của NLP và các ứng dụng thực tế. Những điểm chính:
- NLP cho phép máy tính hiểu ngôn ngữ con người
- Các thành phần ngôn ngữ cốt lõi (từ âm vị học đến thực dụng) tạo thành nền tảng
- Các ứng dụng thực tế bao gồm tìm kiếm, dịch và hệ thống hội thoại
- Túi Từ là điểm khởi đầu đơn giản nhưng hiệu quả cho xử lý văn bản
