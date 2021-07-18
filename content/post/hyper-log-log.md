+++
author = "Le Viet Hoa"
title = "Hyperloglog: Tập đếm"
date = "2021-07-10"
description = "Hyperloglog: Tập đếm"
tags = [
    "data",
    "algorithm"
]
categories = [
    "technology"
]
thumbnail = "images/hyperloglog.png"
+++
*"Buổi mai hôm ấy, một buổi mai đầy sương thu và gió lạnh. Mẹ tôi âu yếm nắm tay tôi dẫn đi trên con đường làng dài và hẹp. Con đường này tôi đã quen đi lại lắm lần, nhưng lần này tự nhiên tôi thấy lạ. Cảnh vật chung quanh tôi đều thay đổi, vì chính lòng tôi đang có sự thay đổi lớn: Hôm nay tôi đi học."*  
Những cảm xúc mà ai cũng một lần trải qua trong đời, chỉ là lúc đó chúng ta còn quá nhỏ không nhận thức được, và khi nhận thức được thì chúng ta cũng không phải nhà văn như Thanh Tịnh để diễn tả cái cảm xúc đấy một cách văn học như thế này :D 
Anyway, để hiểu tại sao tôi lại trích đoạn một tác phẩm văn học vào đây thì bạn hãy đọc tiếp, xin thề là có liên quan :(  
Một trong những bài học đầu tiên khi cắp sách tới trường, và bắt đầu bước vào thế giới toán học đáng sợ đó là tập làm quen với những con số, để làm quen với các con số chúng ta có bài học tập đếm. Và dụng cụ có sẵn nhất, được phép mang vào mọi phòng thi dành cho việc đếm tất nhiên là ngón tay, ngón chân. Sau này khi biết được các số lớn hơn 10, tay chân không đủ nữa thì dùng que tính. Tôi vẫn nhớ hồi đó tôi có vài bó que tính, mỗi bó một màu riêng xanh, đỏ, vàng, xanh đủ cả. Mỗi lần học đếm xong thì tất cả các màu đã bị trộn lẫn với nhau hết, nên phải ngồi lúi húi nhặt từng màu ra thành bó riêng như ban đầu. Số que tính khi đó của tôi chỉ có 4 màu, tôi biết vì từ lúc mua đã bó thành từng màu riêng như thế. Nhưng hãy giả sử nếu các que tính là đống hỗn độn như khi tôi mới làm bài tập tập đếm xong, và số lượng que tính giả sử là rất rất lớn, và nói chung số màu que tính cũng rất lớn.  
Vậy có cách nào để biết được có bao nhiêu màu trong đống que tính kia, chả lẽ ngồi đếm từng que tính và xem có bao nhiêu màu trong đó! Không thể, tôi đã nói số que tính là rất rất lớn mà. Câu hỏi này trong khoa học máy tính được gọi là vấn đề đếm duy nhất (Count-distinct problem). Hãy cùng nhau tìm hiểu một phương pháp để giải bài tập tập đếm này nhé, phương pháp đó chính là thuật toán Hyperloglog.  
Hãy lấy một ví dụ tương tự như việc tìm ra số màu trong tập các que tính phía trên. Giả sử rằng có một buổi triển lãm tranh của một hoạ sỹ nổi tiếng kéo dài cả tháng trời, người xem kéo đến ầm ầm, kín cả sảnh mỗi ngày. Bạn là người soát vé ở lối vào và ngoài ra còn có một nhiệm vụ đếm xem có bao nhiêu khách tham gia buổi triển lãm, tất nhiên 1 khách chỉ được đếm một lần dù họ có thể đến buổi triền lãm nhiều lần vào các ngày khác nhau. Bạn có thể nghĩ đến ngay một cách: chuẩn bị một tệp giấy, ghi lại định danh của từng vị khách, ví dụ là số điện thoại chẳng hạn (hãy nhớ điều này, chúng ta sẽ sử dụng số điện thoại ở phía dưới), sau đó chỉ việc đếm trong danh sách đấy. Eww, a lot of work! Khách thì quá đông, bạn phải ghi quá nhiều, đang ghi hết giấy thì sao, kể cả ghi xong thì việc đếm duy nhất trên số khách đó cũng là không tưởng. Hãy làm cho vấn đề khó hơn nữa, bạn cần phải trả về kết quả ngay lập tức, hay thậm chí sếp chỉ cho phép bạn sử dụng tay chân để đếm như thời tiểu học. Nhưng thật tuyệt vời, hoàn toàn có cách để đáp ứng tất cả các yêu cầu trên của sếp bạn.  

**Flajolet-Martin Algorithm**  
Bạn hoàn toàn có thể đếm được số lượng khách gần như ngay lập tức mà chỉ cần đếm bằng đầu ngón tay. Philippe Flajolet và G. Nigel Martin đã giới thiệu một phương pháp giải quyết vấn đề này vào năm 1984. Bạn chỉ cần ghi lại số số 0 trong chuỗi số 0 đứng đầu liên tiếp dài nhất mà bạn thấy được trong các số điện thoại những vị khách.  
- Ví dụ vị khách đầu tiên có số điện thoại là 532885, không có số 0 nào đứng đầu, bạn ghi xuống 0. 
- Vị khách thứ 2 có số điện thoại 042311, chuỗi 0 đứng đầu dài nhất có một chữ số 0, bạn ghi xuống 1. 
- Sau một vài khách chuỗi 0 đứng đầu cũng chỉ có một hoặc không có số 0 nào đứng đầu, thì bạn gặp một vị khác có số điện thoại 009989, có hai số 0 đứng ở đầu, lúc này bạn ghi xuống 2.

Khi bạn nhìn thấy hơn 10 người, chuỗi 0 đứng đầu dài nhất sẽ có nhiều khả năng là 1. Tương tự, khi bạn thấy hơn 100 người, chuỗi dài nhất sẽ có nhiều khả năng là 2. Dễ dàng nhận thấy rằng trong một tập dữ liệu ngẫu nhiên, trung bình, một chuỗi có k số 0 liên tiếp đứng ở đầu sẽ xuất hiện một lần sau mỗi 10ᴷ phần tử.  
Bạn hiểu ý tưởng rồi phải không. Vậy là, dựa trên xác suất, số khách tham gia sẽ gần bằng 10ᴸ, với L là dãy số 0 ở đầu dài nhất mà bạn tìm thấy trong tất cả các số điện thoại.  

Thực tế trong bài báo của mình vào năm 1984, các tác giả đã băm (hash) đầu vào (số điện thoại) để có được các đầu ra là chuỗi nhị phân được phân phối đồng đều hơn. Ví dụ: họ có thể băm một phần tử sđt1 thành 010001 và một phần tử khác sdt2 thành 101000. Nhờ đó, kết quả không bị ảnh hưởng do các số điện thoại có thể có cùng định dạng như mã vùng,... Ngoài ra, bởi vì họ đã biến đầu ra thành một chuỗi bit nhị phân, kết quả đếm bây giờ sẽ là 2ᴸ thay vì 10ᴸ.
Tuy nhiên, phân tích thống kê cho thấy rằng kết quả 2ᴸ có sai số có thể dự đoán được. Vì vậy, họ thêm một hệ số hiệu chỉnh ϕ ≈ 0,77351 để hoàn thành công thức cuối cùng: 2ᴸ / ϕ . Thuật toán này gọi là Flajolet-Martin Algorithm.

**LogLog: Sự cải tiến**  
Đọc thuật toán phía trên, hắn có bạn sẽ thắc mắc, nếu xuất hiện một khách hàng có số điện thoại siêu đẹp có nhiều số 0 ở đầu (giả sử 000000010), mọi tính toán phía trên sẽ sai hết bởi vì vị khách ngoại lệ này. Việc này trên thực tế luôn xảy ra. Và các tác giả của thuật toán này cũng biết điều đó, may quá=))  
Vậy làm thế nào để kết quả ước tính của chúng ta ít bị ảnh hưởng bới các trường hợp ngoại lệ này. Giải pháp có thể nghĩ ngay ra được là chúng ta sẽ lặp lại thuật toán Flajolet-Martin nhiều lần với các hàm băm sử dụng là khác nhau, sau đó lấy kết quả trung bình. Ví dụ: nếu chúng ta thu được chuỗi số 0 đứng đầu dài nhất bằng cách sử dụng m hàm băm khác nhau, ở đây chúng ta biểu thị giá trị độ dài của chuỗi số 0 đứng đầu dài nhất là L₁, L₂,…, Lₘ, thì ước tính cuối cùng của chúng ta sẽ trở thành: m * 2 ^ (( L₁ +… + Lₘ) / m))

