# I. Bối Cảnh
Bạn là một Data Analyst làm việc cho một công ty thương mại điện tử tên là X. Bạn được giao nhiệm vụ chuẩn bị một bài thuyết trình để trình bày tổng quan tình hình kinh doanh và vận hành của công ty tính đến thời điểm hiện tại cho Giám đốc bán hàng và Giám đốc vận hành. Bài thuyết trình tối thiểu phải bao gồm các thông tin sau: tổng quan tình hình kinh doanh, mức độ hài lòng của khách hàng, và đề xuất 2 đến 3 lĩnh vực (areas) mà công ty có thể cải thiện. 
# II. Phân tích
# Cây Logic Tree

![Cây Logic Tree](https://scontent.fsgn2-5.fna.fbcdn.net/v/t1.15752-9/291749804_1355260704879787_3177451409374303776_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ae9488&_nc_ohc=Q7ssUVlMDasAX_NyI2s&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn2-5.fna&oh=03_AVL2q-4FavLaULgec9N_5pA_aIxpIoc1PZnrU5Y3684exg&oe=633ACD03)

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

![tên ảnh](https://scontent.fsgn2-6.fna.fbcdn.net/v/t1.15752-9/298381806_1553753568375159_491650581988909281_n.png?_nc_cat=111&ccb=1-7&_nc_sid=ae9488&_nc_ohc=PiNp49RhfR8AX8XSUL_&_nc_ht=scontent.fsgn2-6.fna&oh=03_AVJBN4CVGMVh_lTtDcWTmdl2VlHvwUIFDngBnxJzyuEZbg&oe=6328913A)

 Tình hình kinh doanh của công ty qua các năm có sự tăng trưởng mạnh mẽ, nguồn doanh thu tăng nhanh qua các năm, năm 2016 doanh thu chỉ vỏn vẹn là 6.470$, năm 2017 doanh thu tăng lên 8.696.300$ (doanh thu năm 2017 tăng trưởng 134% so với năm 2016), và đến 9/2018 doanh thu là 11.819.953$( doanh thu 9/2018 tăng trưởng 36% so với năm 2017, và có mức tăng trưởng 182% so với năm 2016), doanh thu trong 3 năm qua đã đã có sự tăng trưởng đột biến, mặc dù chỉ tới quý III năm 2018 doanh thu đã tăng 182%, rất nhiều so với với 2 năm trước. Vì vậy thấy được công ty đã phát triển rất là mạnh mẽ, số dơn hàng bán được trong các năm qua đã tăng rất nhiều mang lại rất nhiều doanh thu cho công ty.
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

- Doanh thu từ tháng 1 cho tới tháng 8 nhìn chung có sự tăng trưởng đồng đều, và doanh thu tháng 8 có sự tăng cao rõ rệt so với những tháng trước đó, tuy nhiên đây mới là 9/2018 chưa đủ dữ liệu để đánh giá các tháng sau tháng 9/2018,nhưng có thể thấy là doanh thu các tháng có sự tăng đều chứng tỏ công ty có sự phát triển tốt.

### 2. Top nhóm sản phẩm (product_category_name) mang lại doanh thu nhiều nhất cho công ty
```php
df.groupby('product_category_name')['payment_value'].sum().nlargest(10).reset_index()
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/298973734_404916611537800_103573572929499479_n.png?_nc_cat=107&ccb=1-7&_nc_sid=ae9488&_nc_ohc=0PbBLbF8Uy0AX8wdI8a&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVKmUaCQa1_x9UBLkGOoyLhI-9sLpok612eoG8gSa11dog&oe=6322F1F0)

 Những nhóm sản phẩm ở trên bảng, đó là nhóm sản phẩm bán chạy nhất và đem lại doanh thu cao nhất cho công ty. Đó là những sản phẩm thế mạnh của cửa hàng công ty vì doanh thu của công ty chủ yếu là do những nhóm sản phẩm đó đem lại. Vì vậy chúng ta cần phải tập trung nhiều nguồn lực để mà phát triển những sản phẩm thế mạnh của công ty mình lên nữa. Có thể phát triển bằng những ý sau đây:

- Sau khi đã biết những nhom sản phẩm thế mạnh bán chạy của công ty thì có thể:
  
  -phát triển hệ sinh thái của sản phẩm. Cụ thể hơn, tập trung xây dựng các sản phẩm hỗ trợ/sản phẩm phụ trợ/phụ kiện cho các sản phẩm top này.
  
  -Cung cấp voucher cho các sản phẩm phụ trợ (hệ sinh thái) khi mua các sản phẩm top, để kích cầu họ có thể mua thêm các sản phẩm phụ kiện đi theo.

### 3. Số đơn hàng giao thành công và bị hủy

![](https://scontent.fsgn2-3.fna.fbcdn.net/v/t1.15752-9/299864453_477595030880528_6114840701050237731_n.png?_nc_cat=108&ccb=1-7&_nc_sid=ae9488&_nc_ohc=FWqaG774RV0AX-0s3aN&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn2-3.fna&oh=03_AVJ-JUNnf7EQfnq5SV7o4PV9y6VTSBZlFbsl69bc0Uk-eA&oe=632E343E)

-Nhìn vào bảng số liệu, số đơn hàng bị hủy là 750 đơn chiếm 0.7% trong tổng số các đơn hàng giao thành công,chúng ta cần phải tìm ra nguyên nhân số đơn hàng bị hủy vì lý do gì và từ đó cần phải khắc phục để cho số đơn hàng bị hủy càng ít lại. Đây là những id khách hàng hủy đơn hàng nhiều nhất:

![](https://scontent.fsgn2-5.fna.fbcdn.net/v/t1.15752-9/293521629_635547081131475_7962482481532222686_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ae9488&_nc_ohc=3ZQGDPTNBOEAX_aMGJP&_nc_ht=scontent.fsgn2-5.fna&oh=03_AVKX-_2anpzI1XNP74oqonkP1riMMvp4e8-A_pYMQeqHNw&oe=63295FEA)

   -Đây là những id khách hàng có số lần hủy đơn hàng nhiều nhất cần tìm hiểu nguyên nhân hủy đơn hàng từ họ vì lý do gì, và chúng ta cũng nên cảnh cáo họ đẻ cho họ giảm tỉ lệ hủy đơn hàng xuống bằng cách là những voucher khuyến mãi đối với những người này sẽ bị hạn chế và họ khó có thể nhận được những voucher giảm giá nhiều, hoặc là  thời gian vận chuyển của sẽ bị lâu hơn dự kiến, và  tài khoản đặt hàng sẽ bị hạn chế ở một số mặt hàng mà hj hủy nhiều...

-Biểu đồ thứ hai thì số đơn hàng giao được cho khách hàng tăng lên qua các năm và dến thời điểm hiện tại 2018 con số đã đạt tới 6000 đơn hàng, số đơn hàng bán được cũng rất là nhiều trong thời gian qua.

### 4. Hình thức thanh toán
```php
plt.figure(figsize=(12,6))
sns.countplot(data=df, x='payment_type');
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/292601378_3248779772029624_5936993273079975103_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ae9488&_nc_ohc=d4y2rthoeHUAX9Rw4OW&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIl9ubFshMyp2taBl_TY93485mlk31i7YxSc280iFFayA&oe=63212A13)

-Dựa vào biểu đò thì hình thức thanh toán mà khách hàng dùng để thanh toán nhiều nhất là credit_card, Vì mọi người thường có thói quen mua sắm online bằng thẻ tín dụng.

=>Mục tiêu là hạn chế tối đa các giao dịch bằng tiền mặt. Ưu tiên thanh toán qua thẻ credit card/debit card để giảm thiểu tỉ lệ khách không nhận hàng.

=>Tạo các chương trình ưu đãi áp dụng cho hình thức thanh toán bằng thẻ tín dụng/ghi nợ.


### 5. Tiêu đề đánh giá của khách hàng
```php
df.groupby('review_comment_title')['customer_id'].count().sort_values(ascending= False).nlargest(5).to_frame()
```
Output:

![](https://scontent.fsgn2-5.fna.fbcdn.net/v/t1.15752-9/299093127_825659951758048_1192841136745448186_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ae9488&_nc_ohc=h1ZHpg9F07wAX_fkAX6&_nc_ht=scontent.fsgn2-5.fna&oh=03_AVLDBYwUizxRgd19ty9t913ADKTzhZWcudU2gqO32eiH7Q&oe=632A189B)

Dịch nghĩa các từ trong bảng:
Recomendo: khuyên, đề nghị
Bom: Tốt
super recomendo: rất khuyên bạn nên
Excelente : Tuyệt
Muito bom : rất tốt

- Nhìn vào bảng số liệu thì recomendo và super recomendo được khách hàng đánh giá khá là nhiều, điều đó cho thấy sản phẩm của seller_id(id người bán) chưa thật sự tốt
 được khách hàng khuyên và đề nghị sửa đối, những người bán hàng nên tiếp cận và chăm sóc họ tìm ra được nguyên nhân của những sản phẩm không tốt ở đâu từ dó tiếp thu và cải thiện lại sản phẩm để cho đúng với nhu cầu của người mua, từ đó mới có thể tăng doanh thu bán hàng lên cao được.

- Đối với những sản phẩm được khách hàng đánh giá tốt thì nên tiếp tục giữ vững chất lượng sản phẩm đó và tiếp tục phát triển chất lượng cao hơn nữa để kiếm được doanh thu nhiều hơn nữa

### 6. Mức Hài Lòng của khách hàng.
```php
star= df.groupby('review_score')['customer_id'].count().reset_index(name='count')
ax= star['review_score']
bx= star['count']
plt.figure(figsize=(15,6))
plt.pie(bx, labels=ax ,autopct='%1.1f%%');
```
Output:

![](https://scontent.fsgn2-3.fna.fbcdn.net/v/t1.15752-9/300778288_3240178422907033_6041592468189111535_n.png?_nc_cat=108&ccb=1-7&_nc_sid=ae9488&_nc_ohc=mIbyPsVTOcQAX-EjJSZ&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn2-3.fna&oh=03_AVL5_kDueRoXK2LVIZINilVEjLDK-ZFVfd2A9v1EuZP4Ug&oe=632C8967)

-Đối với những nhóm sản phẩm bị đánh giá 1 và 2 điểm thì chúng ta cần phải xem xét lại , cần tìm hiểu nguyên nhân, và xem xét xem các đánh giá 1,2 đến từ nhóm sản phẩm nào/id người bán nào, từ đó cải thiện lại chất lượng sản phẩm cho đúng nhu cầu khách hàng.

### 7. Danh sách những nhóm sản phẩm bị đánh giá 1 và 2 điểm
```php
star12= df.loc[(df['review_score']== 1)|(df['review_score']==2)]
pd.crosstab(star12.product_category_name, star12.review_score).sort_values(by=1, ascending=False)
```
Output:

![](https://scontent-hkg4-2.xx.fbcdn.net/v/t1.15752-9/293984363_1753700474989259_7733036433922561168_n.png?_nc_cat=109&ccb=1-7&_nc_sid=ae9488&_nc_ohc=FoPJlOkH5okAX9k7YT8&_nc_ht=scontent-hkg4-2.xx&oh=03_AVINXymKRs8eXD0zkc7RmZPN3F-z78SYS-q_E_MSjeh2Hw&oe=6330409D)

Đây là những danh sách nhóm sản phẩm bị đánh giá 1 và 2 điểm.

Bước 1: Chúng ta phải tìm hiểu nguyên nhân từ những id của người bán hàng(seller_id) tại sao khách hàng lại đánh giá 1 và 2 điểm.

Bước 2: Chúng ta cần phải thu tập những phản hồi của khách hàng, tìm hiểu nguyên nhân.

Bước 3: Phân loại nguyên nhân. Điểm đánh giá tệ là do sản phẩm tệ hay chất lượng dịch vụ tệ? Nếu là do sản phẩm, thì cần cân nhắc nâng cao chất lượng sản phẩm, hoặc loại sản phẩm khỏi danh mục bán hàng. Nếu là do chất lượng dịch vụ tệ, cần tìm hiểu và làm việc với nhà cung cấp. 

Bước 4: Nếu lí do đánh giá 1 2 điểm là chính đáng, cần có biện pháp đền bù cho khách hàng. Có thể đền bù bằng voucher qua email xin lỗi, hứa hẹn sẽ cải thiện chất lượng dịch vụ/sản phẩm.


### 8. Danh sách id người bán có lượt đánh giá 1 và 2 sao.
```php
star12.groupby('seller_id')['review_score'].count().sort_values(ascending=False).nlargest(10).reset_index()
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/290608499_777055483432470_4269814101361728412_n.png?_nc_cat=101&ccb=1-7&_nc_sid=ae9488&_nc_ohc=OUcv43ALdGoAX-q1jo1&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJSpdA4S7AHDrfcxBW61GSoJJ7HwXlE8njWQtt0Aejnqg&oe=63230A5E)

Đây là những danh sách id người bán có bán lượt rating 1 và 2 điểm cao nhất. Đối với trường hợp này thì công ty cần
Bước 1: Phân loại các nguyên nhân từ rating 1 và 2 điểm. 
Bước 2: Đề nghị seller giải trình về việc bị rating 1, 2 điểm. Seller cần trình bày hướng khắc phục hậu quả, cải thiện sản phẩm/dịch vụ và cam kết thực hiện. 
Bước 3: Đền bù cho các khách hàng có trải nghiệm không tốt. (voucher, hoàn tiền, đổi trả sản phẩm,…)
Bước 4: Đánh giá năng lực của seller. Nếu lượt rating  1, 2 điểm quá nhiều, seller không cải thiện sản phẩm, dịch vụ => cấm id seller. 

 
Và nên cảnh báo id seller, khi bị đánh giá 1 và 2 sao thì số điểm thấp đó bị đẩy lên và sẽ làm hang mang cho những khách khác khi thấy lượt điểm thấp như vậy, họ sẽ mất niềm tin vào sản phẩm,có sự nghi ngờ về chất lượng và không đưa ra quyết định mua hàng sẽ làm cho doanh số đi xuống đơn hàng sẽ không còn được bán nhiều nữa, và đặc biệt là họ phải tự cải thiện sản phẩm của mình.

### 9. Những khách hàng có số lần đặt hàng nhiều nhất.
```php
df.groupby('customer_id')['order_id'].count().sort_values(ascending=False).head(10).to_frame()
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/293642595_562114019042723_5262240882150269008_n.png?_nc_cat=105&ccb=1-7&_nc_sid=ae9488&_nc_ohc=fdgqvoFyGowAX-aRDNM&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVJbF71WgCqmo1owq5rU2pAGGOplXqp7bXDQ5pR8bWxCKw&oe=632274FE)
- Đây là danh sách những khách có số lần đặt hàng nhiều nhất ở công ty, họ là những người tin tưởng những sản phẩm ở công ty nên cần phải chú trọng chăm sóc họ thường xuyên để họ không rời bỏ việc mua sản phẩm bên công ty mình.

Thông thường, người tiêu dùng đều có xu hướng lựa chọn mua hàng theo số đông. Vì vậy, khi một doanh nghiệp làm hài lòng nhóm khách hàng hiện tại, những người này sẽ đóng vai trò làm trung gian giới thiệu sản phẩm/ dịch vụ với những người quen biết của mình. Một cách vô tình, những khách hàng trung thành đã trở thành những nhân viên bán hàng cho sản phẩm của doanh nghiệp họ tin tưởng. Điều này sẽ mang ý nghĩa sống còn với các doanh nghiệp kinh doanh thời đại 4.0, nơi sức mạnh của phương pháp marketing truyền miệng đang được thổi bùng cùng với sự phát triển của internet. Gần như chỉ cần chăm sóc tốt khách hàng hiện tại, sẽ lại có thêm khách hàng mới.
Các phương pháp chăm sóc có thể như:

-	Tặng voucher cho các khách hàng khi đạt mức chi tiêu theo từng mốc. 

-	Email hỏi thăm khi không thấy khách mua hàng trong thời gian dài. 


### 10. Danh sách những khách hàng đem lại doanh thu cao nhất cho công ty.
![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/298147197_807309750277390_8941411742428927528_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ae9488&_nc_ohc=G0CQOvz2UX8AX8NefPQ&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIBWnB6aM6XoZggImepUoEYL5sxc-glYpMfWwFFiM7IqA&oe=6321AAEE)

-Đây là top 10 khách hàng đem lại chủ yếu doanh thu cho công ty, chúng ta cần chú ý và chăm sóc đặc biệt những khách hàng này nhiều hơn để họ không rời bỏ dịch vụ của mình, và cần tạo ra những cái voucher khuyến mãi tối ửu để có khuyến khích họ mua hàng của mình, vì họ là nguồn thu nhập chính của công ty nếu không chăm sóc tốt họ sẽ rời bỏ khi đó doanh thu công ty sẽ đi xuống. Tuy khách đặt hàng ở công ty nhiều nhưng dựa vào biểu đồ pareto thì thấy đucợ chỉ có 20% khách hàng đó đem lại chủ yêu 80% lợi nhuận của công ty nên vì thế cần phải chăm sóc đặt biệt những vị khách hàng này.

### 11. Phần trăm khách hàng trả hóp và thanh toán một lần.
![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/299424191_1820247374984406_4949406095906604947_n.png?_nc_cat=106&ccb=1-7&_nc_sid=ae9488&_nc_ohc=TcrlZJyMFFcAX9JhOfq&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVIZwmCB3_iOnUcxZnOg4BzEKYtRsXsstJfsbg4b5y7mEA&oe=63242986)

-Số lần thanh toán một lần và thanh toán trả góp gần như tương đương nhau, nhưng đối với khách hàng trả góp chúng ta cần phải chú ý và cần nhắc nhở họ để học trả nợ và lãi đúng hạn, để tránh trường hợp làm phiền cả đôi bên mình phải gị điện và nhắn tin với họ và hpj sẽ cụng bị phiền bởi bởi những tin nhắn của mình và lãi của họ sẽ bị gia tăng theem nếu không trả đúng hạn.

### 12. Những thành phố đem lại doanh thu cao nhất cho công ty
```php
df.groupby('customer_city')['payment_value'].sum().sort_values(ascending=False).to_frame().head(10)
```
Output:

![](https://scontent.fsgn15-1.fna.fbcdn.net/v/t1.15752-9/295633015_837910750961643_107388182019235943_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ae9488&_nc_ohc=zRIOspSnbOoAX85A472&tn=-Fc4noKWOTfEC8FP&_nc_ht=scontent.fsgn15-1.fna&oh=03_AVI8pdw7nFgYMxEL_FskAcPJjMfum3uGjRjTq2d1b9iAuA&oe=63231ACD)

- Doanh thu của công ty chủ yếu tập trung ở những thành phố này, chứng tỏ khách hàng chủ yếu tập trung ở những thành phố này để mà tối ưu hóa thì chứng ta nên đặt những kho hàng để mà việc trung chuyển có thể được tiết kiệm nhiều hơn và khách hàng khi dặt hàng có thể nhận hàng nhanh chóng hơn và họ cũng đỡ việc phí hip nhận hàng của mình.


# III. Kết luận
- Tình hình kinh doanh của công ty trong 3 năm trở lại đây có bước phát triển mãnh mẽ khi mà doanh thu tăng lên rất nhiều qua mỗi năm, số đơn hàng bán được cũng tăng lên đáng kể, nhưng doanh thu theo mỗi tháng có sự không đông đều khi mà những tháng đầu năm thì bán được rất là nhiều đơn hàng nhưng ngược lại những tháng ở cuối năm thì doanh thu lại bị giảm hơn chúng ta cần xem xét lại để cải thiện điều này. Đối với những nhóm sản phẩm đem lại doanh thu cao cho công ty thì cần tập trung nguồn lực để phát triển mạnh mẽ hơn nữa vì nó là thế mạnh của mình, còn với những khách hàng trả góp thì các nhân viên cần chú ý và nhắc nhở họ đóng nợ và tiền lãi đúng hạn, ngoài ra việc chăm sóc những khách hàng đem lại doanh thu cao cho công ty cũng rất quan trọng vì họ là nguồn sống của công ty nên vì thế cần phải có đội ngũ chăm sóc khách hàng dặc biệt đối với những người này.

Các lĩnh vực mà công ty có thể cải thiện:

  - Thứ nhất là về việc các sản phẩm và id người bán có lượt rating thấp thì đây là việc quan trọng đầu tiên công ty cần cải thiện, công ty cần nhắc nhở về việc id các người bán phải cải thiện đối với những món hàng bị rating thấp, vì khách hàng hay có hiện tượng theo số đông khi sản phẩm rating thấp như vậy thì sẽ làm cho những khách hàng phía sau thấy sẽ e ngại về việc đưa ra quyết định mua hàng, có thể họ sẽ bỏ dịch vụ sản phẩm bên mình để mà chọn một dịch vụ mua hàng bên công ty khác. Để mà cải thiện đucợ việc này thì cần phải thu thập và lắng tất cả ý kiến khách hàng về sản phẩm từ đó sẽ cải thiện lại sản phẩm cho nó phù hợp với nhu cầu thị trường và nhà ngu cầu của khách hàng.
  
    -Thứ hai là về việc chăm sóc khách hàng đem lại lợi nhuận cao cho công ty, vì họ là một phần sống cảu công ty đóng góp doanh thu cho công ty rất là cao để mà giữ chân được họ thì cần phải có đội ngũ chăm sóc khách hàng dặc biệt đối những vị khách này, cần phải thường xuyên gửi mail hoặc gọi điện thăm dò ý kiến của họ về sản phẩm của mình, ngoài ra chúng ta có thể tạo ra những cái voucher đặc biệt dựa trên những khách hàng này để mà giữ chân họ và kích cầu họ mua sản phẩm bên mình, như đã nói ở trên khách hàng thường hay theo hiện tượng số đông .Vì vậy, khi một doanh nghiệp làm hài lòng nhóm khách hàng hiện tại, những người này sẽ đóng vai trò làm trung gian giới thiệu sản phẩm/ dịch vụ với những người quen biết của mình. Một cách vô tình, những khách hàng trung thành đã trở thành những nhân viên bán hàng cho sản phẩm của doanh nghiệp họ tin tưởng.  
    
  -Thứ ba là về việc những khách hàng trả góp, công ty cần chú ý đối với nhóm khách hàng trả góp này cần thông báo bên bộ phận chăm sóc khách hàng để thường xuyên chú ý và xem họ đã đóng nợ và tiền lãi đúng hạn hay chưa từ đó mà nhắc nhở họ, để khắc phục tình trạng này thì chúng ta có thể tạo ra những cái khuyến mãi đối với những khách hàng thanh toán một lần để có thể kích cầu họ mua hàng và trả một lần luôn.
  
  
[nhấp vào link để xem dashboard](https://app.powerbi.com/groups/7ddb72d0-2e02-4a0e-b5c4-8d1493524633/reports/9cd9c119-a222-4fe1-93c3-58cc08bf4a82/ReportSection)
   
   




