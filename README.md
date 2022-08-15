# I. Bối Cảnh
Bạn là một Data Analyst làm việc cho một công ty thương mại điện tử tên là X. Bạn được giao nhiệm vụ chuẩn bị một bài thuyết trình để trình bày tổng quan tình hình kinh doanh và vận hành của công ty tính đến thời điểm hiện tại cho Giám đốc bán hàng và Giám đốc vận hành. Bài thuyết trình tối thiểu phải bao gồm các thông tin sau: tổng quan tình hình kinh doanh, mức độ hài lòng của khách hàng, và đề xuất 2 đến 3 lĩnh vực (areas) mà công ty có thể cải thiện. 
# II. Phân tích
### 1. Tình hình kinh doanh theo năm và tháng của công ty
Mã đoạn code của bảng tổng hợp
```php
turnover= pd.pivot_table(df,index='month', columns='year', values='payment_value', aggfunc=np.sum, fill_value=0).reset_index(drop= False)
```
Output: 

![tên ảnh](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/291134670_800840021084850_8783757632599999263_n.png?_nc_cat=100&ccb=1-7&_nc_sid=ae9488&_nc_ohc=HUBUrVLkz0EAX_3rr57&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVKqDgvnwA-GwBZiMvKvbo8C94b5JaEz-qV97jCotAvsIw&oe=63203A54)
#### A. Tình hình kinh doanh theo năm
```php
turnover_year= df.groupby('year')['payment_value'].sum().reset_index()
xs,ys= turnover_year['year'], turnover_year['payment_value']
plt.figure(figsize=(15,10))
plt.bar(xs,ys, color= sns.color_palette('Paired'));
plt.xlabel('year')
plt.ylabel('payment_value')
for x,y in zip(xs,ys):
    
        label = "{:.2f}".format(y)

        plt.annotate(label, # this is the text
                    (x,y), # these are the coordinates to position the label
                    textcoords="offset points", # how to position the text
                    xytext=(0,10), # distance from text to points (x,y)
                    ha='center') # horizontal alignment can be left, right or center

plt.show()
```
output:

![tên ảnh](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/294129672_3207277942844020_7132723186535871234_n.png?_nc_cat=108&ccb=1-7&_nc_sid=ae9488&_nc_ohc=C-F9mgOJEioAX-zullu&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIm_eAd1ZnkraWaKJuWXrXGcoJZ7y8FvvotwukvUIH5_w&oe=631EAFD3)
Nhìn vào biểu đồ thì tình hình kinh doanh của công ty qua các năm có sự tăng trưởng mạnh mẽ, nguồn doanh thu tăng nhanh qua các năm, năm 2016 doanh thu chỉ vỏn vẹn là 6.470$, năm 2017 doanh thu tăng lên 8.456567$ và năm 2018 là 11362962$ từ đó ta thấy tổng giá trị đơn hàng mà công ty bán qua các năm tăng cao và mang lại rất nhiều doanh thu.
=> Doanh thu của công ty tăng cao có thể thấy được tình hình kinh doanh của công ty có cải thiện tốt và phát triển mạnh qua các năm.
#### B. Tình hình kinh doanh theo tháng
```php
turnover_month= df.groupby('month')['payment_value'].sum().reset_index()
x,y= turnover_month['month'], turnover_month['payment_value']
plt.figure(figsize=(15,10))
plt.plot(x,y)
plt.xlabel('month')
plt.ylabel('payment_value')
for a,b in zip(x,y):
    
        label = "{:.2f}".format(b)

        plt.annotate(label, # this is the text
                    (a,b), # these are the coordinates to position the label
                    textcoords="offset points", # how to position the text
                    xytext=(0,10), # distance from text to points (x,y)
                    ha='center') # horizontal alignment can be left, right or center

plt.show()
```
output:

![Github](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/293831931_450349693677349_7679001683496339833_n.png?_nc_cat=106&ccb=1-7&_nc_sid=ae9488&_nc_ohc=DpltahS9cU0AX9UHlBl&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJWlUiSZmw_jwt7Q-kFEmfFPKbV9cXgQtYtt5hVMBdMUA&oe=631FE4AF)

-Nhìn vào biểu đồ thì doanh thu các đơn hàng qua các tháng chưa có sự đồng đều, cụ thể từ tháng 1 cho tới tháng 8 doanh thu tăng lên qua từng tháng đặt biệt là tháng 8 doanh thu rất là cao chênh lệch rất là rõ so với các tháng còn lại.
