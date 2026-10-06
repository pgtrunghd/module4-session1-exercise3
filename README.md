# Tư duy thiết kế theo Domain (DDD)

## Bối cảnh và vấn đề

Dự án phát triển nhiều năm, chức năng liên tục được bổ sung và mã nguồn dần trở thành một **Big Ball of Mud**: các phần phụ thuộc chéo, quy tắc nghiệp vụ nằm rải rác, một thay đổi nhỏ gây lỗi ở nơi khác. Team Leader đề xuất áp dụng Domain-Driven Design (DDD).

DDD là cách tiếp cận thiết kế phần mềm bắt đầu từ **nghiệp vụ và mô hình chung giữa người làm nghiệp vụ với người phát triển**. Mục tiêu là diễn đạt đúng các quy tắc quan trọng của doanh nghiệp trong mã nguồn, xác định ranh giới trách nhiệm rõ ràng và giúp hệ thống thay đổi cùng nghiệp vụ. DDD không đồng nghĩa với việc phải dùng microservices hay một bộ mẫu thiết kế cố định.

## 1. Ưu điểm của DDD

### Gắn kết nghiệp vụ và mã nguồn

Nhóm phát triển cùng chuyên gia nghiệp vụ tìm hiểu các quy tắc, thuật ngữ và tình huống ngoại lệ trước khi thiết kế. Các khái niệm nghiệp vụ được phản ánh trong tên gọi và hành vi của mã nguồn thay vì bị che khuất dưới những hàm chung chung như `processData()`.

Ví dụ, trong bài toán bán hàng, “Đơn hàng được xác nhận” là một trạng thái nghiệp vụ với điều kiện cụ thể; nó không đơn thuần là cập nhật cột `status`. Mô hình có thể quy định rằng đơn hàng chỉ được xác nhận khi còn hàng và đã nhận được hình thức thanh toán hợp lệ. Khi quy tắc thay đổi, nhóm biết cần sửa ở đâu.

### Tạo ngôn ngữ chung (Ubiquitous Language)

Người làm nghiệp vụ, người phân tích, lập trình viên và kiểm thử viên dùng cùng một thuật ngữ trong trao đổi, tài liệu, test và mã nguồn. Điều này giảm hiểu lầm giữa các bộ phận.

Ví dụ, “khách hàng” có thể là người mua trong ngữ cảnh Bán hàng nhưng là người nhận hỗ trợ trong ngữ cảnh Chăm sóc khách hàng. DDD không ép hai ngữ cảnh dùng một mô hình duy nhất; nhóm cần nêu rõ ý nghĩa của thuật ngữ trong từng **Bounded Context** (ngữ cảnh giới hạn).

### Làm rõ ranh giới và giảm phụ thuộc

DDD ở cấp chiến lược giúp xác định các Bounded Context theo năng lực nghiệp vụ, chẳng hạn Đặt hàng, Thanh toán và Kho. Mỗi ngữ cảnh có mô hình và quy tắc riêng; việc tích hợp giữa chúng cần có hợp đồng rõ ràng. Nhờ vậy, nhóm hạn chế tình trạng mọi module cùng sửa một mô hình dữ liệu chung và phụ thuộc lẫn nhau.

### Tập trung nguồn lực vào logic cốt lõi

DDD khuyến khích phân biệt **Core Domain** (nơi tạo lợi thế cạnh tranh), **Supporting Subdomain** (nghiệp vụ hỗ trợ) và **Generic Subdomain** (chức năng phổ thông). Doanh nghiệp nên đầu tư phân tích và thiết kế sâu cho phần logic tạo giá trị khác biệt; những phần phổ thông có thể dùng giải pháp sẵn có nếu phù hợp.

### Dễ kiểm thử và thay đổi quy tắc hơn

Khi quy tắc nghiệp vụ được đặt trong mô hình có trách nhiệm rõ ràng, nhóm có thể viết test theo tình huống nghiệp vụ thay vì chỉ kiểm tra controller hoặc thao tác cơ sở dữ liệu. Điều này giúp phát hiện sai lệch khi quy tắc thay đổi và giảm rủi ro hồi quy.

## 2. Nhược điểm và khó khăn khi áp dụng

