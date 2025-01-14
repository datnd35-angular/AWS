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

<img width="611" alt="image" src="https://github.com/user-attachments/assets/9fb7ec76-fc0a-426e-8657-afc69aedf7ab" />

**Permissions** xác định quyền của **user** hoặc **group** về việc họ có thể thực hiện hoặc không thực hiện các hành động trên AWS. Các quyền này được định nghĩa thông qua các **JSON document**, gọi là **policy**.

### **Nguyên tắc cấp quyền**  
Nên tuân thủ nguyên tắc **cấp quyền tối thiểu** (*Least Privilege Principle*), tức là chỉ cấp những quyền cần thiết để người dùng hoàn thành công việc, nhằm giảm thiểu rủi ro về bảo mật.


## **IAM Policies Inheritance**


<img width="1109" alt="image" src="https://github.com/user-attachments/assets/7673fcbf-c2cf-40b0-9575-93b986d92c8b" />

**Thừa kế quyền** trong IAM xảy ra khi quyền được gán cho **group**, và tất cả các **users** thuộc group đó sẽ tự động thừa kế các quyền đã được định nghĩa.

**Ví dụ**:  
Nếu **group Dev** được gán một bộ quyền nhất định, thì tất cả các thành viên trong **group Dev** đều phải tuân thủ và có các quyền giống nhau theo bộ quyền đó.


## **Cấu trúc IAM Policy (AWS)**

### **1. Thành phần chính**

- **Version**:  
  - Phiên bản ngôn ngữ của policy.  
  - Luôn sử dụng giá trị **"2012-10-17"** (bắt buộc).

- **Id**:  
  - Định danh cho policy (tùy chọn).

- **Statement**:  
  - Bao gồm một hoặc nhiều câu lệnh riêng lẻ, mô tả quyền hạn của policy (bắt buộc).


### **2. Thành phần trong Statement**

- **Sid**:  
  - Định danh cho câu lệnh (tùy chọn).

- **Effect**:  
  - Quy định hành động có **cho phép** (Allow) hay **từ chối** (Deny) truy cập vào tài nguyên.

- **Principal**:  
  - Tài khoản, người dùng, hoặc vai trò mà policy áp dụng. Có thể là **account**, **group**, hoặc **user**.

- **Action**:  
  - Danh sách các hành động được phép hoặc bị từ chối bởi policy. Ví dụ: **update**, **download**, trên một resource nào đó.

- **Resource**:  
  - Danh sách các tài nguyên mà hành động sẽ áp dụng.

- **Condition**:  
  - Điều kiện để policy có hiệu lực (tùy chọn).


## **MFA Devices Options in AWS**

<img width="1109" alt="image" src="https://github.com/user-attachments/assets/12ab7b85-3729-4cd0-b7b3-4a91e7ab8261" />

<img width="1135" alt="image" src="https://github.com/user-attachments/assets/8a714799-4f60-4592-8553-39b25c4368f8" />

Để **đảm bảo xác thực an toàn hơn** cho người dùng, AWS hỗ trợ sử dụng **MFA (Multi-Factor Authentication)**. Ví dụ, bạn có thể sử dụng ứng dụng như **Google Authenticator** để tạo mã xác thực.

- **Root account**:  
  Nên **bật MFA** cho **root account** để bảo vệ tài khoản chính của AWS khỏi các truy cập trái phép.


## **Cách người dùng truy cập AWS**

Có 3 **tùy chọn** để truy cập AWS:

1. **AWS Management Console**:  
   - Được bảo vệ bằng **mật khẩu** và **MFA** (Multi-Factor Authentication).

