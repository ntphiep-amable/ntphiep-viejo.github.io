+++
title = '[Data Warehouse] Kiến Thức Tổng Quan Về Data Warehouse (Kho Dữ Liệu)'
date = 2023-09-24T03:43:26+07:00
draft = false

tags = ["data", "datawarehouse", "data science"]
showTableOfContents = true
+++


# Lời mở đầu
Bài này là mình dịch và sửa lại từ một bài viết thấy khá hay, chi tiết và đầy đủ. Link bài viết mình để ở dưới nhé!. Chúc mọi người một ngày vui vẻ  😁.
 
## Data Warehouse??
**Data Warehouse (Kho dữ liệu)** là một hệ thống quản lý dữ liệu được sử dụng để lưu trữ và tính toán dữ liệu, cho phép thực hiện các hoạt động phân tích như chuyển đổi (transforming) và chia sẻ (sharing) dữ liệu. Nó giúp doanh nghiệp nắm bắt và lưu trữ dữ liệu từ các nguồn bên ngoài. Các kỹ sư phân tích và nhà phân tích dữ liệu sử dụng nó để truy vấn các tập dữ liệu bằng SQL, biến chúng thành các mô hình (models) và báo cáo dữ liệu mạnh mẽ. Data warehouse là nguồn trung tâm cho bất kỳ ngăn xếp dữ liệu hiện đại nào. Dữ liệu được nhập, chuyển đổi và chia sẻ (imported, transformed, and shared) với các công cụ khác từ kho.

Hiện tại, có 2 loại data warehouse chính: **On-prem (tại chỗ)** và **Cloud (nền tảng đám mây)**. On-prem warehouse là một vị trí thực tế nơi các công ty cần duy trì phần cứng và phần mềm để lưu trữ dữ liệu. Trong khi đó, cloud warehouse có sẵn ở mọi nơi và không bao gồm vị trí thực tế bạn cần truy cập, tuy nhiên, bạn sẽ phải trả tiền để sử dụng không gian lưu trữ và sức mạnh tính toán do một công ty thứ 3 khác cung cấp và duy trì. Có thể kể đến như [AWS (Amazon Web Services)](https://aws.amazon.com/), [GCP (Google Cloud Platform)](https://cloud.google.com), ...

![image.png](https://images.viblo.asia/e904f257-0043-4553-8336-ea9adcc1f432.png)

## Nguồn gốc của Data warehouse
Mặc dù dữ liệu đã được lưu trữ trong suốt lịch sử nhưng phải đến những năm 1980, công nghệ mới bắt đầu tăng tốc và Data warehouse thức đầu tiên được tạo ra. Đó là một on-prem warehouse bao gồm rất nhiều storage towers (tháp lưu trữ) và các vi xử lý máy tính, chiếm rất nhiều không gian. Và như bạn có thể tưởng tượng, điều này gây ra rất nhiều vấn đề. Nó không chỉ chiếm nhiều không gian vật lý mà nhân viên còn phải bảo trì phần cứng và phần mềm của nhưng thiết bị cấu hình warehouse này. Điều này nhanh chóng trở nên tốn kém và không thực tế đối với các công ty nhỏ hơn không có ngân sách hoặc không gian.

Khi `Amazon` bắt đầu mở rộng quy mô kho dữ liệu tại chỗ để hỗ trợ hoạt động kinh doanh của mình, họ nhận thấy cơ hội bán năng lực tính toán cho các doanh nghiệp khác để tiết kiệm chi phí :))). Đây là lúc `Redshift`, sản phẩm cloud data warehouse của`Amazon`, ra đời. Ngay sau đó, những gã khổng lồ công nghệ khác như `Google` và `Microsoft` cũng đang xây dựng cơ sở hạ tầng dữ liệu cũng làm theo.

Giờ đây, bạn có thể  tiếp cận và sử dụng sức mạnh của cloud warehouse ở bất cứ đâu. Bạn không cần phải tự mình duy trì cơ sở hạ tầng nữa mà có thể trả tiền cho một công ty để làm việc này cho bạn. Điều này rẻ hơn so với on-prem phải trả để duy trì hệ thống và cho phép khả năng dữ liệu nhanh hơn.

## Tại sao các doang nghiệp cần phải có Data warehouse
Data warehouse đã từng được cho không thực tế do chi phí liên quan đến chúng. Giờ đây, kho lưu trữ đám mây cung cấp chúng cho gần như tất cả mọi người, chúng mang lại rất nhiều lợi ích cho doanh nghiệp. Kho đám mây cho phép khả năng mở rộng, tính sẵn có, tiết kiệm chi phí và tăng cường bảo mật - tất cả đều do chính nhà cung cấp xử lý. Các tiện ích có thể được liệt kê:

