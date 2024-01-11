+++
title = '[DBT] Kết Nối DBT Với Big Query'
date = 2023-09-25T03:43:26+07:00
draft = false

tags = ["python", "data", "dbt", "bigquerry"]
showTableOfContents = true
+++

# Lời mở đầu
Ở bài trước - [Giới thiệu DBT](https://viblo.asia/p/dbt-data-build-tool-la-gi-nhung-thu-co-ban-ve-dbt-oK9VyxbxLQR) - mình đã đề cập đến việc dùng dbt kết nối và làm việc với các data platform khác. Hôm nay mình sẽ kết nối đến và thực hiện một vài thao tác cơ bản đến [**Google Big Query**](https://console.cloud.google.com/bigquery) - một trong những Data platform phổ biến nhất thời điểm hiện tại.

![image.png](https://images.viblo.asia/38e57c2e-2ae4-4c34-b5db-b6289bc3e070.png)

Trong bài, mình sẽ dùng một dataset mẫu để mô phỏng quá trình. Dataset tên là `jaffle_shop`. Nếu bạn thực hành với dataset khác, thao tác tương tự 🥲.
## Các setup cần thiết
Để có thể sử dụng một công cụ nào đó, đương nhiên là trước tiên bạn phải cài đặt rồi 😄

- Với dbt, mình đã hướng dẫn cài dbt ở [bài trước](https://viblo.asia/p/dbt-data-build-tool-la-gi-nhung-thu-co-ban-ve-dbt-oK9VyxbxLQR#_i-dbt-la-gi-1), dùng pip đơn giản:
```bash
pip install dbt-core dbt-bigquery
```

Sau đó, kiểm tra lại với lệnh:
```bash
dbt --version
```

- Với Big query, mình sẽ nói ngắn gọn các thao tác cần thiết, chi tiết các bạn có thể tìm đọc các trang hướng dẫn Big Query cơ bản... Trước hết, bạn cần phải có tài khoản Google trước, sau đó truy cập [BigQuery Console](https://console.cloud.google.com/bigquery), tạo một project mới đặt tên gì cũng được. Sau đó, hãy thử với câu query cơ bản sau khi tạo project:

```sql
select * from `dbt-tutorial.jaffle_shop.customers`;
```

Nếu không có gì sai thì result sẽ như sau:
![image.png](https://images.viblo.asia/fdc23372-b6cd-49da-b384-45afcc47cef4.png)

Sau khi thử chạy thành công trên console của Big query, hãy tạo một `dataset` mới ([hướng dẫn](https://cloud.google.com/bigquery/docs/datasets#create-dataset)), ở đây mình đặt là `jaffle_shop`. Ai đã làm việc với Big query sẽ biết `dataset` tương đương với `database` hoặc `schema` trong các hệ cơ sở dữ liệu cơ bản. Các bước tiếp theo là [Tạo Credential Trong Big query](https://docs.getdbt.com/quickstarts/bigquery?step=4), cuối cùng là download keyfile dạng JSON về. Keyfile JSON có format như sau:

```json
{
  "type": "service_account",
  "project_id": "PROJECT_ID",
  "private_key_id": "KEY_ID",
  "private_key": "-----BEGIN PRIVATE KEY-----\nPRIVATE_KEY\n-----END PRIVATE KEY-----\n",
  "client_email": "SERVICE_ACCOUNT_EMAIL",
  "client_id": "CLIENT_ID",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://accounts.google.com/o/oauth2/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/SERVICE_ACCOUNT_EMAIL"
}
```

Vậy là OK phần setup chuẩn bị các thứ rồi 😁, giờ hãy đến phần chính nào!

## Kết nối dbt đến Big query

### Tạo project với dbt

Di chuyển đến thư mục bạn muốn tạo project, chạy lệnh sau:
```bash
dbt init jaffle_shop
```
Như đã nói, mình sẽ lấy tên project là `jaffle_sshop`, bạn có thể đặt tên khác tùy ý. Hãy mở project vừa tạo lên, ở đây mình sử dụng VS-Code. Bạn sẽ thấy những file và folder được tạo trong thư mục project, mỗi file và folder đảm nhận chức năng khác nhau và cần thiết cho project.

![image.png](https://images.viblo.asia/b9db02be-e6e2-4f2e-9f3e-6fb433e6b4d8.png)

### Ý nghĩa các file và folder trong thư mục project
- **[analyses](https://docs.getdbt.com/docs/build/analyses)**: thư mục hỗ trợ tổ chức các truy vấn SQL phân tích trong dự án, chứa các script sql mà analytic cần, cơ mà nó chỉ compile chứ không có excute.
- **[macros](https://docs.getdbt.com/docs/build/jinja-macros)**: chứa các block code có thể tái sử dụng nhiều lần, tiện lợi đỡ phí thời gian, viết 1 lần rồi thích gọi ở đâu cũng được.
- **[models](https://docs.getdbt.com/docs/build/models)**: là thư mục quan trọng nhất của project, đơn giản bởi vì nó là folder chứa các model :))). Mỗi file .sql trong folder này là một model (có thể là table, view, ...).  Khi chạy `dbt run` sẽ tự generate model và insert data.
- **[seeds](https://docs.getdbt.com/docs/build/seeds)**: chứa các file .csv, static data, giúp load data từ file vào các data platform thuận tiện hơn.
- **[snapshots](https://docs.getdbt.com/docs/build/snapshots)**: giúp chứa snapshot data của một table tại một thời điểm nào đó nếu dữ liệu các bảng có thể bị thay đổi.
- **[tests](https://docs.getdbt.com/docs/build/tests)**: đơn giản như cái tên 😀, chứa các query bạn dùng để test các model và resource. Khi chạy `dbt test` sẽ giúp bạn test tự động.
- **[dbt_project.yml](https://docs.getdbt.com/docs/build/projects#project-configuration)**: file config **quan trọng không thể thiếu** của project, chứa các thông tin như `name`, `version`, `profile`, `path`, `vars`, ... cần thiết cho chạy dự án. 

Tạm thời khi mới khởi tạo project thì chỉ có vậy, nhưng sau khi bạn thực hiện kết nối, sẽ có thêm vài file và folder sinh ra thêm như `seed`,  `logs`, `target`, ... Cơ mà lười giải thích nên để sau nhé 😌

### Thực hiện kết nối
DBT sẽ thực hiện kết nối đến [Data warehouse](https://viblo.asia/p/data-warehouse-kien-thuc-tong-quan-ve-data-warehouse-kho-du-lieu-Rk74aoXvLeO) sử dụng [profile](https://docs.getdbt.com/docs/core/connect-data-platform/connection-profiles). Nó được định nghĩa trong một file gọi là `profiles.yml`, trong đó chứa tất cả các thông tin chi tiết cần thiết để thực hiện kết nối đến data warehouse. Ở thư mục `~/.dbt` (nếu là linux) hoặc `C:\Users\Username\.dbt` (nếu là windows), bạn hãy tạo file `profiles.yml`. Sau đó, hãy copy file JSON chứa key đã tải ở phần trên vào cùng thư mục này, tiếp theo hãy copy và sửa các giá trị cho phù hợp.

```yaml
jaffle_shop: # this needs to match the profile in your dbt_project.yml file
    target: dev
    outputs:
    dev:
        type: bigquery
        method: service-account
        keyfile:  # replace this with the full path to your keyfile
        project:  # Replace this with your project id
        dataset:  # Replace this with dbt_your_name
        threads: 1
        timeout_seconds: 300
        location: US
        priority: interactive
```

 - **Note: Tại sao lại để `profiles.yml` ngoài thư mục project?** Đó là bởi vì lý do hạn chế các `sensitive credentials`(các thông tin nhạy cảm) bị check bởi các công cụ version control. Thực ra thì bạn có thể để chung với thư mục project cũng được với điều kiện là nên sử dụng các environment variables (biến môi trường) để load các  `sensitive credentials`. Còn nếu để ngoài thì dbt sẽ tự động tìm trong thư mục `~/.dbt`.
 
 Ô cê, cuối cùng hãy vào thư mục chứa project và chạy:
 
 ```bash
 dbt debug
 ```
 
 Và nếu kết quả hiển thị như lày là bạn đã kết nối thành công rồi!!
 
 ```bash
 Connection test: OK connection ok
 ```
 
 ![image.png](https://images.viblo.asia/7a3a3832-36fc-4e82-bee8-afb4065f1f44.png)
 
 Vậy là thành công kết nối dbt đến Big Query rồi, các bạn có thể thực hiện các công việc như build model, load data, chạy test, schedule jobs, ...Có thể mình sẽ hướng dẫn ở những bài tiếp theo. Rất đơn giản phải khum 😉. Chúc các bạn kết nối thành công nhé!
 
 # Reference
 https://docs.getdbt.com/quickstarts/manual-install?step=1
 
 https://docs.getdbt.com/quickstarts/bigquery?step=1