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

-Nhưng dối với những tháng cuối năm doanh thu thì lại không bằng doanh thu của những tháng trước.

=> cần tìm ra nguyên nhân vì sao những tháng cuối năm lại có doanh thu ít hơn những tháng đầu năm.

### 2. Top nhóm sản phẩm (product_category_name) mang lại doanh thu nhiều nhất cho công ty
```php
df.groupby('product_category_name')['payment_value'].sum().nlargest(10).reset_index()
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/298973734_404916611537800_103573572929499479_n.png?_nc_cat=107&ccb=1-7&_nc_sid=ae9488&_nc_ohc=0PbBLbF8Uy0AX8wdI8a&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVKmUaCQa1_x9UBLkGOoyLhI-9sLpok612eoG8gSa11dog&oe=6322F1F0)

Nhìn vào bảng tổng hợp thì những nhóm sản phẩm ở trên bảng đó là nhóm sản phẩm bán chạy nhất và đem lại doanh thu cao nhất cho công ty. Đó là những sản phẩm thế mạnh của cửa hàng công ty vì doanh thu của công ty chủ yếu là do những nhóm sản phẩm đó đem lại vì vậy chúng ta cần phải tập trung nhiều nguồn lực để mà phát triển những sản phẩm thế mạnh của công ty mình lên nữa.

Ví Dụ: chúng ta có thể tặng thêm những cái voucher tặng cho khách hàng khi mua trên mức giá bao nhiêu đó để kích cầu khách hàng có thể mua hàng mình nhiều hơn để mà nhận được những cái voucher giảm giá đó.

### 3. Số đơn hàng giao thành công và bị hủy

```php
delivered_df= df.loc[df['order_status']=='delivered']
canceled_df= df.loc[df['order_status']=='canceled']
# số đơn hàng bị hủy
plt.figure(figsize=(12,6))
sns.countplot(data=canceled_df,x='year');
# số đơn hàng giao thành công
plt.figure(figsize=(12,6))
sns.countplot(data= delivered_df,x='year');
```
Output:
### Số đơn hàng bị hủy
![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/292432145_2920740624738636_759369804800923362_n.png?_nc_cat=107&ccb=1-7&_nc_sid=ae9488&_nc_ohc=qMVoerIKfC8AX_A9Gyk&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVLyYLFSJCVpsdOYX92Y7gVi6F_0mcAiN2Ei75fbPIlj_g&oe=6321DF34),
### Số đơn hàng giao thành công
![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/291624471_933935361334110_1112254248511624582_n.png?_nc_cat=100&ccb=1-7&_nc_sid=ae9488&_nc_ohc=eq0re2sVOEkAX96gIsv&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJYTD4Awgg3BwFHsVtTyJmKBBqQNxUwkxCkBhhMcMQNYQ&oe=631F96BE)

-Nhìn vào biểu đồ đầu tiên số đơn hàng bị hủy rất ít chỉ dưới 10 đơn hàng.
-Nhìn vào biểu đồ thứ hai số đơn hàng giao thành công rất là nhiều chênh lệch với số đơn hàng bị hủy rất nhiều.
=> tỷ lệ giao hàng thành công ty rất là cao cho thấy được tình trạng đơn hàng bị hủy bỏ hoặc bị boom hàng rất là ít, từ đó thấy được tình kinh doanh của công ty khá là ổn điịnh.

### 4. Hình thức thanh toán
```php
plt.figure(figsize=(12,6))
sns.countplot(data=df, x='payment_type');
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/292601378_3248779772029624_5936993273079975103_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ae9488&_nc_ohc=d4y2rthoeHUAX9Rw4OW&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIl9ubFshMyp2taBl_TY93485mlk31i7YxSc280iFFayA&oe=63212A13)

-Dựa vào biểu đò thì hình thức thanh toán mà khách hàng dùng để thanh toán nhiều nhất là credit_card, vì mình công ty thương mại điện tử chính vì thế khi thanh toán bằng credit_card khách hàng sẽ được hưởng một khoảng lợi nhuận từ nó, cho nên lượng khách thanh toán bằng credit_card sẽ là nhiều hơn các hình thức thanh toán khác.

