
# Diagram: Củng cố kiến thức AWS cho Developer

### 1. **Kiến trúc tổng quan dịch vụ AWS**

```
+-------------------------------------------------------+
|                    🌐 AWS Global Cloud                |
+-------------------------------------------------------+
| Region (ex: ap-southeast-1) → Availability Zones      |
| → Data Centers → Resource Hosting (EC2, S3, etc.)     |
+-------------------------------------------------------+
```

### 2. **Compute – dịch vụ tính toán**

```
+-------------------------+
|     ⚙️ Compute Services  |
+-------------------------+
| ✔ EC2 – máy chủ ảo      |
| ✔ Lambda – serverless   |
| ✔ Elastic Beanstalk – auto deploy app |
| ✔ ECS / Fargate – container |
| ✔ EKS – Kubernetes      |
+-------------------------+
```

> **Use case:**

* EC2 → máy chủ backend
* Lambda → xử lý sự kiện nhanh (API, cron job)
* ECS → chạy Docker app


### 3. **Storage – dịch vụ lưu trữ**

```
+---------------------------+
|      💾 Storage Services   |
+---------------------------+
| ✔ S3 – object storage     |
| ✔ EBS – disk cho EC2      |
| ✔ EFS – shared file system |
| ✔ Glacier – lưu trữ lâu dài |
+---------------------------+
```

> **Use case:**

* Upload file người dùng → lưu vào **S3**
* Lưu ảnh CDN → CloudFront + S3


### 4. **Database Services**

```
+-----------------------------+
|     🗄️ Database Services     |
+-----------------------------+
| ✔ RDS – SQL (MySQL, Postgres...) |
| ✔ DynamoDB – NoSQL          |
| ✔ Aurora – DB hiệu suất cao |
| ✔ ElastiCache – cache Redis |
+-----------------------------+
```

> RDS dùng khi cần SQL chuẩn
> DynamoDB dùng khi cần hiệu suất cao, scale dễ

### 5. **Networking & Security**

```
+-------------------------------+
|   🌐 Networking & Security     |
+-------------------------------+
| ✔ VPC – mạng ảo riêng          |
| ✔ Security Groups – firewall  |
| ✔ IAM – phân quyền            |
| ✔ Route 53 – DNS & domain     |
| ✔ API Gateway – RESTful API   |
+-------------------------------+
```

> IAM dùng để gán quyền: user, group, role
> API Gateway + Lambda = serverless REST API


### 6. **DevOps & CI/CD**

```
+-------------------------------+
|       ⚙️ DevOps Tools          |
+-------------------------------+
| ✔ CodeCommit – Git repo       |
| ✔ CodePipeline – CI/CD pipeline |
| ✔ CodeBuild – build app       |
| ✔ CloudFormation – IaC        |
| ✔ Terraform – IaC phổ biến hơn |
+-------------------------------+
```

### 7. **Monitoring & Logging**

```
+----------------------------+
|    📊 Monitoring Tools     |
+----------------------------+
| ✔ CloudWatch – log, alert |
| ✔ X-Ray – trace request    |
| ✔ CloudTrail – audit log   |
+----------------------------+
```

> CloudWatch: Xem log Lambda, EC2, các metric
> X-Ray: Trace từng request qua service


### 8. **Triển khai Frontend App (SPA)**

```
+-----------------------------+
|   🌐 Triển khai frontend    |
+-----------------------------+
| ✔ S3 – chứa file build      |
| ✔ CloudFront – CDN          |
| ✔ Route53 – gắn domain      |
| ✔ Certificate Manager – SSL |
+-----------------------------+
```

> `React / Angular / Vue` build → upload lên S3 → dùng CloudFront phân phối toàn cầu


### 9. **Triển khai Backend App**

```
+-----------------------------------+
|         🧩 Backend Deployment      |
+-----------------------------------+
| Option 1: EC2 + Nginx             |
| Option 2: API Gateway + Lambda    |
| Option 3: ECS (Fargate) + Docker |
| Option 4: Elastic Beanstalk       |
+-----------------------------------+
```


### 10. **Best Practices**

```
+---------------------------------------------+
|            ✅ Best Practices AWS             |
+---------------------------------------------+
| ✔ Sử dụng IAM Role đúng scope               |
| ✔ Không hardcode secret, dùng Secrets Manager|
| ✔ Dùng VPC riêng cho các dịch vụ quan trọng |
| ✔ Dùng Multi-AZ cho DB                      |
| ✔ Logging + Monitor qua CloudWatch          |
| ✔ Tự động hóa bằng IaC (Terraform / CF)     |
+---------------------------------------------+
```

# Lý thuyết

<img width="1131" alt="image" src="https://github.com/user-attachments/assets/97fcd3f8-fee1-463c-9c54-7bed36fd5afc" /># **Bắt đầu với AWS**
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

# **High Availability & Scalability**

## **Scalability & High Availability**

Cho dù ứng dụng của chúng ta chạy trên cloud hay on-premise, chúng ta vẫn phải quan tâm đến 2 yếu tố này:

### **Scalability**  
Ứng dụng của mình phải đáp ứng được lượng tải của server khi cao lúc thấp. Nó cần phải mở rộng hoặc thu hẹp, tăng số lượng server hoặc giảm số lượng server để đáp ứng traffic.

Có 2 loại **Scalability**:  
1. **Scalability theo chiều dọc (Vertical Scaling)**  
2. **Scalability theo chiều ngang (Horizontal Scaling)**

## **Khả năng Mở Rộng Theo Chiều Dọc (Vertical Scalability)**