- **Scalability** (Khả năng mở rộng): Data warehouse cho phép bạn mở rộng quy mô tính toán lên hoặc xuống tùy thuộc vào tốc độ bạn cần chạy các phép biến đổi của mình và số tiền bạn sẵn sàng chi tiêu. Bạn cũng có thể bật hoặc tắt tài nguyên máy tính để tiết kiệm chi phí
- **Availability** (Sẵn có): Data warehouse luôn có sẵn. Mặc dù độ trễ có thể thay đổi tùy theo vị trí nguồn và đích nhưng dữ liệu của bạn có thể được truy cập ở mọi nơi, mọi lúc, rất tiện lợi. Điều này trở nên lý tưởng trong xã hội hiện đại, nơi mọi người có thể làm việc từ bất cứ đâu.
- **Cost savings** (Tiết kiệm chi phí): So với on-prem warehouse, cloud warehouse tiết kiệm hơn nhiều vì bạn không còn cần phải bảo trì tất cả cơ sở hạ tầng nên bạn có thể tiết kiệm chi phí liên quan đến bảo trì. Các công ty kho dữ liệu quản lý rất nhiều dữ liệu nên họ có thể tiết kiệm chi phí mà bạn không thể làm được.
- **Security** (Tính bảo mật): Data warehouse cung cấp các tính năng bảo mật nâng cao để đảm bảo dữ liệu của bạn luôn được bảo mật. Nó thường trực tiếp xử lý các chiến lược tuân thủ nhất định cần thiết đối với từng loại dữ liệu khác nhau , giúp bạn không cần phải tự mình thực hiện việc này. Nó cũng có các tính năng như vai trò và người dùng giúp bạn kiểm soát ai có quyền truy cập vào dữ liệu của mình. Nhưng chúng ta sẽ đi sâu vào vấn đề này sau.

