# **Bắt đầu với AWS**
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

<img width="488" alt="image" src="https://github.com/user-attachments/assets/d1b3ee9d-cc13-4b85-952c-907573e10da1" />

Trong một **Region**, có từ **3 đến 6 Availability Zones (AZs)**.

- **Availability Zone (AZ):**  
  Là một cụm **data center**, nhưng các AZ sẽ không đặt cùng một chỗ mà được phân bổ ở các vị trí khác nhau. Chúng kết nối với nhau bằng **mạng băng thông cao** để đảm bảo tính sẵn sàng và tránh thảm họa.  

- **Ví dụ:**  
  - **ap-southeast-2a:** Đây là mã **AZ**, trong khi **ap-southeast** là mã **Region** (châu Á - Thái Bình Dương).

**Khi xây dựng hệ thống Availability:**

- Không thể chỉ xây dựng **một server duy nhất**.
- Ít nhất cần **2 server** và chúng phải được đặt ở **2 AZ khác nhau**.  
- Nếu một server gặp sự cố, server còn lại vẫn có thể tiếp tục hoạt động, giúp đảm bảo tính **sẵn sàng cao**.


## **AWS Points of Presence (Edge Locations)**

<img width="1056" alt="image" src="https://github.com/user-attachments/assets/c74c7c1f-22e9-41c3-b7e5-654587eeca1b" />

AWS Region chỉ có 23 khu vực trên toàn thế giới, nhưng lại có hơn **400 điểm Edge Locations**.  

### **1. Edge Locations là gì?**

**Edge Locations** hoạt động giống như các **access point** (điểm truy cập). Để dễ hiểu, hãy hình dung một văn phòng lớn chỉ có một bộ phát Wi-Fi. Những người làm việc ở xa bộ phát sẽ gặp tình trạng kết nối chậm. Vì vậy, các kỹ sư mạng thường đặt thêm nhiều **access point** ở các vị trí khác nhau trong văn phòng. Nhờ đó, mọi người có thể kết nối tới **access point** gần nhất, giúp cải thiện tốc độ truy cập và giảm độ trễ.

Tương tự, trong AWS, **Edge Locations** là những điểm truy cập đặt tại nhiều nơi trên thế giới. Các yêu cầu từ người dùng sẽ được định tuyến đến **Edge Locations** gần nhất thay vì đi trực tiếp đến **AWS Region**, giúp giảm thiểu độ trễ và cải thiện trải nghiệm người dùng.

### **2. Vai trò của Edge Locations**
Một số máy chủ tại **Edge Locations** sẽ hoạt động ở tầng biên để xử lý và phân phối dữ liệu nhanh hơn. Điều này đặc biệt quan trọng đối với các dịch vụ như:

- **Amazon CloudFront**: Dịch vụ CDN của AWS, sử dụng Edge Locations để phân phối nội dung nhanh chóng đến người dùng cuối.
- **AWS Global Accelerator**: Giúp tăng tốc kết nối tới ứng dụng bằng cách định tuyến qua các Edge Locations gần nhất.

## **CloudFront - cách để giảm thiểu độ trễ khi truy cập server từ xa**

<img width="682" alt="image" src="https://github.com/user-attachments/assets/aadba7f1-4fb7-4461-9eb4-0b95d972d278" />

Khi bạn ở **Việt Nam** và muốn truy cập vào một máy chủ ở **Bắc Mỹ**, yêu cầu sẽ phải đi qua rất nhiều **hub trung gian**, dẫn đến tình trạng **độ trễ cao**. Để giải quyết vấn đề này, người ta sử dụng **Amazon CloudFront**, giúp **cache lại nội dung tĩnh**.  

### **CloudFront – Giảm tải cho server và tăng tốc độ truy cập**

CloudFront đặc biệt hiệu quả khi xử lý các loại **nội dung tĩnh** như:  
- **Video**  
- **Hình ảnh chất lượng cao**  
- **Các blog tĩnh** chứa **HTML**, **CSS**, **JavaScript**  

Khi sử dụng CloudFront:  
- Nội dung được trả về trực tiếp từ **Edge Locations**, nơi CloudFront đặt các điểm cache gần người dùng nhất, mà không cần truy cập vào server chính.  
- CloudFront giao tiếp với server thông qua **mạng nội bộ AWS**, giúp giảm thiểu độ trễ và cải thiện tốc độ tải dữ liệu.  
- Điều này giúp người dùng nhận được phản hồi nhanh hơn, đặc biệt là khi truy cập nội dung nặng hoặc ở khoảng cách xa với server chính.