| Khó khăn | Ảnh hưởng thực tế | Cách giảm rủi ro |
| --- | --- | --- |
| **Đường cong học tập cao** | Nhóm dễ hiểu sai các khái niệm như Entity, Value Object, Aggregate và Bounded Context; áp dụng máy móc làm mã nguồn phức tạp hơn. | Học qua một luồng nghiệp vụ nhỏ, review mô hình với người có kinh nghiệm và chỉ dùng mẫu thiết kế khi giải quyết vấn đề cụ thể. |
| **Tốn thời gian tìm hiểu nghiệp vụ** | Cần workshop, phỏng vấn và xác thực mô hình với chuyên gia nghiệp vụ; tiến độ ban đầu có thể chậm. | Ưu tiên các quy tắc quan trọng, làm theo từng phần và ghi lại quyết định bằng ví dụ nghiệp vụ. |
| **Cần sự tham gia liên tục của chuyên gia nghiệp vụ** | Nếu chuyên gia không có thời gian, mô hình có thể phản ánh suy đoán của lập trình viên thay vì thực tế. | Phân công đầu mối nghiệp vụ, thống nhất lịch trao đổi và kiểm tra mô hình bằng các tình huống thật. |
| **Ranh giới ban đầu có thể sai** | Chia ngữ cảnh không hợp lý khiến nhiều phần phải gọi qua lại hoặc trùng lặp dữ liệu khó quản lý. | Bắt đầu với giả thuyết, quan sát các luồng thay đổi và điều chỉnh ranh giới khi hiểu nghiệp vụ sâu hơn. |
| **Khó áp dụng vào mã nguồn cũ** | Quy tắc đang nằm rải rác; sửa toàn bộ một lần dễ gây lỗi và ngưng trệ phát triển. | Khoanh một phần có nhiều thay đổi, tạo lớp chuyển đổi với mã cũ và di chuyển từng luồng có kiểm thử. |
| **Chi phí thiết kế có thể vượt lợi ích** | Với ứng dụng CRUD đơn giản, mô hình phức tạp làm tăng số lớp và thời gian phát triển mà không giải quyết vấn đề đáng kể. | Áp dụng DDD ở mức phù hợp; tập trung sâu vào Core Domain, giữ phần đơn giản thật đơn giản. |

## 3. Đánh giá tính khả thi cho dự án hiện tại

Đề xuất DDD **có cơ sở** nếu nguyên nhân chính của “Big Ball of Mud” là các quy tắc nghiệp vụ phức tạp, thuật ngữ không thống nhất và ranh giới module mờ. Tuy nhiên, DDD không tự động sửa được mã nguồn chỉ bằng cách đổi tên class hoặc thêm các lớp `Entity`, `Repository`, `Service`. Nếu vấn đề chủ yếu là thiếu kiểm thử, quy ước code hoặc quy trình review, nhóm cũng cần giải quyết trực tiếp những vấn đề đó.

Lộ trình áp dụng từng bước:

1. **Khảo sát hiện trạng:** Chọn một luồng nghiệp vụ gây nhiều lỗi hoặc thay đổi thường xuyên; ghi lại các thuật ngữ, quy tắc và điểm phụ thuộc chéo.
2. **Làm việc với chuyên gia nghiệp vụ:** Dùng ví dụ thực tế để làm rõ điều kiện, ngoại lệ và ý nghĩa của từng thuật ngữ; xây dựng ngôn ngữ chung.
3. **Xác định ngữ cảnh giới hạn:** Phân biệt mô hình và quyền sở hữu dữ liệu của từng ngữ cảnh; ghi rõ cách chúng trao đổi với nhau.
4. **Thiết kế lại một phần nhỏ:** Đưa quy tắc của luồng được chọn vào mô hình nghiệp vụ có test; dùng lớp chống sai lệch (*Anti-Corruption Layer*) nếu cần kết nối với phần mã cũ có mô hình khác.
5. **Đo kết quả rồi mở rộng:** Theo dõi thời gian thực hiện thay đổi, số lỗi hồi quy và mức độ dễ hiểu của mã nguồn. Chỉ mở rộng cách làm khi nhóm thấy lợi ích rõ ràng.

Ví dụ, nếu doanh nghiệp có quy trình định giá nhiều điều kiện đặc thù, đó có thể là Core Domain cần mô hình hóa kỹ. Chức năng gửi email thông báo thông thường có thể giữ cách triển khai đơn giản hoặc dùng dịch vụ có sẵn. Hai phần không cần mức đầu tư thiết kế như nhau.

## Kết luận

DDD giúp nhóm hiểu và diễn đạt **logic cốt lõi của doanh nghiệp** trong phần mềm. Giá trị lớn nhất nằm ở sự thống nhất giữa chuyên gia nghiệp vụ và đội phát triển, ngôn ngữ chung và ranh giới mô hình rõ ràng. Đổi lại, nhóm phải đầu tư thời gian học hỏi, trao đổi nghiệp vụ và cải tiến mã nguồn từng bước. Với dự án đang thành “Big Ball of Mud”, nên thử DDD ở phần nghiệp vụ phức tạp nhất và đánh giá kết quả trước khi áp dụng rộng hơn.