![image](https://github.com/user-attachments/assets/51e899e4-af84-488f-a488-db6b56b51efc)

**Khả năng mở rộng theo chiều dọc**: Ví dụ, hiện tại mình là một junior developer, nhưng sau 2 hoặc 3 năm nữa, mình trở thành một senior developer, lúc đó mình có thể đảm nhận nhiều task hơn và làm việc với hiệu suất tốt hơn.

### **1. Ví dụ**:  
Ứng dụng của bạn chạy trên một instance t2.micro, và bây giờ bạn nâng cấp lên t2.large (điều này sẽ tăng RAM, CPU, core, ...).

### **2. Lưu ý**:  
Scalability theo chiều dọc thường được áp dụng cho **database** và **elastic cache**.

## **Khả năng Mở Rộng Theo Chiều Ngang (Horizontal Scalability)**

![image](https://github.com/user-attachments/assets/97006978-173b-4cbc-b5ec-245eacebd2fb)

### **1. Khả năng mở rộng theo chiều ngang**:  
Mình sẽ nhân rộng ra, ví dụ một operator có thể đảm nhận một task, giờ nếu có 6 operators, mình sẽ đảm nhận được 6 tasks.

### **2. Hiểu đơn giản**:  
Là việc tăng hoặc giảm số lượng **EC2 instances**.

### **3. Ứng dụng**:  
Điều này rất phổ biến trong **web applications**.


## **High Availability: Tính Sẵn Sàng**

![image](https://github.com/user-attachments/assets/a8a4ece3-f4d7-4ac0-a640-1c558a0dec86)

**Tính sẵn sàng cao**:  
Hệ thống của mình, cho dù gặp vấn đề nào đó, vẫn phải đáp ứng được traffic truy cập của người dùng. Đây gọi là tính sẵn sàng cao.

**Ví dụ**:  
Có văn phòng ở New York và San Francisco. Nếu văn phòng ở New York nghỉ, cúp điện, hoặc vì lý do nào đó không làm việc, văn phòng ở San Francisco vẫn có thể tiếp tục hoạt động và phục vụ khách hàng, luôn đảm bảo tính sẵn sàng.

## **Độ Sẵn Sàng Cao và Khả Năng Mở Rộng Cho EC2**

### **1. Scale Theo Chiều Dọc**  
- **Tăng size instance (scale up/down)**  
  Từ: t2.nano – 0.5G RAM, 1 vCPU  
  Đến: u-12tb1.metal – 12.3 TB RAM, 448 vCPUs  

### **2. Scale Theo Chiều Ngang**  
- **Tăng số lượng instances (scale out/in)**  
- **Auto Scaling Group**: Dựa vào các điều kiện để kích hoạt scale out hoặc scale in  
- **Load Balancer**: Cân bằng tải cho các server ở phía dưới (ví dụ, 2 servers dùng để scale theo chiều ngang)

### **3. Độ Sẵn Sàng Cao**  
- Chạy ứng dụng của mình trên một server duy nhất có thể gây rủi ro nếu server đó gặp sự cố. Tuy nhiên, để tăng tính sẵn sàng, ứng dụng sẽ chạy trên ít nhất 2 server, mỗi server ở một Availability Zone (AZ).  
- **Mô hình**:  
  - **Auto Scaling Group**: Chia ra cho nhiều AZ  
  - **Load Balancer**: Cân bằng tải trên nhiều AZ
 
## **What is Load Balancing?**

![image](https://github.com/user-attachments/assets/0ffe7d50-6989-410d-86dc-fd79b95496b8)

**Cân Bằng Tải**:  
Đơn giản, khi người dùng gửi yêu cầu, load balancer sẽ nhận và phân phối tải cho các server bên dưới. Mục tiêu là không để tất cả yêu cầu chạy trên một server duy nhất mà chia đều cho nhiều server, đảm bảo hiệu suất và tính ổn định.


## **Tại Sao Sử Dụng Load Balancer?**

- **Chia tải cho nhiều downstream instance**:  
  Những cái gì sau load balancer gọi là downstream instance (như EC2), còn những gì trước load balancer gọi là upstream instance.

- **Health Check**:  
  Load balancer có cơ chế kiểm tra sức khỏe của các instance. Nếu instance nào báo unhealthy, nó sẽ bị loại bỏ khỏi cân bằng tải, đảm bảo người dùng không gửi yêu cầu đến một server không thể phục vụ.

- **Cung cấp SSL termination (HTTPS)**:  
  Cho phép người dùng sử dụng HTTPS để truy cập đến load balancer, bảo mật giao tiếp.

- **Áp dụng cơ chế stickiness với cookies**:  
  Giúp duy trì phiên làm việc của người dùng với một server nhất định, tạo sự ổn định trong quá trình tương tác.

- **Đảm bảo độ sẵn sàng cao trên các khu vực**:  
  Đặt EC2 instances trên nhiều Availability Zones (AZ) thay vì chỉ một AZ, giúp đảm bảo tính sẵn sàng.

- **Tách biệt public traffic và private traffic**:  
  - **Public traffic**: Những yêu cầu từ người dùng đến load balancer thông qua môi trường internet.  
  - **Private traffic**: Những yêu cầu từ EC2 đến load balancer trong mạng nội bộ (private network).
 
## **Tại Sao Sử Dụng Elastic Load Balancer?**

- **Không cần setup server riêng**:  
  Nếu không dùng dịch vụ **Elastic Load Balancer** của AWS, bạn sẽ phải tự setup một server và cài đặt một web server như **Apache**, **Nginx**, hoặc **IIS**. Việc này yêu cầu bạn phải vận hành EC2 và cấu hình, điều này có thể không đảm bảo tính **High Availability (HA)**.

- **Cung cấp cấu hình đơn giản**:  
  Elastic Load Balancer đơn giản hóa việc cấu hình và vận hành cân bằng tải, giúp tiết kiệm thời gian và công sức.

- **Các dịch vụ đi kèm với Load Balancer**:
  - **Cân bằng tải trên EC2**: Kết hợp với **EC2 Auto Scaling Groups** để scale in/out.
  - **ECS (Elastic Container Service)**: Dùng để cân bằng tải cho các container Docker trong ECS.
  - **AWS Certificate Manager (ACM)**: Cung cấp dịch vụ tạo certificate cho phép người dùng truy cập qua HTTPS.
  - **CloudWatch**: Ghi lại log của load balancer.
  - **Route 53**: Dịch vụ định tuyến.
  - **AWS WAF (Web Application Firewall)**: Tường lửa bảo vệ ứng dụng web.
  - **AWS Global Accelerator**: Tăng tốc hiệu suất ứng dụng toàn cầu.

- **Lưu ý**:  
  Các web server như **Apache**, **Nginx**, **IIS** là những hệ thống cân bằng tải thủ công, bạn phải tự cấu hình cho chúng, trong khi **Elastic Load Balancer** tự động quản lý và cấu hình.


## **Kiểm Tra Sức Khỏe (Health Checks)**

![image](https://github.com/user-attachments/assets/410252fc-3140-4718-9c7e-31251608f5a5)

- **Health Check** sẽ luôn được thực hiện sau một khoảng thời gian nhất định (tính bằng giây).

## **Các Loại Load Balancer trên AWS**

![image](https://github.com/user-attachments/assets/5bc9ca1c-5dd4-47c1-a7e6-6fe79b92b5bc)

Có 4 loại **Load Balancers**:

1. **Classic Load Balancer (v1 - thế hệ cũ, không dùng nữa) – 2009 – CLB**  
   Hỗ trợ: HTTP, HTTPS, TCP, SSL (TCP bảo mật).

2. **Application Load Balancer (v2 - thế hệ mới) – 2016 – ALB**  
   Hoạt động ở **Layer 7**: HTTP, HTTPS, WebSocket.

3. **Network Load Balancer (v2 - thế hệ mới) – 2017 – NLB**  
   Hoạt động ở **Layer 4**: TCP, TLS (TCP bảo mật), UDP.

4. **Gateway Load Balancer – 2020 – GWLB (rất ít khi dùng)**  
   Hoạt động ở **Layer 3 (Lớp Mạng)** – Giao thức IP.

### **1. Lưu Ý**:
- **7 Layer** là cách hai máy tính giao tiếp với nhau trong mạng nội bộ (tìm hiểu thêm về kiến thức mạng để rõ hơn).
- Trong đề thi, các ứng dụng hoạt động ở tầng **TCP và UDP** thường là các ứng dụng **game** và **mobile game**. **TCP** là cơ chế truyền tin tin cậy, sẽ gửi lại nếu gói tin bị mất, trong khi **UDP** không gửi lại gói tin (ví dụ: live stream).

### **2. Tổng Quan**:  
Nên sử dụng các **load balancer thế hệ mới** vì chúng cung cấp nhiều tính năng hơn.

Một số load balancers có thể được thiết lập là **internal (riêng tư)** hoặc **external (công khai)** ELBs.

## **Load Balancer Security Groups**

![image](https://github.com/user-attachments/assets/f5cf587a-f36d-454d-9d44-6280b484c9ad)

- **Security Group của Load Balancer**:  
  Cho phép **port 80 (HTTP)** hoặc **port 443 (HTTPS)** từ internet.

- **Security Group của EC2**:  
  Chỉ cho phép **Security Group của Load Balancer** truy cập vào **port 80**. Điều này có nghĩa là bạn chỉ có thể truy cập đến EC2 thông qua Load Balancer.

## **Application Load Balancer Target Groups (Nhóm Services)**

![image](https://github.com/user-attachments/assets/d0745d8c-a1cc-4336-9da6-18db03e8a68c)

- **Một nhóm EC2 instances**.
- **Một nhóm container**.
- **Lambda functions** – Yêu cầu HTTP được chuyển đổi thành một sự kiện JSON.
- **IP address** – Phải là địa chỉ **private IP** (có thể là một server trong data center hoặc EC2, tóm lại chỉ cần private IP là được).
- **ALB có thể định tuyến tới nhiều target groups**.
- **Target group có cơ chế health check**.

## **Application Load Balancer - Good to Know**  

![image](https://github.com/user-attachments/assets/d94c0d90-5721-4386-aa8a-6e6f271321df)

**Biết thì tốt hơn**:

- Khi **client request** đến **load balancer** và load balancer **routing traffic** đến **EC2**, khi đọc log tại EC2, bạn sẽ không thấy địa chỉ IP của client, mà chỉ thấy **private IP của load balancer**.  
- Tuy nhiên, nếu bạn muốn lấy **client IP**, bạn cần phải lấy từ **X-Forwarded-For** (tự tìm hiểu thêm).

## **Network Load Balancer (v2)**

- **Hoạt động ở Layer 4**: Tầng **TCP và UDP** (phù hợp với ứng dụng game).
- **Độ trễ**: Khoảng **100ms**, gấp 4 lần ALB (40ms).
- **Sử dụng** cho các ứng dụng cần **performance cực kỳ nhanh**.
- **Không có trong Free Tier**: Demo sẽ mất phí.

## **Network Load Balancer – Target Groups**

- **Rules**: Dùng để **route** tới các **target groups** khác nhau.

### **Các loại Target Groups**:
- **EC2 instances**.
- **IP Addresses** – Phải là **private IPs**.
- **Application Load Balancer**.

- **Health Checks**: Hỗ trợ các giao thức **TCP**, **HTTP**, và **HTTPS**.



## **Gateway Load Balancer**

![image](https://github.com/user-attachments/assets/f75421eb-2ae5-4175-9b32-5c298e8a0ab5)

- **Cơ chế bảo vệ application**:  
  Bằng cách sử dụng một **target group** được vận hành bởi bên thứ 3, giúp phát hiện và ngăn chặn các bất thường của hệ thống (ví dụ: traffic mang tính tấn công).
  
- **Không được sử dụng nhiều**.

## **Gateway Load Balancer – Target Groups**

![image](https://github.com/user-attachments/assets/09b7d2bd-026a-4c4e-b0d7-e7bf369b0c2c)

### **Các loại Target Groups**:
- **EC2 instances**.
- **IP Addresses** – Phải là **private IPs**.

## **Sticky Sessions (Session Affinity)**

![image](https://github.com/user-attachments/assets/f223a03f-f3c7-435f-b1a1-2a622e00dbda)

- **Khái niệm**:  
  Khi người dùng request, đôi khi sẽ được route tới **server này**, đôi khi lại là **server khác**. Tuy nhiên, khi người dùng **login**, session cần được lưu trữ ở một nơi để khi chuyển trang (mua hàng, thanh toán,...), hệ thống kiểm tra xem session còn tồn tại không. 
  - Vấn đề: Mỗi lần chuyển trang, bạn có thể được route tới một **server khác**, mà server này không có session của bạn, dẫn đến **logout**.
  
- **Giải pháp**:  
  Sử dụng **Sticky Sessions** để giữ người dùng luôn gắn liền với cùng một server trong suốt phiên làm việc.

- **Dịch vụ lưu trữ session data**:  
  Trong thực tế, có thể sử dụng **DynamoDB** hoặc **ElastiCache** để lưu session data, thay vì các cơ sở dữ liệu như **SQL**.  
  - **Lý do**: Session data cần được truy xuất liên tục với **hiệu suất cao** và có thời gian lưu trữ ngắn. Các cơ sở dữ liệu như SQL không đủ hiệu suất cho yêu cầu này và không có cơ chế tự xoá dữ liệu khi hết hạn.
 
## **Cross-Zone Load Balancing**

- **Đây là một thiết lập (setting)**:  
  Khi không sử dụng **Cross-Zone Load Balancing**, traffic sẽ được phân bố **không đồng đều**. Ví dụ, một server có thể chịu 25% tải, trong khi server khác chỉ chịu 6.25% tải.

![image](https://github.com/user-attachments/assets/d95d7db0-50ce-4d31-99f5-5401e3628112)

- **Khi sử dụng Cross-Zone Load Balancing**, traffic sẽ được phân bố **đồng đều** trên tất cả các server. Ví dụ, mỗi server sẽ chịu **10%** tải.

![image](https://github.com/user-attachments/assets/55fdb161-13de-4f41-aebb-e372416bbd50)


## **SSL/TLS - Basics**

- **Tạo và liên kết SSL/TLS** với **load balancer** (Network Load Balancer hoặc Application Load Balancer) để đảm bảo **security**.
  
- Bạn có thể mua chứng chỉ SSL/TLS từ **bên thứ 3** hoặc sử dụng dịch vụ **ACM (AWS Certificate Manager)** của AWS.

## **SSL – Server Name Indication (SNI)**

![image](https://github.com/user-attachments/assets/a893d8c3-f3e5-4bb8-b0b0-cb184e4b6f30)

- **Quản lý nhiều chứng chỉ SSL** trên cùng một **ALB (Application Load Balancer)**.
  
- ALB có thể routing đến nhiều **target group** khác nhau, mỗi **target group** có thể có một **domain riêng**. Mỗi domain sẽ có một **chứng chỉ SSL riêng**.

- **SNI** giúp kết hợp và quản lý **nhiều chứng chỉ SSL** cho cùng một **ALB**.

## **Connection Draining**

![image](https://github.com/user-attachments/assets/cbfdec5b-4ec0-47eb-bf6d-8438f5e47a96)

- Khi **Load Balancer** cân bằng tải cho các server, có thể xảy ra tình trạng một server bị **fail** vì lý do nào đó. Lúc này, server sẽ chuyển sang trạng thái **de-registering** hoặc **unhealthy**.
  
- **In-flight requests**: Là những yêu cầu vẫn đang chờ xử lý khi load balancer chuyển hướng đến server bị lỗi. Thời gian hoàn thành của các yêu cầu này sẽ phụ thuộc vào cấu hình của bạn (từ 0 đến 3600 giây).

- **Ví dụ**: Nếu bạn đặt **60 giây**, sau thời gian này:
  - Nếu server phục hồi, các yêu cầu sẽ tiếp tục được xử lý.
  - Nếu không, server sẽ **fail** và trả về kết quả lỗi (ví dụ: lỗi **500**).
 
## **Auto Scaling Group in AWS**

![image](https://github.com/user-attachments/assets/15005308-9f4b-4422-83da-0ce0c82a1086)

- Trong thực tế, tải trên ứng dụng sẽ thay đổi theo mức độ traffic, có thể tăng cao hoặc giảm. Để đáp ứng yêu cầu này, cần có cơ chế tự động tăng hoặc giảm số lượng **EC2 instances**. AWS cung cấp dịch vụ **Auto Scaling Group**.

- **Cấu hình**:
  - **Min, Max, và Desired**: Bạn đặt số lượng EC2 tối thiểu, tối đa và số lượng mong muốn.
  
- **Tính năng**:
  - **Scale out**: Tự động đăng ký EC2 mới vào **load balancer** khi cần thiết.
  - **Tự động tạo EC2 mới**: Trong trường hợp một EC2 bị **terminated** hoặc **unhealthy**, Auto Scaling Group sẽ tự động tạo một EC2 mới để thay thế.
  - Trong **trường hợp bình thường**, Auto Scaling Group sẽ duy trì số lượng EC2 theo mức **Desired**, không vượt quá số lượng **Max** và không ít hơn số lượng **Min**.


 ## **Auto Scaling Group in AWS With Load Balancer**

![image](https://github.com/user-attachments/assets/8e02f6b0-88ec-4819-87f3-362de60d5832)

- Khi **EC2 instances** mới được tạo ra trong **Auto Scaling Group**, chúng sẽ tự động được **thêm vào Load Balancer** để tham gia vào quá trình cân bằng tải.

## **Thuộc Tính của Auto Scaling Group**

![image](https://github.com/user-attachments/assets/a2694f82-c29f-48bd-bc70-59a3c1ddc7ab)

- **Launch Template**: Định nghĩa cấu hình khi EC2 instances được tạo ra.
  - **AMI + Loại Instance**: Cấu hình AMI và loại instance (ví dụ: 1 CPU, 2GB RAM).
  - **EC2 User Data**: Dữ liệu người dùng cấu hình khi khởi tạo EC2.
  - **EBS Volumes**: Cấu hình các ổ đĩa EBS cho EC2 instances.
  - **Security Groups**: Nhóm bảo mật cho EC2 instances.
  - **SSH Key Pair**: Cặp khóa SSH để truy cập EC2.
  - **IAM Roles**: Các vai trò IAM cần cho EC2 instances.
  - **Network + Subnets**: Thông tin về mạng và subnet của EC2.
  - **Thông tin Load Balancer**: Thông tin cấu hình load balancer mà EC2 sẽ tham gia.
=> đây là Launch Template nó bao gồm những thông tin này, khi tạo ra ec2 thì phải đáp ứng config template trước đó 

- **Scaling Policies**: Định nghĩa các điều kiện và cách thức thực hiện **scale in/out** dựa trên các yếu tố nhất định.

## **Auto Scaling - CloudWatch Alarms & Scaling**

- **CloudWatch Alarms** giám sát các **metric** như mức sử dụng trung bình của **EC2**, **RAM**, hoặc **Network In/Out** của các EC2 instances trong **Auto Scaling Group**.
- Khi một **metric** đạt hoặc vượt qua ngưỡng đã thiết lập trước, **CloudWatch Alarms** sẽ kích hoạt **scale in/out** tự động để điều chỉnh số lượng EC2 instances sao cho phù hợp với yêu cầu tài nguyên.


## **Auto Scaling Groups – Scaling Policies**

1. **Dynamic Scaling**
   - **Target Tracking Scaling**: Chỉ định một mức sử dụng cụ thể (ví dụ: 40% CPU). Hệ thống sẽ tự động tăng hoặc giảm số lượng EC2 instances để giữ mức sử dụng ở gần con số này. Đây là phương pháp phổ biến hơn so với **Simple / Step Scaling**.
   - **Simple / Step Scaling**: Đặt ngưỡng cao và thấp cho các cảnh báo:
     - Khi cảnh báo CloudWatch kích hoạt (ví dụ: CPU > 70%), thêm 2 đơn vị EC2.
     - Khi cảnh báo CloudWatch kích hoạt (ví dụ: CPU < 30%), giảm 1 đơn vị EC2.

2. **Scheduled Scaling**: Thiết lập lịch để tăng hoặc giảm số lượng EC2 vào một thời điểm cụ thể, giúp điều chỉnh tài nguyên theo kế hoạch đã định.

3. **Predictive Scaling**: Sử dụng dữ liệu lịch sử (ví dụ: vài tuần trước) để dự đoán và thực hiện **scale in/out** cho các khoảng thời gian sắp tới.

![image](https://github.com/user-attachments/assets/7b422c57-0110-4bfd-900f-6401e40c8372)

## **Các Chỉ Số Quyết Định Scale hay Không**

![image](https://github.com/user-attachments/assets/e24d6868-1282-4671-8195-d4ba8009abd6)

- **CPUUtilization**: Mức sử dụng CPU trung bình trên các instance.
- **RequestCountPerTarget**: Đảm bảo số lượng yêu cầu trên mỗi EC2 instance được duy trì ổn định.
- **Average Network In / Out**: Dùng khi ứng dụng của bạn phụ thuộc nhiều vào mạng.
- **Bất kỳ chỉ số tùy chỉnh nào**: Ví dụ:
  - Giả sử bạn muốn theo dõi số lượng đơn hàng chưa xử lý trong ứng dụng thương mại điện tử. Bạn có thể gửi số lượng đơn hàng này lên CloudWatch dưới dạng custom metric, ví dụ:
  ```bash
  aws cloudwatch put-metric-data --metric-name PendingOrders --namespace ECommerceApp --unit Count --value 150
  ```
  - Sau đó, cấu hình **CloudWatch Alarm** để mở rộng hoặc thu nhỏ **Auto Scaling Group** dựa trên số lượng đơn hàng, ví dụ:
    - Khi **PendingOrders > 200**, mở rộng quy mô.
    - Khi **PendingOrders < 50**, giảm quy mô.

## **Auto Scaling Groups - Scaling Cooldowns**

![image](https://github.com/user-attachments/assets/7cf2c3a0-3cf7-4158-8f5e-cbc846d52c98)

- **Cooldowns**: Sau khi quá trình scaling diễn ra, bạn có **300s** để thực hiện thời gian cooldowns.
- Trong thời gian cooldown, **ASG** sẽ không terminated hoặc khởi tạo thêm bất kỳ EC2 nào, giúp các metric ổn định.
- **Sử dụng AMI sẵn có**: Để giúp server phục vụ request nhanh nhất, ví dụ: khi tạo EC2 làm web server, bạn cần cài đặt nhiều thứ như Apache, Nginx, v.v. Tuy nhiên, bạn có thể cài đặt trước và đóng gói mọi thứ cần thiết trong một AMI để tiết kiệm thời gian khi triển khai.


# Amazon S3

## **Section Introduction**

S3 là một dịch vụ lưu trữ dữ liệu (storage) tương tự như Google Drive, cho phép:  
- **Tải lên file (upload files)**  
- **Tạo thư mục (create folders)**  

Ngoài ra, S3 còn có khả năng liên kết với nhiều dịch vụ khác trong hệ sinh thái AWS, giúp mở rộng khả năng sử dụng và quản lý dữ liệu hiệu quả hơn.  

## ** Trường hợp sử dụng Amazon S3 **

<img width="311" alt="image" src="https://github.com/user-attachments/assets/d2efa3b0-5243-463c-b967-d73737b80fe6" />

1. **Backup and Storage**  
   - Lưu trữ các bản sao lưu (backup) với chi phí thấp.

2. **Disaster Recovery**  
   - Hỗ trợ phục hồi dữ liệu trong các tình huống khẩn cấp.  

3. **Archive**  
   - Lưu trữ dữ liệu lâu dài với chi phí tối ưu.

4. **Hybrid Cloud Storage**  
   - Kết hợp lưu trữ giữa hệ thống on-premise và đám mây AWS.  

5. **Application Hosting**  
   - **Landing Pages**  
   - Blog hoặc portfolio cá nhân.  
   - Ứng dụng web tĩnh sử dụng API từ backend.  
   - Trang tài liệu trực tuyến.  
   - Ứng dụng React, Angular, hoặc Vue đã được build thành các file tĩnh.  

6. **Media Hosting**  
   - Lưu trữ và phân phối các tệp phương tiện như hình ảnh, video, và âm thanh.  

7. **Data Lakes & Big Data Analytics**  
   - Xây dựng kho dữ liệu lớn và hỗ trợ phân tích dữ liệu.

8. **Software Delivery**  
   - Nén mã nguồn thành file ZIP và đẩy lên S3.  
   - Các dịch vụ triển khai (deploy) sẽ tải ZIP này để triển khai ứng dụng.  

9. **Static Website Hosting**  
   - Lưu trữ và chạy website tĩnh được xây dựng bằng HTML, CSS.
  
## **Amazon S3 - Buckets**

<img width="206" alt="image" src="https://github.com/user-attachments/assets/99921c78-cdbf-4dc3-bfd1-925dbc3fee49" />

- **Bucket là gì?**  
  Bucket là một "container" dùng để chứa file và thư mục, tương tự như ổ đĩa C hoặc E trên máy tính.  

- **Đặc điểm của Bucket**:  
  1. **Tên bucket phải duy nhất**  
     - Không được trùng lặp với bất kỳ bucket nào trên toàn cầu.  
  2. **Bucket gắn với một region cụ thể**  
     - Mỗi bucket được liên kết với một khu vực (region) mà bạn chọn khi tạo.  
  3. **Quy tắc đặt tên bucket**  
     - Có các quy tắc riêng để đặt tên (ví dụ: chỉ dùng ký tự thường, số, và dấu gạch ngang).  





Dưới đây là phiên bản đã được định dạng và trình bày rõ ràng hơn:  

---

### **Amazon S3 - Objects**  

- **Object là gì?**  
  - Một object là một file được tải lên S3.  
  - Mỗi lần upload file, một object mới sẽ được tạo.  

- **Cấu trúc và Định danh của Object**:  
  1. **Key**  
     - Mỗi object có một key, đóng vai trò như đường dẫn đến file đó.  
     - Ví dụ:  
       - `s3://my-bucket/my_file.txt`  
       - `s3://my-bucket/my_folder1/another_folder/my_file.txt`  
  2. **Meta Data**  
     - Thông tin về file, chẳng hạn như kích thước, loại file, hoặc thông tin thêm từ hệ thống.  
  3. **Tags**  
     - Gắn thẻ để quản lý object dễ dàng hơn (ví dụ: xác định môi trường như `dev`, `staging`).  
  4. **Version ID**  
     - Được sử dụng để quản lý nhiều phiên bản của cùng một file (nếu bật Versioning).  

- **Kích thước tối đa của Object**:  
  - **Tối đa 5TB (5000 GB)** cho mỗi object.  
  - Nếu file lớn hơn 5GB, nên sử dụng **Multi-Part Upload** để cải thiện tốc độ và độ tin cậy:  
    - Ý tưởng: Băm nhỏ file thành nhiều phần rồi upload từng phần.  

## **Amazon S3 - Objects**  

- **Object là gì?**  
  - Một object là một file được tải lên S3.  
  - Mỗi lần upload file, một object mới sẽ được tạo.  

- **Cấu trúc và Định danh của Object**:  
  1. **Key**  
     - Mỗi object có một key, đóng vai trò như đường dẫn đến file đó.  
     - Ví dụ:  
       - `s3://my-bucket/my_file.txt`  
       - `s3://my-bucket/my_folder1/another_folder/my_file.txt`  
  2. **Meta Data**  
     - Thông tin về file, chẳng hạn như kích thước, loại file, hoặc thông tin thêm từ hệ thống.  
  3. **Tags**  
     - Gắn thẻ để quản lý object dễ dàng hơn (ví dụ: xác định môi trường như `dev`, `staging`).  
  4. **Version ID**  
     - Được sử dụng để quản lý nhiều phiên bản của cùng một file (nếu bật Versioning).  

- **Kích thước tối đa của Object**:  
  - **Tối đa 5TB (5000 GB)** cho mỗi object.  
  - Nếu file lớn hơn 5GB, nên sử dụng **Multi-Part Upload** để cải thiện tốc độ và độ tin cậy:  
    - Ý tưởng: Băm nhỏ file thành nhiều phần rồi upload từng phần.  


## **Amazon S3 – Security**

### **Hình thức phân quyền trong S3**  
1. **User-Based Policy**  
   - **Phân quyền dựa trên người dùng**:  
     - Đây là cách phổ biến nhất, sử dụng **IAM Policies**.  
     - Ví dụ: Khi một EC2 instance cần truy cập S3, bạn gắn IAM Policies vào **role** để cấp quyền truy cập S3 cho EC2.  
<img width="862" alt="image" src="https://github.com/user-attachments/assets/1eb31e5c-2bc5-413e-8ded-f402a8d7ec4c" />

<img width="862" alt="image" src="https://github.com/user-attachments/assets/a2939183-2461-457d-b09a-7a786e928d30" />

2. **Resource-Based Policy**  
   - **Phân quyền dựa trên tài nguyên**:  
     - S3 có **Bucket Policies** độc lập không phụ thuộc vào IAM Policies.  
     - Bucket Policies có thể cho phép các dịch vụ khác truy cập mà không cần IAM Policies.  

<img width="1116" alt="image" src="https://github.com/user-attachments/assets/e32688ea-1895-48c3-a6dc-2e6f33d2a4f2" />

<img width="1005" alt="image" src="https://github.com/user-attachments/assets/2c315702-6fe7-4bc9-ab57-d4edad921a41" />


### **Các cơ chế phân quyền trong S3**  
1. **Bucket Policies**  
   - Thiết lập quy tắc phân quyền trực tiếp cho toàn bộ bucket từ giao diện quản lý S3.  
   - Hỗ trợ truy cập giữa các tài khoản AWS khác nhau.  
   - **Thường được sử dụng** trong các trường hợp không thể gắn role.  

2. **Object Access Control List (ACL)**  
   - Phân quyền chi tiết ở cấp độ từng object.  
   - **Không khuyến nghị sử dụng** do đã lỗi thời.  

3. **Bucket Access Control List (ACL)**  
   - Phân quyền cho toàn bộ bucket.  
   - **Ít được sử dụng** và có thể bị vô hiệu hóa.  

### **Lưu ý khi lựa chọn giữa IAM Policies và Bucket Policies**  
- **Ưu tiên sử dụng IAM Policies khi có thể gắn role**.  
- Ví dụ:  
  - EC2 truy cập S3: Dùng IAM Policies vì EC2 có thể gắn role.  
  - ALB truy cập S3: Dùng Bucket Policies vì ALB không hỗ trợ gắn IAM Policies.  

### **Xử lý xung đột giữa IAM Policies và Bucket Policies**  
- Khi IAM Policies cho phép truy cập nhưng Bucket Policies từ chối (Deny), kết quả cuối cùng luôn là **Deny**.  

## **S3 Bucket Policies**  

### **Đặc điểm chính**  
- **S3 Bucket Policies** rất giống với IAM Policies, nhưng chúng **chỉ áp dụng cho S3**.  
- Các chính sách này được xây dựng dựa trên **JSON**.  
- **Định dạng**: Tất cả chính sách S3 đều sử dụng cấu trúc JSON.  

### **Các thành phần chính trong chính sách**  

1. **Resources (Tài nguyên)**  
   - Chỉ định các bucket và các đối tượng (object) bên trong bucket mà chính sách sẽ áp dụng.  
   - Ví dụ:  
     - Toàn bộ bucket: `"arn:aws:s3:::my-bucket"`  
     - Một file cụ thể: `"arn:aws:s3:::my-bucket/my-file.txt"`  

2. **Effect (Hiệu lực)**  
   - Xác định hành động được cho phép hoặc từ chối.  
   - Các giá trị:  
     - `"Allow"`: Cho phép.  
     - `"Deny"`: Từ chối.  

3. **Actions (Hành động)**  
   - Tập hợp các API mà chính sách sẽ cho phép hoặc từ chối.  
   - Ví dụ:  
     - `"s3:GetObject"`: Cho phép tải file.  
     - `"s3:PutObject"`: Cho phép tải lên file.  

4. **Principal (Chủ thể)**  
   - Xác định tài khoản hoặc người dùng mà chính sách áp dụng.  
   - Ví dụ:  
     - Một tài khoản AWS cụ thể.  
     - Một dịch vụ (như EC2).  
     - `"*"`: Tất cả mọi người.  

### **Ví dụ về S3 Bucket Policy**  

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### **Giải thích ví dụ**  
- **Effect**: Hành động được **cho phép** (`"Allow"`).  
- **Principal**: Tài khoản AWS có ARN `"arn:aws:iam::123456789012:root"`.  
- **Action**: API được cho phép là `"s3:GetObject"` (tải file).  
- **Resource**: Tất cả các đối tượng trong bucket `"my-bucket"`.  



## **Bucket Settings for Block Public Access**  


<img width="801" alt="image" src="https://github.com/user-attachments/assets/aa44e013-2fc0-43ec-ac10-c81ece3853fc" />

### **Mặc định của S3**  
- S3 mặc định **block tất cả public access** để đảm bảo an toàn.  
- Khi tạo một bucket mới, tất cả các file (objects) trong đó đều được thiết lập là **private**, nghĩa là không ai bên ngoài có thể truy cập.  
  - Ví dụ: Nếu bạn thử kiểm tra **Object URL**, truy cập sẽ bị từ chối.  

### **Cách cấu hình để public file**  
Nếu bạn muốn cho phép public access (truy cập công khai), cần thực hiện **2 bước sau**:  
1. **Tắt Block Public Access**  
   - Tắt tùy chọn "Block Public Access" trong cài đặt bucket.  

2. **Thiết lập Bucket Policies**  
   - Cấu hình chính sách bucket để cho phép người dùng bên ngoài truy cập vào file.  
   - Ví dụ: Một Bucket Policy với hiệu lực cho phép (`Allow`) và hành động `"s3:GetObject"` để mọi người có thể tải file.  

## **Amazon S3 – Static Website Hosting**  

<img width="308" alt="image" src="https://github.com/user-attachments/assets/86859de2-0cd1-488c-9763-60a15dac2ca4" />

### **Khái niệm**  
- S3 hỗ trợ **Static Website Hosting**, cho phép bạn sử dụng bucket để lưu trữ và phân phối các website tĩnh (HTML, CSS, JS).  

### **Cách thực hiện**  
1. **Bật tính năng Static Website Hosting**  
   - Truy cập vào cài đặt bucket và **enable Static Website Hosting**.  

2. **Chỉ định file index tĩnh**  
   - Chỉ định file **index** (ví dụ: `index.html`) làm file mặc định được tải khi truy cập website.  

## **Amazon S3 – Versioning**  

<img width="308" alt="image" src="https://github.com/user-attachments/assets/f1af5d8a-d686-4c8f-a5b0-d3edf9553444" />

### **Khái niệm**  
- **Versioning** là tính năng theo dõi và quản lý nhiều phiên bản của cùng một file (object) trong bucket.  
- Mỗi lần chỉnh sửa và upload lại file, một **version mới** sẽ được tạo.  

### **Lợi ích của Versioning**  
- **Tránh mất dữ liệu**:  
  - Giúp ngăn ngừa việc xóa nhầm file.  
- **Khôi phục dữ liệu**:  
  - Sau khi xóa, có thể khôi phục các phiên bản trước đó của file.  

### **Cách sử dụng**  
- Nếu cần quản lý phiên bản file, hãy **bật Versioning** trong cài đặt bucket.

## **Amazon S3 – Replication (CRR & SRR)**  

### **Khái niệm**  
- **Replication** là tính năng cho phép S3 **đồng bộ toàn bộ nội dung** từ một S3 bucket này sang một S3 bucket khác.  
- Quá trình này là **bất đồng bộ**, tức là dữ liệu từ bucket nguồn sẽ được sao chép sang bucket đích mà không cần phải đồng bộ ngay lập tức.  

### **Các loại Replication**  
1. **Cross-Region Replication (CRR)**  
   - Đồng bộ dữ liệu giữa các bucket ở các **region khác nhau**.  
   - Thường dùng khi muốn sao chép dữ liệu giữa các vùng địa lý khác nhau để tối ưu hiệu suất và độ bền.  

2. **Same-Region Replication (SRR)**  
   - Đồng bộ dữ liệu giữa các bucket trong **cùng một region**.  
   - Thường dùng trong trường hợp sao lưu hoặc phục hồi dữ liệu trong cùng một khu vực.  

### **Ứng dụng thực tế**  
- Replication thường được sử dụng trong các trường hợp như:  
  - **Đồng bộ log** từ nhiều region hoặc nhiều tài khoản vào một nơi duy nhất để dễ dàng phân tích và quản lý.

## **Amazon S3 – Replication (Notes)**

- Khi bật tính năng Replication, chỉ các object mới được replicate, các object trước đó sẽ không được replicate.
- Để replicate các objects trước đó, có thể sử dụng **S3 Batch Replication**, cho phép thao tác hàng loạt với những files có sẵn trong S3.
- Replication không có tính chất bắc cầu (A → B) mà không thể làm ngược lại (B → C).


## **S3 Storage Classes: Tầng Lưu Trữ Của S3**

S3 có 7 tầng lưu trữ khác nhau, mỗi tầng có mục đích và chi phí riêng:

1. **Amazon S3 Standard - General Purpose**  
   - Tầng tiêu chuẩn của S3 dành cho các mục đích chung, như lưu ảnh, video, truy cập thường xuyên hoặc hosting web.
   - Độ khả dụng cao, thích hợp cho data truy cập thường xuyên.
   - **Use Cases**: Big Data analytics, mobile & gaming applications, content distribution,…
   - Đây là loại chi phí đắt nhất.

2. **Amazon S3 Standard-Infrequent Access (IA)**  
   - Giống S3 Standard (tốc độ, độ khả dụng) nhưng chi phí rẻ hơn.
   - Dùng lưu trữ data ít truy cập (1 vài lần mỗi tuần, tháng, năm).
   - Lấy file ra sẽ mất phí.
   
3. **Amazon S3 One Zone-Infrequent Access**  
   - Giống S3 Standard-Infrequent Access, nhưng data lưu trữ chỉ trên 1 zone (độ khả dụng thấp hơn).
   - Rẻ hơn S3 Standard-Infrequent Access.

4. **Amazon S3 Glacier**  
   - Dùng lưu trữ lâu dài cho những thứ ít đụng đến, ví dụ như log.
   - Giá rẻ nhất với tần suất truy cập cực kỳ thấp.
   - Có 3 loại:
     - **Amazon S3 Glacier Instant Retrieval**: Cần file lấy ngay.
     - **Amazon S3 Glacier Flexible Retrieval**: Cần file nhưng phải đợi một thời gian (rẻ hơn Instant Retrieval).
     - **Amazon S3 Glacier Deep Archive**: Rẻ nhất nhưng lấy file lâu từ 12 đến 48h và yêu cầu lưu trữ tối thiểu 180 ngày.

5. **Amazon S3 Intelligent-Tiering**  
   - Dùng cho dữ liệu có mô hình truy cập thay đổi. S3 tự động chuyển các objects giữa các lớp lưu trữ để tối ưu chi phí và hiệu suất.


## **Amazon S3 – Quy tắc Vòng đời (Kịch bản 1)**

**Kịch bản:**  
Ứng dụng trên EC2 tạo ra các ảnh thumbnail sau khi ảnh hồ sơ được tải lên Amazon S3. Các thumbnail này có thể dễ dàng được tạo lại và chỉ cần lưu trữ trong 60 ngày.  
Ảnh gốc cần có khả năng truy xuất ngay lập tức trong 60 ngày đầu tiên, và sau đó người dùng có thể đợi tới 6 giờ để truy xuất.

**Giải pháp thiết kế:**
- **Ảnh gốc**: Lưu trên **S3 Standard** và cấu hình vòng đời để chuyển sang **S3 Glacier** sau 60 ngày.
- **Thumbnail**: Lưu trên **S3 One-Zone IA** và cấu hình vòng đời để xóa (expire) sau 60 ngày.

## **Amazon S3 – Quy tắc Vòng đời (Kịch bản 2)**

**Kịch bản:**  
Một quy định trong công ty yêu cầu các đối tượng S3 bị xóa phải có khả năng khôi phục ngay lập tức trong 30 ngày, mặc dù trường hợp này hiếm khi xảy ra. Sau khoảng thời gian đó và kéo dài đến 365 ngày, các đối tượng bị xóa phải có khả năng khôi phục trong vòng 48 giờ.

**Giải pháp thiết kế:**
- Kích hoạt **S3 Versioning** để duy trì các phiên bản đối tượng. Khi đối tượng bị xóa, chúng chỉ bị ẩn bởi một "delete marker" và có thể khôi phục lại được.
- Chuyển các phiên bản không phải hiện tại (noncurrent versions) sang **S3 Standard IA** sau 30 ngày.
- Chuyển các phiên bản không phải hiện tại sang **S3 Glacier Deep Archive** sau 365 ngày.

## **Amazon S3 Analytics – Storage Class Analysis**

<img width="357" alt="image" src="https://github.com/user-attachments/assets/14ac7b05-a9f0-4078-87a4-c9f8e0e36689" />


- Tính năng này cho phép xuất một file CSV, trong đó liệt kê tình trạng các tầng lưu trữ trong S3.
- Dùng cho mục đích phân tích và đánh giá hiệu quả sử dụng các tầng lưu trữ.

## **S3 – Requester Pays**

<img width="624" alt="image" src="https://github.com/user-attachments/assets/64eca2d7-eb8b-4303-9209-25203bdb103f" />


- Tính năng yêu cầu người dùng trả tiền cho chi phí networking khi sử dụng S3. Điều này bao gồm chi phí lưu trữ và truyền tải dữ liệu.
- Khi áp dụng tính năng này, người dùng phải trả tiền khi tải file về và phải thực hiện trong môi trường AWS.
- Trong thực tế, tính năng này ít khi được sử dụng, nhưng nên biết để tham khảo.


## **S3 Event Notifications**

<img width="440" alt="image" src="https://github.com/user-attachments/assets/9b1f458a-d8d0-411f-82b0-c79b4af9d5ab" />

- Mỗi khi có thao tác trên S3, có thể bắt các sự kiện (events) và từ những sự kiện này, có thể trigger các service khác.
- Ví dụ: Mỗi khi upload ảnh lên S3, có thể chạy Lambda để xử lý và thu nhỏ ảnh.
- Hiện tại, S3 hỗ trợ 3 destinations: **SNS**, **SQS**, và **Lambda**.

## **S3 – Baseline Performance**

- Mỗi folder trên S3 có thể chịu được đồng thời:
  - 3,500 **PUT/COPY/POST/DELETE** requests mỗi giây.
  - 5,500 **GET/HEAD** requests mỗi giây (tương tự traffic).
- Để tăng performance, nên chia nhỏ các folder. Nếu lưu tất cả vào 1 folder, với 1 triệu người dùng sẽ gây vấn đề hiệu suất.


## **S3 Performance – Tính Năng Upload**

- **Multi-Part Upload:**

  <img width="458" alt="image" src="https://github.com/user-attachments/assets/f309ceb9-23e5-41b6-a30d-e2562980bfed" />

  - Tính năng chia nhỏ files thành nhiều phần và upload song song các phần đó lên S3.
  - Thường được sử dụng cho các file có dung lượng lớn hơn 5GB.

- **S3 Transfer Acceleration:**

  <img width="533" alt="image" src="https://github.com/user-attachments/assets/12073d44-ec69-44fc-bf8e-37e8bd14602f" />

  - Tính năng tự động điều hướng request upload file đến điểm cung cấp dịch vụ gần nhất của AWS.
  - Từ điểm cung cấp dịch vụ đó, dữ liệu sẽ được truyền qua đường truyền nội bộ và đến destination.

## **S3 Performance – S3 Byte-Range Fetches**

- Khi sử dụng **S3 Byte-Range Fetches**, bạn có thể chia nhỏ một file lớn thành các đoạn byte cụ thể và yêu cầu tải các đoạn đó song song.
- Điều này giúp giảm thời gian tải dữ liệu. Nếu một đoạn tải thất bại, chỉ cần tải lại đoạn đó mà không phải tải lại toàn bộ file.

## **S3 Select & Glacier Select**

<img width="533" alt="image" src="https://github.com/user-attachments/assets/faf257c4-5c53-4655-8949-9d09c2ad8083" />

- Tính năng cho phép thực hiện **query** trực tiếp trên file trong S3, giúp giảm bớt việc tải toàn bộ file về để xử lý.


## **S3 Batch Operations**

<img width="447" alt="image" src="https://github.com/user-attachments/assets/a3ef8355-73e3-4f47-a45d-e80c5f9d1a08" />

- Tính năng áp dụng thao tác hàng loạt đối với các files, ví dụ: chỉnh sửa hàng loạt, gắn tags hàng loạt, xóa hàng loạt,…
- Trong thực tế, tính năng này ít được sử dụng.


## **S3 – Storage Lens**

<img width="1131" alt="image" src="https://github.com/user-attachments/assets/37e19dfc-0c6e-489c-a2f1-2891bc2dc3db" />

- Tính năng cho phép hiển thị toàn bộ tình trạng sử dụng S3 của bạn dưới dạng các dashboard.

## **Amazon S3 – Object Encryption**

S3 hỗ trợ 2 hình thức mã hóa:

- **Server-Side Encryption (SSE):**  
  Sau khi file được upload lên S3, nó sẽ được mã hóa.  
  Có 3 loại:
  - **SSE-S3**  
  - **SSE-KMS**  
  - **SSE-C**

- **Client-Side Encryption:**  
  Mã hóa file trước khi upload lên S3.

## **Amazon S3 Encryption – SSE-S3**

<img width="1061" alt="image" src="https://github.com/user-attachments/assets/0a776163-f6ec-4da9-9fdb-9cdbac4cf5e2" />

- Đây là loại mã hóa mặc định trên S3. Nếu không chỉ định loại mã hóa, S3 sẽ tự động sử dụng SSE-S3.
- Việc thực hiện mã hóa và giải mã sẽ do S3 tự động xử lý.


## **Amazon S3 Encryption – SSE-KMS**

<img width="1008" alt="image" src="https://github.com/user-attachments/assets/ac8fa8bb-326f-4154-b4d2-31da1712c57e" />

- Việc thực hiện mã hóa và giải mã vẫn do S3 xử lý, nhưng khóa mã hóa (key) sẽ do bạn quản lý.
- Bạn sẽ quản lý khóa mã hóa thông qua dịch vụ **AWS KMS** (Key Management Service).
- Bạn có nhiều quyền hạn hơn để kiểm soát cách mã hóa, ví dụ:  
  - Ai đã sử dụng khóa này để mã hóa dữ liệu.  
  - Ai đã sử dụng khóa để tải dữ liệu xuống.

## **Amazon S3 Encryption – SSE-C**

<img width="976" alt="image" src="https://github.com/user-attachments/assets/b25c85eb-f3a2-41e1-b07c-fad0ee381d41" />

- Khi upload file lên S3, bạn phải kèm theo key (key này do bạn tự quản lý).
- Tính năng này thực tế ít được sử dụng.


## **Amazon S3 Encryption – Client-Side Encryption**

<img width="933" alt="image" src="https://github.com/user-attachments/assets/64bcf44b-0d4a-4f00-b704-45ca010f7a9c" />


- **Client-Side Encryption** là quá trình mã hóa trước khi upload file lên S3, tương tự như việc gửi một file zip cho ai đó, bạn sẽ đặt mật khẩu cho file đó để mã hóa, và sau đó cung cấp mật khẩu cho người nhận để giải mã.
- Cụ thể là bạn sẽ tạo mật khẩu cho file, mã hóa file và upload lên S3. Khi người dùng tải file về, họ cũng cần mật khẩu để giải mã file.


## **Amazon S3 – Encryption in Transit (SSL/TLS): Mã Hóa Trên Đường Truyền**

<img width="1017" alt="image" src="https://github.com/user-attachments/assets/56dbb6ae-0a8c-4a24-a1f4-1221ba7c248e" />

- Để bảo mật dữ liệu khi truyền tải, cần cấu hình **bucket policies**.
- Deny bất kỳ request nào sử dụng HTTP và chỉ cho phép HTTPS.


## **Amazon S3 – CORS (Cross-Origin Resource Sharing)**

<img width="1060" alt="image" src="https://github.com/user-attachments/assets/09f63f91-b720-494f-9d40-198ab775513e" />

- Nếu hệ thống của bạn cần truy cập từ cùng một domain thì không có vấn đề gì, nhưng nếu frontend và backend ở các domain khác nhau, mặc định sẽ bị chặn.
- Để cho phép các request này, bạn cần cấu hình **CORS**.
  - Ví dụ: Nếu bạn host website trên **bucket 1** và muốn truy xuất file từ **bucket 2**, thì **bucket 2** phải cấu hình CORS để cho phép lấy file từ **bucket 1**.
- Bạn có thể bật CORS trực tiếp từ **S3 Console**.


## **S3 Access Logs**

<img width="297" alt="image" src="https://github.com/user-attachments/assets/07eb40ca-2f93-4724-b74b-5df320ab1e16" />

- Cơ chế ghi lại toàn bộ các request đến S3 và lưu trữ chúng sang một S3 bucket khác.
- Ví dụ: Bạn có thể ghi lại thông tin ai đã xem file gì, xóa hay chỉnh sửa file nào. Điều này giúp bạn lưu lại thông tin để tránh việc xóa nhầm hoặc trong trường hợp bị tấn công, bạn vẫn có thể truy vết lại.



## **Amazon S3 – Pre-Signed URLs**

<img width="178" alt="image" src="https://github.com/user-attachments/assets/043034d5-1c45-4acf-815f-4da46400eaa3" />

- **Pre-Signed URLs** cho phép cấp quyền truy cập tạm thời vào một file trên S3 cho người dùng ngoài hệ thống mà không cần cấp quyền trực tiếp.
  - Ví dụ: Một file có trạng thái private sẽ bị từ chối truy cập. Nếu muốn người dùng truy cập vào file đó trong một thời gian nhất định mà không cần chỉnh sửa policies, bạn có thể sử dụng Pre-Signed URLs để cấp quyền tạm thời cho họ.


## **S3 Glacier Vault Lock**

<img width="369" alt="image" src="https://github.com/user-attachments/assets/20478c5a-88fe-4248-8996-f6b4c0c21879" />

- Tính năng giống như một "hộp khóa" để bảo vệ các dữ liệu quan trọng, bạn có thể đóng lại và đảm bảo không ai có thể truy cập vào những thứ trong đó trong khoảng thời gian nhất định. Thường dùng để bảo quản các nội dung tuyệt mật.
- Có 2 chế độ:
  - **Compliance:** Người dùng có quyền cao nhất cũng không thể thao tác với Vault (tương tự như S3 Bucket).
  - **Governance:** Người có quyền cao vẫn có thể chỉnh sửa và thay đổi Vault.


## **S3 – Access Points**

<img width="1027" alt="image" src="https://github.com/user-attachments/assets/17589624-b054-4efd-9147-727d608a579c" />

- Cho phép tạo và chia sẻ nhiều điểm truy cập khác nhau đối với cùng một S3 bucket.
- Mỗi folder có thể cung cấp một điểm truy cập riêng biệt, phục vụ từng đối tượng người dùng và đảm bảo tính bảo mật.
- Truy cập S3 theo cách này tương tự như truy cập S3 thông thường.


## **S3 Object Lambda**

<img width="697" alt="image" src="https://github.com/user-attachments/assets/ace77657-0087-4f6b-9030-77969e216180" />

- Cho phép gia công dữ liệu trước khi trả về người dùng.
- Ví dụ: Có một file chung, nhưng ba hệ thống khác nhau sẽ yêu cầu định dạng dữ liệu khác nhau. Hệ thống 1 cần XML, hệ thống 2 cần JSON, và hệ thống 3 cần một định dạng khác.
- Bạn phải viết chức năng xử lý dữ liệu trong **Lambda** để chuyển đổi các định dạng dữ liệu theo yêu cầu.