### **Kiến trúc của CloudFront**

1. **Edge Locations**:  
   Đây là các máy chủ đặt tại nhiều vị trí khác nhau trên toàn thế giới, nơi CloudFront thực hiện việc cache và phân phối nội dung gần với người dùng nhất.

2. **Regional Edge Caches**:  
   Đây là tầng cache cao hơn, nơi các nội dung được lưu trữ trong thời gian dài hơn trước khi hết hạn và cần làm mới từ server chính.  


## **Tour of the AWS Console**

### **1. Global Services**  
Một số dịch vụ AWS là **global**, nghĩa là chúng không thuộc về một **region** cụ thể mà hoạt động trên phạm vi toàn cầu. Các dịch vụ này bao gồm:

- **Identity and Access Management (IAM)**:  
  Quản lý người dùng, nhóm, vai trò và quyền truy cập trên toàn cầu.  
- **Route 53** (*DNS service*):  
  Dịch vụ quản lý DNS và định tuyến trên toàn cầu.  
- **CloudFront** (*Content Delivery Network*):  
  Phân phối nội dung với độ trễ thấp, giúp cải thiện trải nghiệm người dùng cuối.  
- **WAF** (*Web Application Firewall*):  
  Bảo vệ ứng dụng web khỏi các mối đe dọa phổ biến như tấn công SQL Injection và XSS.


### **2. Region-Specific Services**  
Hầu hết các dịch vụ AWS khác thuộc về một **region cụ thể**, tức là chúng được triển khai và quản lý trong từng khu vực địa lý riêng biệt. Ví dụ:

- **Amazon EC2** (*Infrastructure as a Service*):  
  Cung cấp máy chủ ảo trên đám mây, được triển khai theo từng **region**.  
- **Elastic Beanstalk** (*Platform as a Service*):  
  Dịch vụ triển khai và quản lý ứng dụng trong môi trường được phân vùng theo **region**.  
- **Lambda** (*Function as a Service*):  
  Dịch vụ điện toán không máy chủ (*serverless*), hoạt động trong từng **region** riêng biệt.  
- **Rekognition** (*Software as a Service*):  
  Dịch vụ phân tích hình ảnh và video, được triển khai cụ thể theo **region**.

# **Quản lý danh tính và quyền truy cập AWS (AWS IAM)**

## **IAM: Users & Groups**

### **1. IAM (Identity and Access Management)**  
IAM là dịch vụ của AWS cho phép bạn tạo **users** và gán các **roles** phù hợp để quản lý quyền truy cập vào các dịch vụ AWS. Thay vì sử dụng **root account** cho công việc hàng ngày, bạn nên tạo các user cho **dev**, **admin** và các nhân viên khác để đăng nhập và làm việc.


### **2. Root Account**  
- **Root account** là tài khoản bạn tạo khi dùng email để đăng ký AWS.  
- Tài khoản này có toàn quyền kiểm soát tất cả các dịch vụ và tài nguyên trong AWS.  
- Root account chỉ nên được sử dụng cho các trường hợp đặc biệt như:  
  - Cấu hình các thiết lập ở cấp **account** (ví dụ: tham gia vào một group tổ chức lớn).  
  - Quản lý **thanh toán** với AWS.

### **3. Users**  
- **Users** đại diện cho từng cá nhân trong tổ chức của bạn.  
- Mỗi user có thể:  
  - Thuộc về một hoặc nhiều **group**.  
  - Không bắt buộc phải thuộc bất kỳ group nào.  

### **4. Groups**  
- **Groups** đại diện cho các nhóm người dùng trong công ty.  
- Một công ty có thể có nhiều group như: **team BA**, **team Dev**, **team QA**…  
- Mỗi group có thể chứa nhiều **members** và mỗi **user** có thể thuộc về nhiều **group**.

## **IAM: Permissions**

**Permissions** xác định quyền của **user** hoặc **group** về việc họ có thể thực hiện hoặc không thực hiện các hành động trên AWS. Các quyền này được định nghĩa thông qua các **JSON document**, gọi là **policy**.

### **Nguyên tắc cấp quyền**  
Nên tuân thủ nguyên tắc **cấp quyền tối thiểu** (*Least Privilege Principle*), tức là chỉ cấp những quyền cần thiết để người dùng hoàn thành công việc, nhằm giảm thiểu rủi ro về bảo mật.