### 5. Mức Hài Lòng của khách hàng.
```php
star= df.groupby('review_score')['customer_id'].count().reset_index(name='count')
ax= star['review_score']
bx= star['count']
plt.figure(figsize=(15,6))
plt.pie(bx, labels=ax ,autopct='%1.1f%%');
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/295655344_1146686159220152_8239129103048987975_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ae9488&_nc_ohc=wSJ0QizKBnkAX-9ya3n&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIwoKiMQkAc7lmpkwHmwuWuWYAXf2tfNFhdHRMBLlFlaA&oe=6321A34B)

- Độ hài lòng của khách hàng với mức 4 và 5 điểm chiếm hơn 70% thì có thể thấy sản phẩm của công ty rất là tốt đáp ứng đúng với nhu cầu cảu khách hàng chúng ta cần phải duy trì và năng mức điểm này lên cao hơn nữa.
-Đối với những sản ohaamr bị đánh giá 1 và 2 điểm thì chúng ta cần phải xem xét lại tại tao những sản phẩm này lại bị đánh giá thấp đến như vậy, cần tìm hiểu nguyên nhân do shop bán hàng có những sản phẩm kém chất lượng k đạt được sự hài lòng của khách hàng..

### 6. Danh sách những nhóm sản phẩm bị đánh giá 1 và 2 điểm
```php
star12= df.loc[(df['review_score']== 1)|(df['review_score']==2)]
pd.crosstab(star12.product_category_name, star12.review_score)
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/299161731_390536316495381_2087884427408624672_n.png?_nc_cat=109&ccb=1-7&_nc_sid=ae9488&_nc_ohc=69DWnicjnq8AX8qLcRk&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJR2Ms9BC-jhlW6WDIGTZxcdZ5ZjwB6OyWZyQyBEAGekw&oe=632123DF)

-Đây là những danh sách nhóm sản phẩm bị đánh giá 1 và 2 điểm chúng ta phải tìm hiểu nguyên nhân từ những id của người bán hàng(seller_id) tại sao khách hàng lại đánh giá 1 và 2 điểm có số điểm thấp đến như vậy, sau khi tìm hiểu nguyên nhân và khắc phục nếu mà những id người bán hàng này bán sản phẩm mà khách hàng vẫn còn đánh giá 1 và điểm nhiều nữa thì chúng ta cần phải xem xét và xử lý việc này , tại vì khi mà khách hàng muốn mua một sản phẩm nào đó mà họ nhìn thấy những lượt chấm của những khách hàng thấp đến như vậy thì họ sẽ e ngại và k còn mua nữa, từ đó lượng khách hàng dùng cái dịch vụ thương mại điện tử của mình để mua hàng sẽ bị giảm xuống, chính vì thế mà chúng ta cần phải khắc phục việc người bán hàng giao đến những sản phẩm hư hỏng hoặc những sản phẩm kém chất lượng cho khách hàng.

### 7. Danh sách id người bán có lượt đánh giá 1 và 2 sao.
```php
star12.groupby('seller_id')['review_score'].count().sort_values(ascending=False).nlargest(10).reset_index()
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/290608499_777055483432470_4269814101361728412_n.png?_nc_cat=101&ccb=1-7&_nc_sid=ae9488&_nc_ohc=OUcv43ALdGoAX-q1jo1&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJSpdA4S7AHDrfcxBW61GSoJJ7HwXlE8njWQtt0Aejnqg&oe=63230A5E)

Đây là những danh sách id người bán có bán lượt rating 1 và 2 điểm, đối với trường hợp này thì công ty sẽ khuyên các id người bán này bằng những cách sau :
    
   _Đích thân cái người bán phải xin lỗi khách hàng đó là lẽ tất yếu vì những sản phẩm của mình không được tốt và chưa làm hài lòng khách hàng nên khi shop hoặc id người bán đó đích thân phản hồi xin lỗi thì khách hàng sẽ thấy được thiện chí của id người và có thể sẽ thông cảm được.
   _ Có thể tặng hoặc đền bù cho khách một sản phẩm khác đẻ họ dễ dàng chấp nhận hơn.
   _ Đổi trả sản phẩm cho khách, thậm chí là hoàn tiền.
 
Và công ty nên nói với nững id khách hàng đó khi mà bị đánh giá 1 và 2 sao thì số điểm thấp đó bị đẩy lên và sẽ làm hang mang cho những khách khác khi thấy lượt điểm thấp như vậy, họ sẽ mất niềm tin vào sản phẩm,có sự nghi ngờ về chất lượng và không đưa ra quyết định mua hàng sẽ làm cho doanh số đi xuống đơn hàng sẽ không còn được bán nhiều nữa, và đặc biệt là họ phải tự cải thiện sản phẩm của mình.

###


