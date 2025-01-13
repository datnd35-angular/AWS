## **AWS Cloud History**

![image](https://github.com/user-attachments/assets/5f90e0b3-2e6e-4ea4-9dbf-95e3a1d38c24)

## **AWS Global Infrastructure**


![image](https://github.com/user-attachments/assets/6938d147-06d4-4960-9afb-8cd1d8ef4505)

### **1. AWS Regions : Khu vực địa lý nơi AWS đặt data center**

Hiện tại, AWS có hơn **23 Region** và một số Region mà AWS đang có kế hoạch đặt **data center** trong tương lai.  

- Mỗi **Region** có tên và mã riêng, ví dụ: **us-east-1** (Bắc Mỹ), tương ứng với vị trí **data center** ở đó.  

**Các yếu tố cần cân nhắc khi chọn Region:**

1. **Compliance (Tuân thủ quy định):**  
   - Cần tuân thủ các quy định của chính phủ về việc lưu trữ dữ liệu, tránh để dữ liệu ra ngoài quốc gia.  

2. **Proximity to Customers (Gần khách hàng):**  
   - Lựa chọn Region gần với khách hàng để **giảm độ trễ** khi người dùng tương tác với dịch vụ.  

3. **Available Services within a Region (Dịch vụ có sẵn trong Region):**  
   - Kiểm tra xem các dịch vụ mà bạn muốn sử dụng có sẵn trong Region đó không, vì không phải Region nào cũng có đủ dịch vụ như mong muốn.  

4. **Pricing (Giá cả):**  
   - Mức giá của dịch vụ, ví dụ như **EC2**, có thể khác nhau giữa các Region.
  
### **2. Availability Zones trong một Region**

Trong một **Region**, có từ **3 đến 6 Availability Zones (AZs)**.

- **Availability Zone (AZ):**  
  Là một cụm **data center**, nhưng các AZ sẽ không đặt cùng một chỗ mà được phân bổ ở các vị trí khác nhau. Chúng kết nối với nhau bằng **mạng băng thông cao** để đảm bảo tính sẵn sàng và tránh thảm họa.  

- **Ví dụ:**  
  - **ap-southeast-2a:** Đây là mã **AZ**, trong khi **ap-southeast** là mã **Region** (châu Á - Thái Bình Dương).

**Khi xây dựng hệ thống Availability:**

- Không thể chỉ xây dựng **một server duy nhất**.
- Ít nhất cần **2 server** và chúng phải được đặt ở **2 AZ khác nhau**.  
- Nếu một server gặp sự cố, server còn lại vẫn có thể tiếp tục hoạt động, giúp đảm bảo tính **sẵn sàng cao**.