![image.png](https://images.viblo.asia/2c048d0a-ef16-4663-8420-20bb4c56d770.png)


## Tiềm năng trong ứng dụng doanh nghiệp
Các doanh nghiệp có thể tận dụng kho dữ liệu vì nhiều lý do khác nhau. Hầu hết những lý do này đều giúp tiết kiệm thời gian và tiền bạc cho doanh nghiệp, dù trực tiếp hay gián tiếp.

### Hợp nhất tất cả dữ liệu về một nơi
Thay vì để tất cả dữ liệu của bạn trải rộng trên các nền tảng khác nhau, dữ liệu đó có sẵn cho bạn ở một nơi. Điều này cho phép bạn chuẩn hóa tất cả các metrics (chỉ số cốt lõi) và definitions (định nghĩa dữ liệu) của mình, thay vì phụ thuộc vào các chỉ số được tính toán bởi các nền tảng như `Google` và `Facebook`. Nếu bạn thấy rằng các số liệu khác nhau không phù hợp trên các nền tảng thì data warehouse sẽ đóng vai trò là nguồn đáng tin cậy cho số liệu phù hợp. Thay vì dựa vào các nền tảng bên ngoài, giờ đây bạn đã có một nền tảng tập trung tất cả dữ liệu của mình.

Chưa kể, bạn sẽ khiến kỹ sư (DE) và nhà phân tích dữ liệu (DA) của mình phải đau đầu. Nếu không, họ sẽ phải lấy dữ liệu cần thiết từ nhiều nguồn khác nhau theo cách thủ công. Việc không có một nguồn thông tin chính xác duy nhất sẽ làm giảm chất lượng dữ liệu của bạn, lãng phí thời gian của nhóm dữ liệu và gây khó khăn cho việc kết hợp dữ liệu từ các nguồn khác nhau.

### Khả năng kiểm soát các quyền truy cập và loại quyền truy cập của các đối tượng
Data warehouse có các tính năng bảo mật mở rộng cho phép bạn kiểm soát ai có quyền truy cập vào nội dung gì. Bạn có khả năng cấp cho ai đó các quyền ít hoặc nhiều tùy theo ý muốn của bạn. Nó cũng cung cấp cho bạn khả năng tạo người dùng và gán vai trò cho họ. Mỗi vai trò có bộ quyền riêng đối với cơ sở dữ liệu và bảng mà nó có thể xem. Sau đó, bạn cũng có thể chọn người được phép thực hiện query (truy vấn) các bảng đó hoặc thậm chí update (cập nhật) và delete (xóa) chúng.

Khi bất kỳ ai trong tổ chức của bạn có thể dễ dàng truy cập vào dữ liệu của bạn, điều tồi tệ có thể xảy ra. Nguy cơ dữ liệu quan trọng có thể bị xóa, chỉnh sửa sai hoặc truy cập không thích hợp. Người dùng, vai trò, chính sách và biện pháp bảo mật của kho dữ liệu có thể giúp đảm bảo dữ liệu nằm trong tay đúng người.

### Thích hợp cho Fast reporting (báo cáo nhanh)
Vì tất cả dữ liệu của bạn đều nằm ở cùng một nơi nên nó cho phép báo cáo nhanh hơn so với việc lấy dữ liệu từ nhiều nguồn khác nhau. Vị trí trung tâm cho phép bạn truy cập và truy vấn nhanh chóng hàng triệu hàng dữ liệu, cho phép thực hiện chuyển đổi và báo cáo nhanh hơn nhiều.


## Các Data platform hỗ trợ data warehousing workloads
Hiện nay có nhiều các nền tảng cung cấp, hỗ trợ Data warehouse dưới dạng dịch vụ, có thể kể đến như:

| **Data platform** | **Mô tả** | 
|---|---|
| `Snowflake` ![image.png](https://images.viblo.asia/90b8da35-1fe9-492f-a4e1-0044edfb0799.png) | Snowflake là một nền tảng được quản lý hoàn toàn để lưu trữ dữ liệu, hồ dữ liệu (data lake), kỹ thuật dữ liệu, khoa học dữ liệu và phát triển ứng dụng dữ liệu. |
| `Databricks` ![image.png](https://images.viblo.asia/9d73e77a-6492-4124-b2b4-954ba456816b.png) | Databricks là một nền tảng phân tích dữ liệu, kỹ thuật dữ liệu và khoa học dữ liệu cộng tác dựa trên đám mây, kết hợp những gì tốt nhất của data warehouse và data lake vào kiến trúc lakehouse 😀. |
| `Google BigQuery` ![image.png](https://images.viblo.asia/9eb7ddf0-e926-4621-8bbb-c725e6819096.png) | Google BigQuery là một serverless (không máy chủ) warehouse, có khả năng mở rộng cao, đi kèm với công cụ truy vấn tích hợp.. |
| `Amazon Redshift` ![image.png](https://images.viblo.asia/fc0d1bee-09e9-422c-ad48-0cd16b6f67d8.png)| Amazon Redshift là data warehouse dựa trên đám mây có quy mô petabyte được quản lý hoàn toàn, được thiết kế để lưu trữ và phân tích tập dữ liệu quy mô lớn (biggg data). |
| `Postgres` ![image.png](https://images.viblo.asia/dec421af-eeef-42e0-8b0b-e30d1f3fbcde.png) | PostgreSQL là một cơ sở dữ liệu quan hệ mã nguồn mở cấp doanh nghiệp nâng cao hỗ trợ cả truy vấn SQL (quan hệ) và JSON (không quan hệ). |


## Data warehouse vs Data lake
Data lake (hồ dữ liệu) là một hệ thống nơi bạn lưu trữ, xử lý và truy vấn dữ liệu phi cấu trúc, bán cấu trúc và có cấu trúc ở hầu hết mọi quy mô. Sự khác biệt chính giữa data warehouse và data lake là loại và cách lưu trữ dữ liệu. Data warehouse chứa dữ liệu có cấu trúc nhằm tổ chức dữ liệu để sử dụng phân tích, trong hi đó Data lake có thể chứa khá nhiều loại dữ liệu—có cấu trúc hoặc không cấu trúc—và dữ liệu thường được giữ nguyên ở định dạng thô cho đến khi sẵn sàng sử dụng.

Hiểu đơn giản thì data lake chứa đủ mọi loại mọi kiểu dữ liệu tùm lum, một phần dữ liệu có cấu trúc trong data lake sẽ được load vào data warehouse trong quá trình ETL hoặc ELT.

## Kết luận
Data warehouse đã đi một chặng đường dài trong 40 năm qua. Nó bắt đầu như một bộ máy vật lý thực tế với chi phí khổng lồ đến  một hệ thống có sẵn cho bất kỳ ai, ở bất kỳ đâu với chi phí phải chăng. Nó có khả năng tập trung tất cả dữ liệu của doanh nghiệp bạn, cho phép thực hiện các hoạt động phân tích nhanh hơn, KPI được tiêu chuẩn hóa và một nguồn thông tin đáng tin cậy duy nhất. Tất cả các doanh nghiệp đều cần một kho dữ liệu để hoạt động nhanh chóng và hiệu quả với dữ liệu mà họ có thể dựa vào. Câu hỏi không phải là bạn có cần data warehouse hay không mà là bạn nên chọn loại data warehouse nào. 

# Reference
https://docs.getdbt.com/terms/data-warehouse