---
title: "Sự Kiện 2 - AWS GenAI Builder Club"
weight: 2
chapter: false
---

## AWS GenAI Builder Club: Vòng Đời Phát Triển Do AI Điều Khiển - Tái Tưởng Tượng Kỹ Thuật Phần Mềm

**Ngày & Giờ:** Thứ Sáu, 3 tháng 10 năm 2025 | 14:00 (2:00 PM)

**Địa Điểm:** AWS Event Hall, L26 Tòa Nhà Bitexco, Thành phố Hồ Chí Minh

**Giảng Viên:** Toàn Huỳnh & Mỹ Nguyễn

**Điều Phối Viên:** Diễm Mỹ, Đại Trương, Định Nguyễn

---

## Tổng Quan Sự Kiện

Phiên làm việc AWS GenAI Builder Club này khám phá Vòng Đời Phát Triển Do AI Điều Khiển (AI-DLC), một cách tiếp cận biến đổi đối với kỹ thuật phần mềm tích hợp AI như một cộng tác viên trung tâm trong toàn bộ quá trình phát triển. Phiên làm việc này có các bản trình diễn thực hành về Amazon Q Developer và Kiro, giới thiệu các ứng dụng thực tế của AI trong phát triển phần mềm hiện đại.

---

## Chương Trình

| Giờ           | Phiên                                                                              | Giảng Viên |
| ------------- | ---------------------------------------------------------------------------------- | ---------- |
| 14:00 - 14:15 | Chào Mừng                                                                          | -          |
| 14:15 - 15:30 | Tổng Quan Vòng Đời Phát Triển Do AI Điều Khiển & Bản Trình Diễn Amazon Q Developer | Toàn Huỳnh |
| 15:30 - 15:45 | Giải Lao                                                                           | -          |
| 15:45 - 16:30 | Bản Trình Diễn Kiro                                                                | Mỹ Nguyễn  |

---

## Các Khái Niệm & Bài Học Chính

### 1. Tổng Quan Vòng Đời Phát Triển Do AI Điều Khiển (AI-DLC)

#### Triết Lý Cốt Lõi

Vòng Đời Phát Triển Do AI Điều Khiển đại diện cho một sự thay đổi cơ bản trong cách phần mềm được xây dựng. Thay vì coi AI là một suy nghĩ sau hoặc công cụ hoàn thành mã đơn giản, AI-DLC nhúng AI như một đối tác thông minh trong toàn bộ quá trình phát triển.

**Các Nguyên Tắc Chính:**

- **Bạn Kiểm Soát** - AI là trợ lý của bạn, không phải người quản lý của bạn. Bạn phải duy trì quyền quyết định về hướng dự án và chi tiết triển khai.

- **AI Là Cộng Tác Viên, Không Phải Thay Thế** - AI nên đặt những câu hỏi quan trọng về yêu cầu, kiến trúc và mục tiêu dự án của bạn. Sự hợp tác nên là hai chiều, với bạn hướng dẫn các đề xuất của AI.

- **Lập Kế Hoạch Trước Triển Khai** - Luôn tạo một kế hoạch toàn diện trước khi đi vào mã. AI có thể giúp tạo kế hoạch này, nhưng bạn phải xem xét, xác thực và tinh chỉnh nó.

#### Quy Trình Phát Triển

**Bước 1: Tạo Kế Hoạch Dự Án**

- Xác định rõ ràng yêu cầu và phạm vi dự án
- Yêu cầu AI tạo kế hoạch dựa trên thông số kỹ thuật của bạn
- Xem xét kế hoạch một cách phê phán và yêu cầu sửa đổi
- Đảm bảo kế hoạch chi tiết và rõ ràng

**Bước 2: Chia Nhỏ Thành User Stories**

- Chuyển đổi kế hoạch thành user stories với tiêu chí chấp nhận rõ ràng
- Chia phạm vi lớn thành các đơn vị nhỏ hơn, dễ quản lý
- Mỗi đơn vị trở thành một dự án nhỏ có thể được giao cho các thành viên nhóm
- Ước tính thời gian cho mỗi đơn vị (nhưng cẩn thận với việc ước tính quá cao)

**Bước 3: Xác Định Ngăn Xếp Công Nghệ**