Tuy nhiên, việc băm một đầu vào với nhiều hàm băm có thể khá tốn kém về mặt tính toán. Do đó, các tác giả đã đưa ra một giải pháp: sử dụng một hàm băm duy nhất và sử dụng một phần đầu ra của nó để chia giá trị thành nhiều nhóm (buckets) khác nhau, họ sử dụng một vài bit đầu tiên của giá trị băm làm chỉ số của bucket và đếm chuỗi dài nhất của các số 0 đứng đầu dựa trên các bit còn lại của chuỗi.  
Ví dụ cho dễ hiểu này. Nếu chúng ta muốn có 4 nhóm, chúng ta có thể sử dụng 2 bit đầu tiên của đầu ra giá trị băm làm chỉ số của các nhóm. Giả sử chúng ta có 4 phần tử và nhận các giá trị băm của chúng như sau :
- Hash(sđt1) = 100101 thuộc bucket số 2 (bucket: 10) với độ dài chuỗi số 0 đứng đầu dài nhất = 1 (phần còn lại: 0101)
- Hash(sđt2) = 010011 thuộc bucket số 1 (bucket: 01) với độ dài chuỗi số 0 đứng đầu dài nhất = 2 (phần còn lại: 0011)
- Hash(sđt3) = 001111 thuộc bucket số 0 (bucket: 00) với độ dài chuỗi số 0 đứng đầu dài nhất = 0 (phần còn lại: 1111)
- Hash(sđt4) = 110101 thuộc bucket số 3 (bucket: 11) với độ dài chuỗi số 0 đứng đầu dài nhất = 1 (phần còn lại: 0101)

