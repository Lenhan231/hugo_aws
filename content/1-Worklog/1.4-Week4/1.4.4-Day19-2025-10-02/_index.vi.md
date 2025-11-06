---
title: "Ngày 19 - Disaster Recovery trên AWS"
weight: 4
chapter: false
pre: "<b> 1.4.4. </b>"
---

**Ngày:** 2025-10-02 (Thứ Năm)  
**Trạng thái:** "Hoàn thành"  

---

# **Ghi chú Bài học**

## Disaster Recovery (DR) trên AWS

**Disaster Recovery** là quá trình khôi phục dịch vụ CNTT sau sự cố lớn (mất điện, thiên tai, phần cứng hỏng, tấn công mạng).

- **RTO (Recovery Time Objective):** Thời gian cần để khôi phục dịch vụ.  
- **RPO (Recovery Point Objective):** Mức dữ liệu tối đa có thể mất (theo thời gian).

### Chiến lược DR (tăng dần theo độ phức tạp & chi phí)

**Backup & Restore**

- Chỉ lưu backup (snapshot EBS/RDS, S3/Glacier).  
- Khôi phục hạ tầng mới khi gặp sự cố.  
- **RTO:** vài giờ tới vài ngày. **RPO:** phụ thuộc tần suất backup. **Chi phí:** thấp nhất.

**Pilot Light**

- Duy trì các dịch vụ lõi ở trạng thái thu nhỏ trên AWS.  
- Scale lên toàn bộ sản xuất khi DR.  
- **RTO:** hàng giờ. **RPO:** vài phút. **Chi phí:** trung bình.

**Warm Standby**

- Hệ thống hoàn chỉnh chạy ở quy mô giảm trên AWS.  
- Scale lên khi failover.  
- **RTO:** phút – giờ. **RPO:** giây – phút. **Chi phí:** cao hơn.
