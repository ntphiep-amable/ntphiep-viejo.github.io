+++
title = 'DBT (Data Build Tool) Là Gì? Những Thứ Cơ Bản Về DBT'
date = 2023-08-05T03:43:26+07:00
draft = false

tags = ["python", "data", "dbt", "etl"]
showTableOfContents = true
+++


# Lời mở đầu

![](https://images.viblo.asia/9927b339-597a-448d-823d-ce2a37fe0e04.png)


Các bạn DE, DA khi tìm search từ khóa **"dbt"** trên Google thường sẽ ra nhiều kết quả khác nhau về những cụm từ có viết tắt là "dbt", và không may thay là cái mà DE, DA cần lại thường không được suggest đầu tiên :))). Cái mình nhắc đến hôm nay là **Data Build Tool**, một công cụ mã nguồn mở hỗ trợ chủ yếu việc transform data. Dạo gần đây thấy nó nổi lên khá là mạnh và được sử dụng tương đối nhiều.


## I. DBT là gì?


![](https://images.viblo.asia/ea9dcb65-fb62-46e0-8ee8-5d0827c0227a.png)


Đối với các bạn DA, muốn xây dựng một BI Model, việc module hóa quá trình **transform data** là thực sự cần thiết. Có rất nhiều tool hỗ trợ transform mạnh mẽ có thể kể đến như Pandas (python), Apache Spark, R, Apache Nifi, ..... Tuy nhiên để có thể dễ dàng sử dụng cho các bạn đã biết SQL thì dbt là một lựa chọn rất tốt vì nó hỗ trợ module hóa các câu lệnh SQL, vậy nên các bạn chỉ cần có nền tảng SQL có thể dễ dàng sử dụng.

Như đã lói, nó là một tool open-source được xây dựng và phát triển bởi [RJMetrics](https://en.wikipedia.org/wiki/RJMetrics) vào năm 2016. Mục đích ban đầu và cũng là mục đích chính của dbt là **transform** data. Data build tool sủ dụng **SQL** dó đó quá trình transform trở nên nhanh và dễ dàng hơn.


Điều làm dbt trở nên đặc biệt là dbt có thể giúp một Analyst bình thường có thể thực hiện được những công việc cơ bản của Engineer (chủ yếu là transform - biến đổi dữ liệu). DBT giúp việc transform, document, test data trở nên dễ dàng hơn và có thể nhân rộng được. Thực hiện các điều trên cũng trở nên đơn giản hơn thông qua việc sử dụng các công cụ của dbt chứ bạn không cần phải set up hệ thống test và viết document tách biệt.




![](https://images.viblo.asia/31e92ca2-441a-40df-8708-14b03e3ec142.png)



Nguồn: https://www.getdbt.com/product/what-is-dbt/

## II. Các điểm nổi bật của dbt
Lý do dbt được lựa chọn trong việc transform dữ liệu cho cả những người mới và trong cả các công ty  là bởi dbt có rất nhiều tính năng hay ho. Dưới đây mình sẽ liệt kê một vài tính năng và công cụ rất tiện lợi mà dbt cung cấp:

### 1. Rút gọn boilerplate (đoạn code mang tính hình thức)
Mỗi DBMS (hệ quản trị cơ sở dữ liệu) sẽ sủ dụng một loại SQL khác nhau. Ví dụ như Oracle sử dụng PL-SQL trong khi Microsoft SQL Server sử dụng T-SQL, .... Cơ bản thì nó vẫn là SQL tuy nhiên mỗi loại lại có những có pháp, cách sủ dụng có phần khác nhau không nhiều thì ít, kiểu như có loại sẽ dùng LIMIT, có loại dùng TOP, có loại dùng kiểu STRING  có loại lại là VARCHAR, db có gốc Java thì có kiểu DOUBLE còn mấy cái khác thì không v.v....

Cũng vì lý do này mà không mấy ai thích việc phải viết và define từng table, column hay insert data vào database trực tiếp bằng SQL ( thậm chí developer còn tránh làm điều này mà dùng ORM để thay thế 🙃). DBT sinh ra để một phần cải thiện vấn đề này. Nó giúp lược bỏ những đoạn code mang tính hình thức. Điều này có thể giúp bạn thao tác tốt hơn với các table với số lượng lớn và phụ thuộc vào nhau.

Ví dụ bạn có một table tên `customer`, bạn muốn tạo một bảng mới tên là `person` với một số cột từ bảng `customer`, với SQL bình thường thì phải làm như sau:

```sql
DROP TABLE IF EXISTS person;

CREATE TABLE person AS (
    SELECT ID, FirstName, LastName, City, Address FROM customer;
)

```


Còn với dbt, bạn chỉ cần viết:

```sql
SELECT ID, FirstName, LastName, City, Address FROM customer
```

Và lưu lại với file tên `person.sql`, đơn giản và nhanh gọn là bạn đẫ có table tên `person`, đầy đủ data y hệt như khi viết SQL bình thường. Ở một ví dụ nhỏ như thế này chúng ta có thể thấy sự khác biệt không nhiều, nhưng trên thực tế rất có ích với quy mô hàng trăm tables cùng 1 lúc và phụ thuộc vào nhau.

### 2. Hỗ trợ đa dạng Data Platform - Data Warehouse

DBT hỗ trợ kết nối và làm việc cùng với rất đa dạng các Data Platform khác nhau. Dưới đây là một số những data platform phổ biến mà dbt có thể kết nối và làm việc.

![](https://images.viblo.asia/b7d5892e-a89d-454c-ad05-ef5074fdebdd.png)


Nguồn: https://docs.getdbt.com/docs/supported-data-platforms

Đối với mỗi data platfom mà bạn muốn làm việc, cần cài đặt 2 thư viện là `dbt-core` và thư viện tương ứng với platform đó. Ví dụ nếu muốn làm việc với **Google BigQuery** thì cài `dbt-bigquery`  nữa là được 
```bash
pip install dbt-core dbt-bigquery
```

### 3. Models hóa abstraction và dependency
Các table trong một database có một abstract dependency ( Tạm gọi là sự lệ thuộc ảo ) với nhau. Khi chạy SQL thông qua dbt, thì các câu SQL sẽ được chạy theo tuần tự như dependency đã được khởi tạo trong dbt-models. 

### 4. Data Lineage và documentation

Thêm nữa, dbt còn cung cấp công cụ document, khi bạn tạo ra dependency, dbt sẽ tự động tạo ra các tài liệu để biểu thị sự phụ thuộc giữa các model. Bạn sẽ không cần phải tự ghi tài liệu mà vẫn có tài liệu để nắm rõ nguồn gốc và mối liên hệ của data. Khá là tiện lợi!!


Ngoài việc có đồ thị cho data lineage, dbt còn hỗ trợ tự động tạo tài liệu cho data. Bạn có thể ghi lại ý nghĩa của từng column, table do bạn tạo ra (tương tự `Metadata` đã nhắc tới trong [Dagster](https://viblo.asia/p/dagster-la-gi-dagster-co-ban-cho-nguoi-moi-bat-dau-WR5JRvKdJGv), tránh trường hợp sau này quên mất chúng là gì.

### 5. Hỗ trợ Jinja template

Jinja là một template engine rất nổi tiếng, ai làm Front-End web chắc hẳn đã nghe qua và sử dụng. Trong trường hợp của dbt, nó sẽ giúp bạn thu gọn những câu lệnh SQL rất dài thành ngắn lại. Ví dụ như  sau đây:

```sql
select *
from {{ ref('my_first_dbt_model') }}
where id = 1
```

Ở đây thay vì các bạn phải ghi chi tiết từ database đến table .... để kết nối tới table `my_first_dbt_model` thì các bạn chỉ cần ghi ngắn gọn lại như vậy và dbt sẽ xử lý đầy đủ.

### 6.  Test tự động
DBT có rất nhiều loại test tự động khác nhau được soạn sẵn, hoặc bạn có thể tự viết bằng dbt macros. Như vậy bạn có thể chuẩn hóa quy trình bằng cách yêu cầu tất cả models pass các test được đề ra mà không cần phải check thủ công từng table.

![](https://images.viblo.asia/ec565b39-1330-415a-9818-eba35dfd5a5d.png)



### 7. Có nền tảng Cloud
DBT có 2 bản Cloud và CLI, bản Cloud có giao diện rất đẹp cùng nhiều tính năng tiện lợi hay ho hơn bản CLI nhiều, và đương nhiên là mất phí rồi :)))

# Lời kết
Cơ bản thì không có một công cụ nào là hoàn hảo về mọi măt. DBT cũng chỉ là một lựa chọn trong số rất nhiều công cụ khác. Trong một dự án phải sử dụng kết hợp nhiều công cụ mới có thể tối uu điểm mạnh. Tuy nhên DBT cũng là một công cụ khá hay ho mà các bạn nên trai nghiệm vì một phần nó không khó để bắt đầu và cũng vì nó đang dần trở nên phổ biến thời gian gần đây. Bài viết mình tổng hợp từ vài nguồn và hiểu biết, thiếu sót mong mọi người góp ý. Cảm ơn mọi người đã đọc!


# Reference
https://tuananalytic.com/dbt-data-build-tool-la-gi/

https://www.getdbt.com/

https://www.getdbt.com/product/what-is-dbt/