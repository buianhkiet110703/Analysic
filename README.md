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

-Đây là những danh sách nhóm sản phẩm bị đánh giá 1 và 2 điểm chúng ta phải tìm hiểu nguyên nhân từ những id của người bán hàng(seller_id) tại sao khách hàng lại đánh giá 1 và 2 điểm có số điểm thấp đến như vậy, chúng ta cần phải thu tập những  phản hồi của khách hàng, sau khi tìm hiểu nguyên nhân và khắc phục nếu mà những id người bán hàng này bán sản phẩm mà khách hàng vẫn còn đánh giá 1 và 2 điểm nhiều nữa thì chúng ta cần phải xem xét và xử lý việc này , tại vì khi mà khách hàng muốn mua một sản phẩm nào đó mà họ nhìn thấy những lượt chấm của những khách hàng thấp đến như vậy thì họ sẽ e ngại và k còn mua nữa, từ đó lượng khách hàng dùng cái dịch vụ thương mại điện tử của mình để mua hàng sẽ bị giảm xuống, chính vì thế mà chúng ta cần phải khắc phục việc người bán hàng giao đến những sản phẩm hư hỏng hoặc những sản phẩm kém chất lượng cho khách hàng. Do vậy, qua những phản hồi từ khách hàng, doanh nghiệp sẽ có cơ hội để hoàn thiện và làm thỏa mãn thị trường với những sản phẩm có giá trị hơn.

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
 
Và công ty nên nói với nững id người bán đó khi mà bị đánh giá 1 và 2 sao thì số điểm thấp đó bị đẩy lên và sẽ làm hang mang cho những khách khác khi thấy lượt điểm thấp như vậy, họ sẽ mất niềm tin vào sản phẩm,có sự nghi ngờ về chất lượng và không đưa ra quyết định mua hàng sẽ làm cho doanh số đi xuống đơn hàng sẽ không còn được bán nhiều nữa, và đặc biệt là họ phải tự cải thiện sản phẩm của mình.

### 8. Những khách hàng có số lần đặt hàng nhiều nhất.
```php
df.groupby('customer_id')['order_id'].count().sort_values(ascending=False).head(10).to_frame()
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/293642595_562114019042723_5262240882150269008_n.png?_nc_cat=105&ccb=1-7&_nc_sid=ae9488&_nc_ohc=fdgqvoFyGowAX-aRDNM&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJbF71WgCqmo1owq5rU2pAGGOplXqp7bXDQ5pR8bWxCKw&oe=632274FE)
- Đây là danh sách những khách có số lần đặt hàng nhiều nhất ở công ty, họ là những người tin tưởng những sản phẩm ở công ty nên cần phải chú trọng chăm sóc họ thường xuyên để họ không rời bỏ việc mua sản phẩm bên công ty mình.

Thông thường, người tiêu dùng đều có xu hướng lựa chọn mua hàng theo số đông. Vì vậy, khi một doanh nghiệp làm hài lòng nhóm khách hàng hiện tại, những người này sẽ đóng vai trò làm trung gian giới thiệu sản phẩm/ dịch vụ với những người quen biết của mình. Một cách vô tình, những khách hàng trung thành đã trở thành những nhân viên bán hàng cho sản phẩm của doanh nghiệp họ tin tưởng. 
Điều này sẽ mang ý nghĩa sống còn với các doanh nghiệp kinh doanh thời đại 4.0, nơi sức mạnh của phương pháp marketing truyền miệng đang được thổi bùng cùng với sự phát triển của internet. Gần như chỉ cần chăm sóc tốt khách hàng hiện tại, bạn sẽ lại có thêm khách hàng mới.

### 9. Danh sách những khách hàng đem lại doanh thu cao nhất cho công ty.
![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/298147197_807309750277390_8941411742428927528_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ae9488&_nc_ohc=G0CQOvz2UX8AX8NefPQ&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIBWnB6aM6XoZggImepUoEYL5sxc-glYpMfWwFFiM7IqA&oe=6321AAEE)

-Đây là top 10 khách hàng đem lại chủ yếu doanh thu cho công ty, chúng ta cần chú ý và chăm sóc đặc biệt những khách hàng này nhiều hơn để họ không rời bỏ dịch vụ của mình, và cần tạo ra những cái voucher khuyến mãi tối ửu để có khuyến khích họ mua hàng của mình, vì họ là nguồn thu nhập chính của công ty nếu không chăm sóc tốt họ sẽ rời bỏ khi đó doanh thu công ty sẽ đi xuống. Tuy khách đặt hàng ở công ty nhiều nhưng dựa vào biểu đồ pareto thì thấy đucợ chỉ có 20% khách hàng đó đem lại chủ yêu 80% lợi nhuận của công ty nên vì thế cần phải chăm sóc đặt biệt những vị khách hàng này.

### 10. Phần trăm khách hàng trả hóp và thanh toán một lần.
![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/299424191_1820247374984406_4949406095906604947_n.png?_nc_cat=106&ccb=1-7&_nc_sid=ae9488&_nc_ohc=TcrlZJyMFFcAX9JhOfq&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIZwmCB3_iOnUcxZnOg4BzEKYtRsXsstJfsbg4b5y7mEA&oe=63242986)

-Số lần thanh toán một lần và thanh toán trả góp gần như tương đương nhau, nhưng đối với khách hàng trả góp chúng ta cần phải chú ý và cần nhắc nhở họ để học trả nợ và lãi đúng hạn, để tránh trường hợp làm phiền cả đôi bên mình phải gị điện và nhắn tin với họ và hpj sẽ cụng bị phiền bởi bởi những tin nhắn của mình và lãi của họ sẽ bị gia tăng theem nếu không trả đúng hạn.

### 11. Những thành phố đem lại doanh thu cao nhất cho công ty
```php
df.groupby('customer_city')['payment_value'].sum().sort_values(ascending=False).to_frame().head(10)
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/295633015_837910750961643_107388182019235943_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ae9488&_nc_ohc=zRIOspSnbOoAX85A472&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVI8pdw7nFgYMxEL_FskAcPJjMfum3uGjRjTq2d1b9iAuA&oe=63231ACD)