- Chỉ định rõ ràng các công nghệ, framework và công cụ sẽ được sử dụng
- Thay vì bảo AI "đừng triển khai cái này", hãy bảo nó "triển khai theo cách này"
- Hướng dẫn tích cực mang lại tỷ lệ thành công cao hơn các ràng buộc tiêu cực

**Bước 4: Yêu Cầu & Thiết Kế Chi Tiết**

- Viết yêu cầu với độ chính xác và rõ ràng
- Hợp tác với AI để tạo các thông số kỹ thuật chi tiết
- Xác định các mô hình dữ liệu, hợp đồng API và kiến trúc hệ thống
- Tạo tài liệu thiết kế trước khi triển khai bắt đầu

**Bước 5: Triển Khai & Xác Minh**

- Triển khai các tính năng theo kế hoạch
- Sử dụng cách tiếp cận phát triển mob (nhóm làm việc cùng nhau trên mã)
- Xác minh tất cả mã đầu ra như một nhóm
- Tiến hành đánh giá mã và kiểm tra chất lượng

**Bước 6: Kiểm Thử & Triển Khai**

- Di chuyển qua các môi trường: Development (Dev) → Testing (QA) → User Acceptance Testing (UAT) → Production (Prod)
- Đảm bảo các cổng chất lượng ở mỗi giai đoạn
- Xác thực chức năng trước khi phát hành sản xuất

#### Các Yếu Tố Thành Công Quan Trọng

- **Tạo Kế Hoạch Trước** - Đừng mong đợi AI xử lý mọi thứ. Luôn bắt đầu với một kế hoạch rõ ràng.
- **Xem Xét Thường Xuyên** - Liên tục xem xét các đề xuất và đầu ra của AI. Tỷ lệ lỗi cao là có thể.
- **Bạn Là Người Quản Lý** - Giá trị của bạn nằm ở xác thực mã và quản lý dự án, không phải viết từng dòng mã.
- **Đặt Câu Hỏi Làm Rõ** - Đảm bảo AI hiểu ngữ cảnh dự án của bạn bằng cách đặt những câu hỏi quan trọng về yêu cầu, kiến trúc và mục tiêu.
- **Sử Dụng Mẫu Prompt** - Tạo các prompt có cấu trúc bao gồm ngữ cảnh người dùng, user stories và yêu cầu cụ thể để nhận được phản hồi AI rõ ràng hơn.
- **Xuất Kế Hoạch Thành Tệp** - Yêu cầu AI tạo kế hoạch dưới dạng tệp bạn có thể lưu, xem xét và sửa đổi. Điều này tạo ra một tài liệu sống cho tham chiếu trong tương lai.
- **Lịch Sự Với AI** - Duy trì giao tiếp tôn trọng với các công cụ AI. Mối quan hệ tốt có thể giúp ích trong các tương tác trong tương lai (và đó chỉ là thực hành tốt!).

---

### 2. Bản Trình Diễn Amazon Q Developer

#### Amazon Q Developer Là Gì?

Amazon Q Developer là một trợ lý được hỗ trợ bởi AI biến đổi vòng đời phát triển phần mềm (SDLC) thông qua các khả năng tác nhân trên nhiều nền tảng:

- **AWS Console** - Giúp với cấu hình cơ sở hạ tầng và dịch vụ
- **IDE (Integrated Development Environment)** - Cung cấp các đề xuất tạo mã và tối ưu hóa
- **CLI (Command Line Interface)** - Hỗ trợ tạo lệnh và tự động hóa
- **Các Nền Tảng DevSecOps** - Tích hợp các thực tiễn bảo mật vào quy trình phát triển

#### Các Khả Năng Chính

**Tạo Mã & Chất Lượng**

- Tăng tốc độ tạo mã với các đề xuất được hỗ trợ bởi AI
- Cải thiện chất lượng mã thông qua các khuyến nghị thông minh
- Duy trì tích hợp liền mạch với các quy trình công việc hiện có
- Hiểu các cơ sở mã phức tạp và đề xuất tối ưu hóa

**Tài Liệu & Kiểm Thử**