2. **AWS Command Line Interface (CLI)**:  
   - Cung cấp quyền truy cập qua dòng lệnh.  
   - Người dùng phải cung cấp **Access Keys** và **Secret Keys**.  
   - [Link tài liệu](https://aws.amazon.com/documentation/cli/)

3. **AWS Software Developer Kit (SDK)**:  
   - Dùng thư viện SDK của ngôn ngữ lập trình.  
   - Cũng yêu cầu cung cấp **Access Keys** và **Secret Keys**.

## **Thông tin về Access Keys**

- **Access Keys** được tạo thông qua **AWS Console**.
- Người dùng phải **tự quản lý Access Keys** của mình.
- **Access Keys** là **bí mật**, giống như mật khẩu, không được chia sẻ.
- **Access Key ID** tương tự như **username**.
- **Secret Access Key** tương tự như **password**.
- **Access Keys** và **Secret Access Keys** đại diện cho một **user**, vì vậy các **role** cũng giống nhau.


## **AWS SDK là gì?**

<img width="297" alt="image" src="https://github.com/user-attachments/assets/d30b4e6a-ddf3-40c5-8db3-94e457955f6e" />

**AWS Software Development Kit (AWS SDK)** là bộ **API** dành riêng cho từng ngôn ngữ lập trình, bao gồm các thư viện hỗ trợ việc truy cập và quản lý các dịch vụ AWS thông qua lập trình.

- **Tính năng**:
  - Cho phép bạn tích hợp các **AWS services** trực tiếp vào ứng dụng của mình.
  - Cung cấp các công cụ giúp phát triển ứng dụng sử dụng AWS hiệu quả hơn.

### **1. Các SDK được hỗ trợ**:

- **SDK cho các ngôn ngữ lập trình**:
  - JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++.

- **Mobile SDK**:
  - Android, iOS, ...

- **IoT Device SDK**:
  - Embedded C, Arduino, ...

### **2. Lưu ý**:
- Cơ chế bảo mật của AWS SDK vẫn sử dụng **Access Keys** và **Secret Access Keys** để xác thực và bảo vệ các thao tác.


## **IAM Roles cho các dịch vụ AWS**

Trong môi trường **AWS**, các dịch vụ không sử dụng **Access Keys**, **Secret Access Keys**, hay **user/password** trực tiếp. Thay vào đó, **IAM Roles** được sử dụng để quản lý quyền truy cập.

### **1. Các loại IAM Roles phổ biến**:

- **EC2 Instance Roles**:  
  Gán quyền cho các **EC2 instances** để truy cập các tài nguyên AWS.

- **Lambda Function Roles**:  
  Gán quyền cho các **Lambda functions** để tương tác với các dịch vụ AWS khác.

- **Roles for CloudFormation**:  
  Gán quyền cho **AWS CloudFormation** để triển khai và quản lý tài nguyên AWS.

### **2. Ví dụ**:

- Nếu **ứng dụng chạy trên EC2** cần tương tác với **S3** hoặc **DynamoDB**, bạn cần tạo một **IAM role** với quyền truy cập vào các tài nguyên này, và gán nó cho EC2 instance.

## **Hướng dẫn và thực hành tốt nhất cho IAM**

1. **Không sử dụng tài khoản root**, ngoại trừ khi thiết lập tài khoản AWS ban đầu.
   
2. **Một người dùng thực tế** tương ứng với một **AWS user**.

3. **Gán người dùng vào các nhóm**, sau đó cấp quyền cho các nhóm thay vì cấp quyền cho từng cá nhân.

4. **Thiết lập chính sách mật khẩu mạnh** để bảo vệ tài khoản người dùng.

5. **Sử dụng và bắt buộc dùng Multi-Factor Authentication (MFA)** cho tài khoản AWS.

6. **Tạo và sử dụng IAM Roles** để cấp quyền cho các dịch vụ AWS.

7. **Sử dụng Access Keys** cho truy cập lập trình thông qua **CLI** hoặc **SDK**.

8. **Kiểm tra và theo dõi quyền truy cập** bằng **IAM Credentials Report** và **IAM Access Advisor** (tải báo cáo về và kiểm tra).

9. **Không bao giờ chia sẻ IAM users** hoặc **Access Keys** với người khác.


## **Tóm tắt về IAM**

1. **Users**:  
   - Gắn với một người dùng thực tế, có mật khẩu để đăng nhập vào **AWS Console**.

2. **Groups**:  
   - Chỉ chứa **users**, giúp **quản lý quyền** dễ dàng hơn.

3. **Policies**:  
   - Tài liệu **JSON** xác định các quyền cho **users** hoặc **groups**.

4. **Roles**:  
   - Dùng để cấp quyền cho **EC2 instances** hoặc các dịch vụ AWS.

5. **Bảo mật**:  
   - **Sử dụng MFA** và thiết lập chính sách **mật khẩu mạnh**.

6. **AWS CLI**:  
   - Quản lý các dịch vụ AWS thông qua **dòng lệnh (command-line)**.

7. **AWS SDK**:  
   - Quản lý các dịch vụ AWS bằng **ngôn ngữ lập trình**.

8. **Access Keys**:  
   - Dùng để **truy cập AWS** thông qua **CLI** hoặc **SDK**.

9. **Kiểm tra**:  
   - Sử dụng **IAM Credential Reports** và **IAM Access Advisor** để **kiểm tra và theo dõi quyền truy cập**.

# Amazon EC2 – Basics

## **Amazon EC2**

**EC2** là một trong những dịch vụ phổ biến nhất của **AWS**.  
**EC2** = **Elastic Compute Cloud** = **Hạ tầng dưới dạng dịch vụ** (Infrastructure as a Service).

### **Chức năng chính của EC2**:
1. **Thuê máy ảo** (**EC2 Instances**).
2. **Lưu trữ dữ liệu trên ổ đĩa ảo** (**EBS – Elastic Block Store**) => bộ nhớ.
3. **Phân phối tải giữa các máy ảo** (**ELB – Elastic Load Balancer**) => cân bằng tải.
4. **Tự động mở rộng hoặc thu nhỏ dịch vụ** bằng **Auto Scaling Group (ASG)** => mở rộng hay thu hẹp **EC2** phụ thuộc vào ứng dụng của bạn.


## **Tùy chọn kích thước và cấu hình EC2**

1. **Hệ điều hành (Operating System - OS)**:  
   - **Linux**, **Windows** hoặc **Mac OS**.

2. **Sức mạnh tính toán & Lõi CPU**:  
   - Chọn **số lượng lõi** và **công suất xử lý** phù hợp với nhu cầu.

3. **Bộ nhớ truy cập ngẫu nhiên (RAM)**:  
   - Lựa chọn **dung lượng RAM** cần thiết cho ứng dụng của bạn.

4. **Dung lượng lưu trữ**:  
   - Chọn loại bộ nhớ rời:
     - **EBS** (Elastic Block Store) hay **EFS** (Elastic File System).
   - **Bộ nhớ cứng**: **EC2 Instance Store**.

5. **Card mạng**:  
   - Tốc độ của **card mạng** và **địa chỉ IP công khai**.

6. **Quy tắc tường lửa**:  
   - Thiết lập các **Security Group** để kiểm soát lưu lượng mạng vào và ra từ **EC2**.

7. **Bootstrap script**:  
   - Đoạn lệnh chạy khi **EC2** khởi động lần đầu tiên.
   - **Ví dụ**:

     <img width="630" alt="image" src="https://github.com/user-attachments/assets/ae0f0931-0b40-402f-8b1e-fea3e9caabe0" />

     Nó chỉ chạy duy nhất **1 lần**, thực thi dưới quyền **root user** trên máy ảo, không phải **root account**.

## **Quiz 1 & Take Note:**

### **Câu 1:**  
**Bạn đang thiết kế một trò chơi di động sử dụng CloudFront, Lambda, DynamoDB và CloudWatch. Cách an toàn NHẤT để cung cấp quyền cho Lambda truy cập vào bảng DynamoDB là gì?**  
- **Đáp án:** Tạo một **IAM role** cho **Lambda function** với quyền truy cập **DynamoDB**.  
- **Lưu ý:**  
  Trên **AWS Cloud**, các dịch vụ vẫn có thể sử dụng **Access Key** và **Secret Key**, nhưng chỉ nên sử dụng trong trường hợp bất khả kháng như **on-premise lên cloud** hoặc khi truy cập từ bên ngoài vào AWS.

### **Câu 2:**  
**Giải pháp tốt nhất để cấp quyền cho ứng dụng chạy trên EC2 thực hiện các truy vấn trên bảng DynamoDB là gì?**  
- **Đáp án:** Tạo một **IAM role** với quyền truy cập **DynamoDB** cần thiết và gán **role** đó cho **EC2 instance**.

- **Lưu ý:**  
  Có một số dịch vụ không thể gắn được **role**. Trong những trường hợp này, dịch vụ **B** có thể sẽ phải sử dụng các **policies** riêng của nó, thay vì phải gắn trực tiếp **role** từ **A**.

## **EC2 Instances Trả Phí**

1. **On-Demand Instances**:
   - **Tiêu chuẩn** của AWS, dùng bao nhiêu trả bao nhiêu, tính theo **giờ** hoặc **giây**, bật lên là tính 1 giờ hoặc 1 giây.
   - Dùng cho **công việc ngắn hạn** và **chạy ổn định**.
   - **Ví dụ**: Môi trường **dev** để chạy test trong 1 tuần hoặc những công việc chỉ chạy trong giờ hành chính.
   - Sau khi tạo **EC2 instance**, nó sẽ là **On-Demand**.

2. **Reserved Instances (1 & 3 năm)**:
   - **Mức chiết khấu** lên tới **72%** so với On-Demand.
   - Mua **đứt** trong **1 hoặc 3 năm**.
   - Có thể **trả trước**, **trả sau**, hoặc **trả trước một phần** (với **discount** khác nhau).
   - Phù hợp cho các hệ thống **24/7 ổn định** không cần thay thế sang instance lớn hơn.
   - **Ví dụ**: Hệ thống có số lượng khách hàng tăng trưởng theo từng năm, không nên mua loại này.
   - Giữa chừng nếu không cần, có thể **bán** trên **chợ AWS**.

3. **Savings Plans (1 & 3 năm)**:
   - **Mức chiết khấu** lên tới **72%** giống **Reserved Instances**.
   - **Cam kết sử dụng** từ **1 đến 3 năm**.
   - Khá **linh hoạt**: Thay vì mua đứt, bạn có thể **bao trọn một dòng instance** (ví dụ: instance family **M5** in **us-east-1**).
   - Cam kết trả một lượng tiền **nhất định theo giờ**. Nếu sử dụng vượt mức cam kết, sẽ tính theo giá **On-Demand**.
   - **Nhược điểm**: Nếu không dùng nữa, sẽ không bán được và vẫn phải trả tiền.

4. **Spot Instances**:
   - Hình thức cho phép **thuê resource dư thừa** của AWS, giá rất rẻ, có thể thấp hơn **90%** so với On-Demand.
   - Hình thức **đấu giá AWS**: Giá thay đổi liên tục mỗi **5 phút**.
   - **Bất ổn**, không phù hợp với các công việc **quan trọng**.

5. **Dedicated Hosts**:
   - **Thuê nguyên giàn vật lý AWS**, có thể **customize** phần cứng, **lõi**.
   - Phù hợp cho các công ty sử dụng **VMware** và **Oracle** với **licenses** (ràng buộc theo phần cứng).
   - Có 2 loại **trả tiền**: 
     - **On-Demand** (dùng bao nhiêu trả bấy nhiêu).
     - **Reserved Instances** (bao trọn từ 1 đến 3 năm).
   - **Lưu ý**: Câu hỏi về **EC2** và **licenses**, chọn **Dedicated Hosts**.

6. **Dedicated Instances**:
   - **Chỉ bao trọn phần cứng nhỏ**, không bao trọn của cả giàn.
   - Cho phép EC2 chạy **cô lập** trên phần cứng, không chia sẻ với AWS account khác.
   - Dùng trong các công ty yêu cầu bảo mật cao như ngân hàng.
   - **Rẻ hơn Dedicated Hosts**, nhưng đắt hơn **On-Demand**.

7. **Capacity Reservations**:
   - Bao trọn một **instance On-Demand** trong thời gian ngắn.
   - AWS giữ **resource** cho bạn, đảm bảo khi khởi động lại sẽ có **resource**.
   - **Tính phí full** dù bạn có sử dụng hay không.


## **Spot Fleets**

- **Cơ chế**: Tự động **request** spot instance gần nhất với các yêu cầu bạn đưa ra (như **giá tiền**, **expect**).
- Trong AWS, có phần **Spot Request** để bạn tìm và yêu cầu các **spot instances** phù hợp với nhu cầu của mình.

Nếu bạn cần tìm hiểu thêm về cách sử dụng **Spot Fleets** trong AWS, có thể tham khảo phần **Spot Request** trong AWS Console.

# Amazon EC2 – Associate

## **Private vs Public IP (IPv4)**

<img width="1144" alt="image" src="https://github.com/user-attachments/assets/b9cca34f-ac79-431a-9e80-2cb05f900547" />

- **Private IP**: Dùng cho **giao tiếp nội bộ** giữa các máy chủ trong cùng một **VPC**. 
  - Ví dụ: EC2 kết nối đến **database** của AWS sẽ sử dụng **private IP**.
  
- **Public IP**: Dùng khi **kết nối từ AWS ra ngoài** hoặc khi bạn cần truy cập từ internet.
  - Ví dụ: Kết nối từ một EC2 instance ra ngoài internet sẽ dùng **public IP**.

**Lưu ý**:
- **Private IP** sẽ **cố định**, không thay đổi khi **restart** các dịch vụ (ví dụ: EC2).
- **Public IP** sẽ **thay đổi** mỗi khi **start** lại instance. Nếu muốn **giữ cố định** một **public IP**, bạn phải sử dụng **Elastic IP**.


## **Elastic IPs**

- **Elastic IP** là một địa chỉ **IPv4 public** cố định mà bạn sở hữu, miễn là bạn không xóa nó.
- Khi bạn dừng và khởi động lại một **EC2 instance**, địa chỉ **IP public** của nó có thể thay đổi. Để có một địa chỉ IP public cố định, bạn cần **Elastic IP**.
- **Elastic IP** chỉ có thể **gắn vào một instance** tại một thời điểm.
- **Mất phí** theo giờ sử dụng.

**Lưu ý**:
- **Elastic IP** dùng rất hạn chế, vì hầu hết các ứng dụng web sử dụng **DNS** (domain) để truy cập vào server, thay vì sử dụng trực tiếp **IP**.
- **Thực tế**, việc sử dụng **Elastic IP** ít phổ biến, do các ứng dụng thường ưu tiên sử dụng **DNS**.

## **Placement Groups**

Placement Groups là cơ chế giúp kiểm soát cách mà các EC2 instances được đặt, với các loại cấu hình khác nhau. Tuy nhiên, trên thực tế, thường không chỉ định loại mà để AWS tự tạo ngẫu nhiên. Dưới đây là ba loại chính:

1. **Cluster**:

   <img width="936" alt="image" src="https://github.com/user-attachments/assets/0090ca63-a1f3-4b9b-afa7-63aaa640dd07" />

   - Các EC2 instances được đặt **cùng trong một Availability Zone (AZ)** và **cùng một giàn vật lý**.
   - **Ưu điểm**: Băng thông mạng cao, lý tưởng cho các ứng dụng đòi hỏi tốc độ truyền tải lớn như **Big Data**.
   - **Nhược điểm**: Nếu phần cứng bị lỗi, toàn bộ EC2 instances trong cluster sẽ bị ảnh hưởng.

3. **Spread**:

   <img width="660" alt="image" src="https://github.com/user-attachments/assets/aa86f553-100f-458e-8f48-c2400f1fd0cd" />

   - EC2 instances được đặt **trên nhiều giàn khác nhau và nhiều AZ khác nhau**.
   - **Ưu điểm**: Tăng tính **khả dụng** của hệ thống, tránh được việc mất toàn bộ EC2 khi một phần cứng gặp sự cố.
   - **Nhược điểm**: **Băng thông mạng** không cao như cluster.

5. **Partition**:

   <img width="566" alt="image" src="https://github.com/user-attachments/assets/98068622-cff5-404a-abb9-7970e935c1a6" />

   - EC2 instances được nhóm thành các **cụm** (partitions), mỗi cụm nằm trên **phần cứng khác nhau** và **nằm ở các AZ khác nhau**.
   - Tính năng này giúp phân tán các instances trên các phần cứng và AZ khác nhau để nâng cao độ khả dụng.

**Lưu ý**: Placement Groups chủ yếu phù hợp để ôn lý thuyết cho các kỳ thi, vì trong thực tế, AWS thường tự động phân phối EC2 instances theo cách tối ưu mà không cần chỉ định cụ thể loại.

## **Elastic Network Interfaces (ENI)**

<img width="443" alt="image" src="https://github.com/user-attachments/assets/cd59d8da-59c0-4120-8d6f-fbb3ed12cae2" />

Elastic Network Interfaces (ENI) là một thành phần quan trọng để EC2 instances có thể kết nối với mạng. Mỗi ENI cung cấp các thông tin mạng như địa chỉ IP, băng thông và các thiết lập khác.

- **Private IP**: Là địa chỉ IP mặc định được gán cho ENI và thường dùng cho giao tiếp nội bộ trong mạng AWS.
- **Public IP**: Là địa chỉ IP công khai (tùy chọn) được gán cho ENI, dùng để giao tiếp với thế giới bên ngoài.
- **Đa ENI**: Một EC2 instance có thể gắn **nhiều ENI**, mỗi ENI có thể có nhiều **private IP** và **public IP**.

ENI giúp cung cấp khả năng cấu hình linh hoạt về mạng, chẳng hạn như các IP phụ hoặc có thể cấu hình các mạng con khác nhau cho các EC2 instance.

## **EC2 Hibernate**

EC2 Hibernate là chế độ giúp EC2 instance "ngủ" và giữ lại trạng thái của bộ nhớ (RAM) thay vì tắt hoàn toàn. Khi bạn khởi động lại instance, hệ thống sẽ khôi phục trạng thái trước đó, giúp tiết kiệm thời gian khởi động lại và giữ lại tiến trình làm việc.

- **Cách kích hoạt**: Chế độ Hibernate phải được **enable từ lúc tạo EC2**. Sau khi instance đã được tạo, không thể bật hoặc tắt tính năng này.
- **Ưu điểm**: Tiết kiệm thời gian khởi động lại, giữ lại trạng thái của bộ nhớ.
- **Hạn chế**: Chế độ này chỉ hoạt động với các loại instance cụ thể và yêu cầu EBS (Elastic Block Store).

# Amazon EC2 – Lưu trữ Instance

## **EBS (Elastic Block Store)**

<img width="1069" alt="image" src="https://github.com/user-attachments/assets/5be941af-f893-43ff-ab3c-751b4e62f9e5" />

EBS là ổ cứng đính kèm với EC2, tương tự như ổ cứng trên laptop có thể tháo ra và cắm vào lại. Trên EC2, bạn cũng có thể gắn và tháo EBS theo nhu cầu.

- EBS phải ở cùng một AZ (Availability Zone) với EC2. Mặc định, khi tạo EC2, nó sẽ có một EBS kèm theo.
- Một EC2 instance có thể gắn nhiều ổ EBS.
- Mặc định, EBS sẽ bị xóa khi bạn xóa EC2 instance.

## **EBS Snapshots**

<img width="900" alt="image" src="https://github.com/user-attachments/assets/38127a4d-edee-4254-b47a-cf7c88f8b5bf" />

EBS Snapshots là phương thức sao lưu ổ cứng (EBS) của bạn và có thể phục hồi nó lên một ổ cứng khác. 

- Bạn có thể sao chép EBS Snapshots sang các AZ hoặc region khác để đảm bảo tính sẵn sàng và khả năng phục hồi dữ liệu.

## **EBS Snapshots Features**

<img width="443" alt="image" src="https://github.com/user-attachments/assets/a916516a-544b-47ea-b1ef-b4961bce9276" />

- **EBS Snapshot Archive**: Đây là tính năng lưu trữ snapshot, nhưng việc phục hồi dữ liệu có thể mất từ 1 đến 3 ngày. Vì vậy, nếu ứng dụng cần phục hồi gấp, tính năng này không phù hợp.
  
- **Fast Snapshot Restore (FSR)**: Khi phục hồi snapshot, hiệu suất có thể chậm nếu không trả phí. Tuy nhiên, nếu bạn trả phí, nó sẽ phục hồi hiệu suất về mức tối đa. Nếu không trả phí, khi mới phục hồi, snapshot sẽ cần thời gian để "nóng lên" trước khi đạt hiệu suất đầy đủ.

## **Tổng quan về AMI**

<img width="828" alt="image" src="https://github.com/user-attachments/assets/2712a495-c2f4-4130-a17e-0a67dde550c0" />

- **EBS** chỉ sao lưu một ổ cứng, trong khi **AMI** sao lưu toàn bộ EC2, bao gồm cả ổ cứng.
- Từ một AMI, bạn có thể khôi phục và tạo ra một EC2 mới.
- Đối với một ứng dụng lớn, một EC2 đơn lẻ có thể không đủ, nên từ một EC2 ban đầu, bạn cần nhân bản và chạy nhiều server.
- Hệ thống có khả năng mở rộng tự động dựa trên lưu lượng truy cập (traffic) sẽ dựa vào AMI để tạo ra các instance EC2 mới khi cần thiết.

## **EC2 Instance Store**

<img width="540" alt="image" src="https://github.com/user-attachments/assets/57c9b258-8937-4000-a95b-c27dc52d1fe4" />

- Thực tế ít sử dụng, chủ yếu xuất hiện trong các bài thi.
- Là một ổ cứng gắn với EC2 instance.
- Có hiệu suất (performance) cao hơn nhiều so với EBS.
- Khi EC2 bị dừng (stop) hoặc khởi động lại (restart), dữ liệu trên instance store sẽ mất hết, chỉ lưu trữ tạm thời.
- Chỉ hỗ trợ trên một số loại instance lớn.

## **Các loại EBS Volume**

EBS Volumes có 6 loại:

1. **gp2 / gp3 (SSD)**
   - Phổ thông nhất, ổn định và được sử dụng nhiều nhất, ví dụ: cho web, mobile, v.v.
   - Giá đắt thứ 2.

2. **io1 / io2 Block Express (SSD)**
   - Băng thông cao, dùng trong trường hợp cần đọc/ghi dữ liệu nhiều, ví dụ: database.
   - Giá đắt nhất.

3. **st1 (HDD)**
   - Dùng để lưu trữ dữ liệu hoặc truy cập ít quan trọng, có thể chấp nhận tốc độ đọc thấp.
   - Giá đắt thứ 4.

4. **sc1 (HDD)**
   - Ổ HDD có chi phí thấp nhất, dành cho các công việc ít truy cập hơn, ví dụ: backup data, backup log, v.v.
   - Giá đắt thứ 5.
  
## **EBS Multi-Attach – Dòng io1/io2**

<img width="441" alt="image" src="https://github.com/user-attachments/assets/5ed2924e-1ade-4b08-a959-792455d90c12" />

- EBS Multi-Attach cho phép một volume EBS (dòng io1/io2) được gắn (attach) cho nhiều EC2 instances cùng một lúc.
- Điều này giúp các EC2 instances có thể chia sẻ cùng một volume, rất hữu ích trong các trường hợp cần tính sẵn sàng cao hoặc khả năng chịu lỗi.

## **EBS Encryption**

- Mặc định, EBS không có mã hóa.
- Mã hóa EBS được thực hiện thông qua KMS (Key Management Service) với thuật toán AES-256.
- Chi phí cho mã hóa EBS là khá rẻ.
- Mã hóa chỉ có thể được áp dụng khi tạo EBS. Sau khi EBS đã được tạo, không thể mã hóa trực tiếp. Nếu muốn mã hóa sau khi tạo, phải thực hiện qua một trung gian snapshot.

## **Amazon EFS – Elastic File System**

<img width="611" alt="image" src="https://github.com/user-attachments/assets/1106c142-001a-48ca-a863-9b02715fd87b" />

- Hệ thống file chia sẻ giữa nhiều máy chủ.
- EBS (io1/io2) có cơ chế chia sẻ được cho nhiều EC2, nhưng trong thực tế, thường sử dụng hệ thống file như EFS vì EBS (io1/io2) có chi phí khá cao.
- Ví dụ: EFS thích hợp cho việc chia sẻ hệ thống file ảnh giữa nhiều server.
