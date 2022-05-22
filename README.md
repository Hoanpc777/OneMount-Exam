# Dữ liệu đầu vào
```
https://drive.google.com/file/d/1R6TFr2aCmouWRQx8LoxZFrw4TF2lj7K3/view?usp=sharing
```
# Mục tiêu
Tăng đối đa tổng giá trị (GMV) của công ty và tối ưu hóa chi phí
# Nội dung
## Báo cáo doanh số
1) Doanh số theo khách hàng và khu vực
2) Doanh số theo ngành hàng
3) Doanh số theo hình thức payments
4) Doanh số theo tháng
## Báo cáo chi phí 
1) Tỷ trọng Freight cost so với Item cost
## Báo cáo leadtime (tình hình đặt hàng từ lúc order, dự kiến thời gian giao hàng cho khách)
1) Báo cáo theo dõi đặt h
1) Thời gian 
## Báo cáo review
Trọng số chấm điểm trên 3 điểm là positive (1), từ 3 điểm trở xuống là negative (0)
Do một đơn hàng có một/nhiều đánh giá, nên sẽ tính trung bình điểm đánh giá cho từng đơn hàng
* Tỷ trọng negative/positive
* Đánh giá doanh thu theo ngành hàng phân loại theo điểm đánh giá

## Báo cáo khách hàng
1) Khách hàng theo thành phố, theo bang (số lượng)

# EDA dữ liệu
## Khám phá dữ liệu (explore)
![data schema](img/schema.png)
1) Bảng customers
* Dữ liệu không có giá trị null
* Có 5 trường thông tin
``` 
0   customer_id               99441 non-null  object
1   customer_unique_id        99441 non-null  object
2   customer_zip_code_prefix  99441 non-null  int64 
3   customer_city             99441 non-null  object
4   customer_state            99441 non-null  object
```
2) Bảng products
* Có 9 trường thông tin
* Cột "product_category_name" có giá trị NaN, nên tạm thời phân loại vào danh mục "Others"
```
 0   product_id                  32951 non-null  object 
 1   product_category_name       32951 non-null  object 
 2   product_name_lenght         32341 non-null  float64
 3   product_description_lenght  32341 non-null  float64
 4   product_photos_qty          32341 non-null  float64
 5   product_weight_g            32949 non-null  float64
 6   product_length_cm           32949 non-null  float64
 7   product_height_cm           32949 non-null  float64
 8   product_width_cm            32949 non-null  float64
```
3) Bảng geolocation
* Có 5 trường thông tin, không có giá trị NaN
* Trường "geolocation_zip_code_prefix" không unique, tạm thời dùng tọa độ average trung bình của kinh độ (lng) và vĩ độ (lat) để unique bảng
```
 0   geolocation_zip_code_prefix  1000163 non-null  int64  
 1   geolocation_lat              1000163 non-null  float64
 2   geolocation_lng              1000163 non-null  float64
 3   geolocation_city             1000163 non-null  object 
 4   geolocation_state            1000163 non-null  object 
```
4) Bảng sellers
* Có 4 trường thông tin, trường seller_id là unique
* Không có giá trị NaN
```
 0   seller_id               3095 non-null   object
 1   seller_zip_code_prefix  3095 non-null   int64 
 2   seller_city             3095 non-null   object
 3   seller_state            3095 non-null   object
```
5) Bảng orders
* Có 8 trường thông tin, có các giá trị NaN tùy thuộc vào "order_status"
* Trường "order_id" là unique
```
 0   order_id                       99441 non-null  object
 1   customer_id                    99441 non-null  object
 2   order_status                   99441 non-null  object
 3   order_purchase_timestamp       99441 non-null  object
 4   order_approved_at              99281 non-null  object
 5   order_delivered_carrier_date   97658 non-null  object
 6   order_delivered_customer_date  96476 non-null  object
 7   order_estimated_delivery_date  99441 non-null  object
```
6) Bảng order_items
* Có 7 trường thông tin, mỗi đơn hàng "order_id" có 1 hoặc nhiều mã product/ seller_id
* Chi phí = Price + Freight_value
* Không có giá trị NaN trong bảng
```
 0   order_id             112650 non-null  object 
 1   order_item_id        112650 non-null  int64  
 2   product_id           112650 non-null  object 
 3   seller_id            112650 non-null  object 
 4   shipping_limit_date  112650 non-null  object 
 5   price                112650 non-null  float64
 6   freight_value        112650 non-null  float64
```
7) Bảng order_reviews
* Có 7 trường thông tin
* Trường "order_id" không unique
```
 0   review_id                100000 non-null  object        
 1   order_id                 100000 non-null  object        
 2   review_score             100000 non-null  int64         
 3   review_comment_title     11715 non-null   object        
 4   review_comment_message   41753 non-null   object        
 5   review_creation_date     100000 non-null  datetime64[ns]
 6   review_answer_timestamp  100000 non-null  datetime64[ns]
```
8) Bảng order_payments
* Có 5 trường thông tin
* Trường "order_id" không unique
```
 0   order_id              103886 non-null  object 
 1   payment_sequential    103886 non-null  int64  
 2   payment_type          103886 non-null  object 
 3   payment_installments  103886 non-null  int64  
 4   payment_value         103886 non-null  float64
```
## Khám phá dữ liệu (explore)