- Tự động tạo tài liệu toàn diện
- Tạo bài kiểm tra đơn vị với nỗ lực thủ công tối thiểu
- Cải thiện đáng kể khả năng bảo trì mã và độ tin cậy
- Giảm boilerplate và các tác vụ mã lặp lại

**Hợp Tác Thông Minh**

- Hoạt động như một cộng tác viên thông minh tận dụng các mô hình ngôn ngữ lớn
- Kết hợp kiến thức dịch vụ AWS sâu sắc với chuyên môn mã hóa
- Giúp các nhà phát triển tăng tốc độ các chu kỳ phát triển
- Nâng cao chất lượng mã và tăng cường tư thế bảo mật

**Tự Động Hóa Trên Toàn Bộ Vòng Đời Phát Triển**

- Tự động hóa các tác vụ thường xuyên trên toàn bộ vòng đời phát triển
- Giảm công việc thủ công, lặp lại
- Cho phép các nhà phát triển tập trung vào các tác vụ sáng tạo có giá trị cao hơn
- Cải thiện năng suất và hiệu quả tổng thể

#### Thực Tiễn Tốt Nhất Khi Sử Dụng Amazon Q Developer

1. **Cung Cấp Ngữ Cảnh Rõ Ràng** - Cung cấp cho Q thông tin chi tiết về dự án, kiến trúc và yêu cầu của bạn
2. **Sử Dụng Prompt Cụ Thể** - Thay vì các yêu cầu mơ hồ, cung cấp các prompt cụ thể, chi tiết với các ví dụ
3. **Xem Xét Các Đề Xuất** - Luôn xem xét các đề xuất của Q trước khi triển khai chúng
4. **Lặp Lại & Tinh Chỉnh** - Nếu đề xuất đầu tiên không hoàn hảo, tinh chỉnh prompt của bạn và thử lại
5. **Tận Dụng Kiến Thức AWS** - Tận dụng sự hiểu biết sâu sắc của Q về các dịch vụ AWS và thực tiễn tốt nhất

---

### 3. Bản Trình Diễn Kiro

#### Kiro Là Gì?

Kiro là một IDE tác nhân (Integrated Development Environment) được phát triển bởi Amazon Web Services lấp khoảng cách giữa việc tạo prototype nhanh được hỗ trợ bởi AI và phát triển phần mềm sẵn sàng cho sản xuất. Nó hiện đang ở giai đoạn xem trước công khai.

#### Triết Lý Cốt Lõi

Kiro thể hiện nguyên tắc rằng AI nên nâng cao năng suất của nhà phát triển trong khi duy trì các tiêu chuẩn chuyên nghiệp, cấu trúc rõ ràng, kiểm thử toàn diện, tài liệu và khả năng bảo trì lâu dài.

#### Các Tính Năng Chính

**Phát Triển Theo Đặc Tả**

- Khi bạn gửi yêu cầu (ví dụ: "thêm hệ thống đánh giá sản phẩm"), Kiro chuyển đổi nó thành:
  - User stories với tiêu chí chấp nhận rõ ràng
  - Tài liệu thiết kế
  - Danh sách tác vụ và kế hoạch triển khai
  - Thông số kỹ thuật có cấu trúc trước khi tạo mã

**Agent Hooks & Tự Động Hóa**

- Tự động kích hoạt các tác vụ dựa trên các sự kiện:
  - Lưu tệp kích hoạt cập nhật tài liệu
  - Commit kích hoạt tạo bài kiểm tra
  - Các hành động cụ thể kích hoạt tối ưu hóa hiệu năng
  - Giảm công việc thủ công, lặp lại

**Steering & Ngữ Cảnh Dự Án**

- Tạo các tệp steering (markdown) để mô tả:
  - Cấu trúc và tổ chức dự án
  - Tiêu chuẩn mã hóa và quy ước
  - Các mô hình kiến trúc mong muốn
  - Hướng dẫn nhóm và thực tiễn tốt nhất
- Giúp Kiro hiểu sâu sắc ngữ cảnh dự án của bạn

**Phân Tích Đa Tệp & Hiểu Ý Định**

- Phân tích nhiều tệp đồng thời
- Hiểu các mục tiêu chức năng trên toàn bộ cơ sở mã
- Thực hiện các thay đổi phù hợp với các mục tiêu dự án tổng thể
- Vượt ra ngoài hoàn thành mã đơn giản

