# I. Bối Cảnh
Bạn là một Data Analyst làm việc cho một công ty thương mại điện tử tên là X. Bạn được giao nhiệm vụ chuẩn bị một bài thuyết trình để trình bày tổng quan tình hình kinh doanh và vận hành của công ty tính đến thời điểm hiện tại cho Giám đốc bán hàng và Giám đốc vận hành. Bài thuyết trình tối thiểu phải bao gồm các thông tin sau: tổng quan tình hình kinh doanh, mức độ hài lòng của khách hàng, và đề xuất 2 đến 3 lĩnh vực (areas) mà công ty có thể cải thiện. 
# II. Phân tích
### 1. Tình hình kinh doanh theo năm và tháng của công ty
Mã đoạn code của bảng tổng hợp
```php
turnover= pd.pivot_table(df,index='month', columns='year', values='payment_value', aggfunc=np.sum, fill_value=0).reset_index(drop= False)
```
Output: 
[tên ảnh](https://imgur.com/a/qoCrlsm)