Giá trị trung bình của chuỗi số 0 ở đầu dài nhất trong tất cả các nhóm là (0 + 2 + 1 + 1) / 4 = 1 . Do đó, ước lượng của chúng ta ở đây là 4 * 2¹ . Chú ý rằng gía trị này ở đây không gần với giá trị thực tế vì chúng ta đang xem xét trên rất ít mẫu, tôi chỉ muốn bạn hiểu được ý tưởng.
Bạn có thể tìm thêm thông tin chi tiết về hệ số hiệu chỉnh ϕ cho LogLog trong bài báo năm 2003 của các tác giả.  
Với LogLog, sử dụng lấy trung bình để làm giảm sai số. Sai số chuẩn của LogLog là 1,3 / √m, với m là số lượng bucket.

**SuperLogLog: siêu Loglog :D**  
Các tác giả của thuật toán này tiếp tục làm việc để làm giảm sai số của thuật toán. Họ phát hiện ra rằng độ chính xác có thể được cải thiện đáng kể bằng cách loại bỏ một số các giá trị lớn nhất mà họ nhận được từ các buckets trước khi tính trung bình. Cụ thể hơn, khi thu thập các giá trị từ các buckets, chúng ta có thể giữ lại 70% giá trị nhỏ nhất và loại bỏ phần còn lại, sau đó lấy giá trị trung bình của các buckets. Bằng cách đó, sai số chuẩn được cải thiện từ 1,3 / √m xuống 1,05 / √m. Thật amazing phải không ? Họ đã quyết định đặt cho phương pháp này một cái tên, chính là: SuperLogLog

**HyperLogLog: Anh on top, em ở trên anh**  
Vào năm 2007, tác giả Flajolet cuối cùng đã tìm ra giải pháp cuối cùng của mình cho bài toán ước lượng số lượng này. Giải pháp này là HyperLogLog, được ông gọi là "thuật toán ước lượng số lượng gần như tối ưu". Ý tưởng đằng sau nó rất đơn giản: thay vì sử dụng trung bình cộng để tính trung bình kết quả mà chúng ta nhận được từ LogLog, chúng ta sẽ sử dụng trung bình điều hoà!  
Ví dụ, trung bình điều của 1, 2, 4 là 3 / (1/1 + 1/2 + 1/4) = 3 / (1,75) = 1,714  
Tại sao sử dụng trung bình điều hoà? Bởi vì nó rất tốt trong việc xử lý các ngoại lệ lớn. Ví dụ, xem xét trung bình điều hoà của 2, 4, 6, 100:
4 / (1/2 + 1/4 + 1/6 + 1/100) = 4.32  
Hệ số ngoại lệ lớn 100 ở đây ít ảnh hưởng đến kết quả vì chúng ta chỉ sử dụng nghịch đảo của nó. Do đó, chúng ta có một phương pháp tính trung bình có thể ít bị ảnh hưởng bởi các giá trị ngoại lệ lớn.  
Bằng cách sử dụng trung bình điều hoà thay vì trung bình cộng được sử dụng trong LogLog và chỉ sử dụng 70% giá trị nhỏ nhất trong SuperLogLog, HyperLogLog đạt được sai số chuẩn là 1,04 / √m, thấp nhất trong số tất cả các phương pháp.

Bây giờ chúng ta đã hiểu cách hoạt động của HyperLogLog. Thuật toán này có thể ước tính số lượng giá trị duy nhất trong một tập dữ liệu rất lớn bằng cách sử dụng ít bộ nhớ và thời gian. Chú ý rằng trong bài báo gốc, các tác giả không thực sự đếm chuỗi số 0 đứng đầu dài nhất, ở đây chúng ta đơn giản hoá để giúp dễ dàng nắm bắt ý tưởng của thuật toán này. Các bạn muốn biết rõ hơn nên tìm đọc trực tiếp các bài báo gốc.

Vậy là chúng ta vừa cùng nhau quay trở về tuổi thơ với bài toán tập đếm. Đây thực sự là một thuật toán rất thú vị khi tôi đọc được, nó đã đánh bại sự lười biếng của tôi để ngồi dịch/viết ra bài này. Have good day!

Tài liệu tham khảo:
- https://towardsdatascience.com/hyperloglog-a-simple-but-powerful-algorithm-for-data-scientists-aed50fe47869
- https://medium.com/@winwardo/counting-crowds-hyperloglog-in-simple-terms-1d345637db5