**Tích Hợp VS Code**

- Được xây dựng dựa trên nền tảng mã nguồn mở của VS Code
- Nhập cài đặt, chủ đề và tiện ích mở rộng từ VS Code
- Giao diện quen thuộc cho người dùng VS Code hiện có
- Chuyển đổi liền mạch cho các nhà phát triển

**Lựa Chọn Mô Hình AI Linh Hoạt**

- Hiện sử dụng Claude Sonnet 4 làm mặc định
- Chế độ "Auto" kết hợp nhiều mô hình dựa trên ngữ cảnh
- Cân bằng giữa chất lượng và chi phí
- Linh hoạt để chọn các mô hình khác nhau cho các tác vụ khác nhau

#### Ưu Điểm Của Việc Sử Dụng Kiro

**Tăng Tính Minh Bạch & Kiểm Soát**

- Bắt đầu với các thông số kỹ thuật trước khi tạo mã
- Xem xét và xác thực các thông số kỹ thuật trước khi triển khai
- Giảm mã "ảo tưởng" hoặc triển khai không phù hợp
- Duy trì khả năng truy xuất rõ ràng từ yêu cầu đến mã

**Giảm Boilerplate & Các Tác Vụ Lặp Lại**

- Agent hooks tự động hóa tạo tài liệu
- Tạo bài kiểm tra đơn vị tự động
- Cập nhật thông tin tự động
- Giải phóng các nhà phát triển cho công việc có giá trị cao hơn

**Bảo Mật & Quyền Riêng Tư**

- Hầu hết các hoạt động mã xảy ra cục bộ
- Dữ liệu chỉ được gửi bên ngoài với sự cho phép rõ ràng
- Duy trì kiểm soát thông tin nhạy cảm

**Khả Năng Mở Rộng & Linh Hoạt**

- Tích hợp các công cụ bên ngoài thông qua MCP (Model Context Protocol)
- Hỗ trợ nhiều mô hình AI
- Không bị ràng buộc vào một môi trường AI duy nhất
- Thích ứng với các quy trình làm việc nhóm khác nhau

#### Hạn Chế & Cân Nhắc

- **Trạng Thái Xem Trước** - Vẫn ở giai đoạn xem trước công khai; tính ổn định và tính năng có thể thay đổi
- **Các Dự Án Phức Tạp** - Có thể gặp khó khăn trong việc hiểu ngữ cảnh sâu sắc trong các dự án rất phức tạp
- **Cần Giám Sát** - Người dùng vẫn cần giám sát và xác thực các quyết định của AI
- **Giá Cả Trong Tương Lai** - Các tầng giá dự kiến:
  - Miễn phí: ~50 tác vụ/tháng
  - Pro: ~1.000 tác vụ/tháng
  - Pro+: ~3.000 tác vụ/tháng

#### Khi Nào Nên Sử Dụng Kiro

- Bạn muốn một quy trình làm việc AI + lập trình duy trì tính chuyên nghiệp và cấu trúc rõ ràng
- Xây dựng prototype nhanh nhưng lo ngại về tính bền vững sản xuất
- Khám phá cách AI có thể trở thành một đồng nghiệp lập trình thực sự, không chỉ là công cụ gợi ý mã
- Bạn cần phát triển theo đặc tả với tài liệu và kiểm thử tự động

---

## Các Lỗi Thường Gặp Khi Sử Dụng AI Trong Phát Triển

### 1. Mong Đợi AI Xử Lý Mọi Thứ

**Vấn Đề:** Nhiều nhà phát triển mong đợi AI hoàn thành toàn bộ dự án một cách tự chủ.

**Giải Pháp:** Luôn tạo kế hoạch trước và xem xét thường xuyên. AI là công cụ để nâng cao năng suất, không phải thay thế phán đoán của nhà phát triển.

### 2. Tỷ Lệ Lỗi Cao

**Vấn Đề:** AI có thể mắc lỗi, đặc biệt là trong các tình huống phức tạp.

**Giải Pháp:** Triển khai các chu kỳ xem xét thường xuyên. Xác thực tất cả mã do AI tạo trước khi triển khai.