- Doanh thu của coong ty chủ yếu tập trung ở những thành phố này, chứng tỏ khách hàng chủ yếu tập trung ở những thành phố này đẻ mà tối ưu hóa thì chứng ta nên đặt những kho hàng để mà việc trung chuyển có thể được tiết kiệm nhiều hơn và khách hàng khi dặt hàng có thể nhận hàng nhanh chóng hơn và họ cũng đỡ việc phí hip nhận hàng của mình.

# III. Kết luận
- Tình hình kinh doanh của công ty trong 3 năm trở lại đây có bước phát triển mãnh mẽ khi mà doanh thu tăng lên rất nhiều qua mỗi năm, số đơn hàng bán được cũng tăng lên đáng kể, nhưng doanh thu theo mỗi tháng có sự không đông đều khi mà những tháng đầu năm thì bán được rất là nhiều đơn hàng nhưng ngược lại những tháng ở cuối năm thì doanh thu lại bị giảm hơn chúng ta cần xem xét lại để cải thiện điều này. Đối với những nhóm sản phẩm đem lại doanh thu cao cho công ty thì cần tập trung nguồn lực để phát triển mạnh mẽ hơn nữa vì nó là thế mạnh của mình, còn với những khách hàng trả góp thì các nhân viên cần chú ý và nhắc nhở họ đóng nợ và tiền lãi đúng hạn, ngoài ra việc chăm sóc những khách hàng đem lại doanh thu cao cho công ty cũng rất quan trọng vì họ là nguồn sống của công ty nên vì thế cần phải có đội ngũ chăm sóc khách hàng dặc biệt đối với những người này.

Các lĩnh vực mà công ty có thể cải thiện:

  - Thứ nhất là về việc các sản phẩm và id người bán có lượt rating thấp thì đây là việc quan trọng đầu tiên công ty cần cải thiện, công ty cần nhắc nhở về việc id các người bán phải cải thiện đối với những món hàng bị rating thấp, vì khách hàng hay có hiện tượng theo số đông khi sản phẩm rating thấp như vậy thì sẽ làm cho những khách hàng phía sau thấy sẽ e ngại về việc đưa ra quyết định mua hàng, có thể họ sẽ bỏ dịch vụ sản phẩm bên mình để mà chọn một dịch vụ mua hàng bên công ty khác. Để mà cải thiện đucợ việc này thì cần phải thu thập và lắng tất cả ý kiến khách hàng về sản phẩm từ đó sẽ cải thiện lại sản phẩm cho nó phù hợp với nhu cầu thị trường và nhà ngu cầu của khách hàng.
  
    -Thứ hai là về việc chăm sóc khách hàng đem lại lợi nhuận cao cho công ty, vì họ là một phần sống cảu công ty đóng góp doanh thu cho công ty rất là cao để mà giữ chân được họ thì cần phải có đội ngũ chăm sóc khách hàng dặc biệt đối những vị khách này, cần phải thường xuyên gửi mail hoặc gọi điện thăm dò ý kiến của họ về sản phẩm của mình, ngoài ra chúng ta có thể tạo ra những cái voucher đặc biệt dựa trên những khách hàng này để mà giữ chân họ và kích cầu họ mua sản phẩm bên mình, như đã nói ở trên khách hàng thường hay theo hiện tượng số đông .Vì vậy, khi một doanh nghiệp làm hài lòng nhóm khách hàng hiện tại, những người này sẽ đóng vai trò làm trung gian giới thiệu sản phẩm/ dịch vụ với những người quen biết của mình. Một cách vô tình, những khách hàng trung thành đã trở thành những nhân viên bán hàng cho sản phẩm của doanh nghiệp họ tin tưởng.  
    
  -Thứ ba là về việc những khách hàng trả góp, công ty cần chú ý đối với nhóm khách hàng trả góp này cần thông báo bên bộ phận chăm sóc khách hàng để thường xuyên chú ý và xem họ đã đóng nợ và tiền lãi đúng hạn hay chưa từ đó mà nhắc nhở họ, để khắc phục tình trạng này thì chúng ta có thể tạo ra những cái khuyến mãi đối với những khách hàng thanh toán một lần để có thể kích cầu họ mua hàng và trả một lần luôn.
  
  
[nhấp vào link](https://app.powerbi.com/groups/7ddb72d0-2e02-4a0e-b5c4-8d1493524633/reports/9cd9c119-a222-4fe1-93c3-58cc08bf4a82/ReportSection)
   
   




