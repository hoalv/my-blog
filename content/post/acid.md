+++
author = "Le Viet Hoa"
title = "Transaction - ACID"
date = "2021-03-03"
description = "Transaction & ACID"
tags = [
    "data",
    "deverloper",
    "system"
]
categories = [
    "technology"
]
thumbnail = "images/tet.png"
+++
Các hệ thống dữ liệu trong thực tế, có thể xảy ra nhiều sai sót:
• Phần mềm hoặc phần cứng của cơ sở dữ liệu (database) có thể bị lỗi bất cứ lúc nào (kể cả khi đang có hành động ghi dữ liệu vào database).
• Ứng dụng có thể gặp sự cố (kể cả khi đang thực hiện một loạt các hành động giữa chừng).
• Sự gián đoạn trong mạng có thể làm mất kết nối ứng dụng khỏi database một cách bất ngờ hoặc một node database này khỏi node database khác.
• Một số máy khách có thể ghi vào cơ sở dữ liệu cùng một lúc, ghi đè các thay đổi của nhau.
• Client có thể đọc dữ liệu không chính xác vì nó mới chỉ được cập nhật một phần.
Để đáng tin cậy, một hệ thống phải xử lý những lỗi này và đảm bảo rằng chúng không gây ra sự cố nghiêm trọng cho toàn bộ hệ thống. Tuy nhiên, việc thực hiện các cơ chế chịu lỗi rất phức tạp và tốn công. Nó đòi hỏi nhiều suy nghĩ cẩn thận về tất cả những sai sót có thể xảy ra và rất nhiều thử nghiệm để đảm bảo rằng giải pháp thực sự hoạt động.
Trong nhiều thập kỷ, trasaction (giao dịch) là cơ chế được lựa chọn để đơn giản hóa những vấn đề này. Hiểu đơn giản giao dịch là cách nhóm nhiều hành động đọc và ghi dữ liệu lại với nhau thành một đơn vị logic. Về mặt khái niệm, tất cả các thao tác đọc và ghi trong một giao dịch được thực hiện như một hoạt động: toàn bộ giao dịch thành công (commit) hoặc không thành công (abort, rollback). Nhờ các giao dịch, việc xử lý lỗi trở nên đơn giản hơn nhiều cho một ứng dụng, vì ứng dụng không cần phải lo lắng về lỗi một phần — tức là trường hợp một số hành động thao tác với dữ liệu thành công và một số hành động không thành công (vì bất kỳ lý do gì). Mặc dù thoạt nhìn các giao dịch có vẻ đơn giản, nhưng trước tiên chúng ta cần hiểu chính xác các giao dịch đảm bảo an toàn cho dữ liệu như thế nào và những chi phí đi kèm theo đó. 
Hầu như tất cả các cơ sở dữ liệu quan hệ ngày nay và một số cơ sở dữ liệu không quan hệ (NOSQL) đều hỗ trợ các giao dịch. Hầu hết chúng tuân theo phong cách được giới thiệu vào năm 1975 bởi IBM Sys‐ tem R, cơ sở dữ liệu SQL đầu tiên [1, 2, 3]. Mặc dù một số chi tiết triển khai đã thay đổi, nhưng ý tưởng chung vẫn giữ nguyên trong 40 năm: giao dịch trong MySQL, PostgreSQL, Oracle, SQL Server, v.v., tương tự như giao dịch trong System R. Vào cuối những năm 2000, cơ sở dữ liệu không quan hệ (NoSQL) bắt đầu trở nên phổ biến. NoSQL ra đời nhằm mục đích cải thiện các nhược điểm của cơ sở dữ liệu quan hệ bằng cách đưa ra các mô hình dữ liệu mới, đi cùng với các khái niệm về replication và partitioning. Các giao dịch là điểm yếu chính của sự chuyển đổi này: nhiều cơ sở dữ liệu thế hệ mới này đã từ bỏ hoàn toàn các giao dịch hoặc định nghĩa lại tập các đảm bảo mà yếu hơn nhiều so với trước đây. Xung quanh sự xuất hiện của các cơ sở dữ liệu phân tán mới này, nhiều người tin rằng các giao dịch và khả năng mở rộng không thể đi cùng nhau và bất kỳ hệ thống quy mô lớn nào cũng sẽ phải từ bỏ các giao dịch để duy trì hiệu suất tốt và tính sẵn sàng cao. Ở một ý kiến khác, đảm bảo giao dịch đôi khi được các nhà cung cấp cơ sở dữ liệu đưa ra như là một yêu cầu thiết yếu đối với “các ứng dụng nghiêm túc” và “dữ liệu có giá trị”. Cả hai quan điểm đều hơi thái quá. Sự thật không đơn giản như vậy: giống như mọi lựa chọn thiết kế kỹ thuật khác, giao dịch có những ưu điểm và hạn chế. Để hiểu những đánh đổi đó, hãy đi vào chi tiết về các đảm bảo mà giao dịch có thể cung cấp.
Các đảm bảo an toàn được cung cấp bởi các giao dịch thường được mô tả bằng từ viết tắt ACID, chúng là Atomicity, Consistency, Isolation và Durability,  được đặt ra vào năm 1983 bởi Theo Härder và Andreas Reuter. Hãy cùng tìm hiểu sâu hơn về các khái niệm này.
Atomicity (Tính nguyên tử)
Nguyên tử dùng để chỉ một thứ không thể chia nhỏ thành các phần nhỏ hơn. 