### 3. Thiếu Yêu Cầu Rõ Ràng

**Vấn Đề:** Yêu cầu mơ hồ hoặc không rõ ràng dẫn đến đầu ra AI mơ hồ.

**Giải Pháp:** Viết yêu cầu với độ chính xác. Hợp tác với AI để tạo các thông số kỹ thuật chi tiết trước khi triển khai.

### 4. Ràng Buộc Tiêu Cực Thay Vì Hướng Dẫn Tích Cực

**Vấn Đề:** Bảo AI "đừng làm cái này" ít hiệu quả hơn "làm cái này".

**Giải Pháp:** Sử dụng các hướng dẫn tích cực, cụ thể. Tỷ lệ thành công cao hơn đến từ hướng dẫn tích cực rõ ràng.

### 5. Ngữ Cảnh Dự Án Không Đủ

**Vấn Đề:** AI không hiểu các yêu cầu và ràng buộc độc đáo của dự án của bạn.

**Giải Pháp:** Tạo các tệp steering, cung cấp ngữ cảnh chi tiết và đặt những câu hỏi quan trọng cho AI về dự án của bạn.

### 6. Coi AI Là Người Quản Lý

**Vấn Đề:** Để AI quyết định tất cả về hướng dự án và kiến trúc.

**Giải Pháp:** Nhớ: **Bạn là người quản lý.** Giá trị của bạn nằm ở xác thực mã và giám sát dự án, không phải viết từng dòng mã.

---

## Những Điểm Chính Rút Ra

1. **AI Là Trợ Lý Của Bạn** - Duy trì kiểm soát các quyết định dự án và hướng triển khai

2. **Lập Kế Hoạch Trước, Mã Sau** - Luôn tạo kế hoạch toàn diện trước khi triển khai

3. **Hợp Tác Hơn Tự Động Hóa** - AI nên đặt câu hỏi và hợp tác, không chỉ thực thi lệnh

4. **Yêu Cầu Rõ Ràng Quan Trọng** - Độ chính xác trong yêu cầu dẫn đến đầu ra AI tốt hơn

5. **Xem Xét Thường Xuyên Là Cần Thiết** - Đừng mong đợi AI hoàn hảo; xem xét và xác thực liên tục

6. **Bạn Là Người Quản Lý Mã** - Giá trị của bạn nằm ở xác thực và giám sát, không phải viết từng dòng

7. **Sử Dụng Prompt Có Cấu Trúc** - Các mẫu với ngữ cảnh, user stories và yêu cầu mang lại kết quả tốt hơn

8. **Xuất Kế Hoạch Thành Tệp** - Tạo các tài liệu sống bạn có thể tham chiếu và sửa đổi

9. **Hướng Dẫn Tích Cực Hiệu Quả Hơn** - Bảo AI phải làm gì, không phải tránh làm gì

10. **Kinh Nghiệm Quan Trọng** - Sử dụng các công cụ này thực hành để hiểu khả năng và hạn chế của chúng

---

## Công Cụ & Tài Nguyên Được Đề Xuất

- **Amazon Q Developer** - Trợ lý phát triển được hỗ trợ bởi AI tích hợp với các dịch vụ AWS
- **Kiro IDE** - Môi trường phát triển theo đặc tả với hợp tác AI
- **AWS CodeWhisperer** - Công cụ tạo mã và tối ưu hóa
- **MCP (Model Context Protocol)** - Khung công tác để tích hợp các công cụ và dịch vụ bên ngoài

---

## Kết Luận

Vòng Đời Phát Triển Do AI Điều Khiển đại diện cho một mô hình mới trong kỹ thuật phần mềm nơi AI và con người hợp tác như những người bình đẳng. Thành công đòi hỏi lập kế hoạch rõ ràng, xem xét thường xuyên, yêu cầu chính xác và duy trì kiểm soát của nhà phát triển đối với hướng dự án. Các công cụ như Amazon Q Developer và Kiro đang cho phép quy trình làm việc mới này, nhưng chúng hoạt động tốt nhất khi các nhà phát triển hiểu khả năng và hạn chế của chúng, và duy trì vai trò của họ là người quản lý dự án và xác thực mã.
