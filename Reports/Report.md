# REPORT

**HYPOTHESIS**
* Các video về lĩnh vực y tế được quan tâm nhiều hơn trong thời điểm Covid bùng phát.

**METHODOLOGY**

* Cách thực hiện:

1.   Cào dữ liệu:
 - Với giả thuyết được đặt ra thì cần một lượng thông tin về tin tức có thể hiện rõ lượt tương tác và phải cùng trên một nguồn đăng trên một nền tảng nhất định. Vì thế, tụi em chọn cào dữ liệu từ kênh youtube của Báo Thanh Niên - kênh tin tức chính thống với tần suất đăng bài, lượt tương tác ổn định.
  - Tụi em cào dữ liệu từ các danh sách phát với nhiều video ở nhiều lĩnh vực và thời điểm khác nhau. Đồng thời, nhờ đó tụi em có thể kiểm soát được số lượng dữ liệu cào về.
  - Tụi em sẽ cào những thuộc tính của video như: Tiêu đề (để xác định lĩnh vực của video), lượt xem và lượt thích (thể hiện độ tương tác của người dùng với video), ngày đăng (xác định thời điểm của video)
  - Sau khi thực hiện các bước trên thì có được dataset ban đầu: video_data.csv

2.  Chia lĩnh vực cho các video
 - Dùng LLM để phân loại các video có liên quan lĩnh vực y tế dựa trên tiêu đề. Sau đó gắn nhãn dữ liệu bằng cách thêm cột medical_field (1: lĩnh vực y tế, 0: các lĩnh vực khác). Dataset thu được: final_data.csv - dataset mà tụi em sẽ sử dụng để kiểm định giả thuyết.
3. Tiền xử lý
 - Kiểm tra và xử lý missing values, dữ liệu bị lỗi và trùng trong quá trình cào
 - Chuyển các dữ liệu về đúng định dạng
 - Xác định thời điểm bị can thiệp và gắn nhãn (0: ngoài thời điểm, 1: trong thời điểm)
 - Xử lý outliers: + Vì lượt xem outlier sẽ ảnh hưởng lên lượt xem trung bình nên tụi em sẽ xử lý outliers đưa về min/max (xử lý theo từng giai đoạn) để kết quả được khách quan nhất.
4. Thực hiện kiểm định
   - Thống kê để quan sát sự thay đổi về số lượng các video về lĩnh vực y tế trên tổng thể
     
     + Tính tỉ lệ số lượng video về lĩnh vực y tế trong các giai đoạn khảo sát
     + Tính hệ số DiD để ước lượng tác động nhân quả của Covid-19 lên số lượng video về lĩnh vực y tế
  - Sử dụng Difference-In-Differences để kiểm định dựa trên lượt xem trung bình
     + Tính lượt xem trung bình của các video về lĩnh vực y tế trong các giai đoạn khảo sát
     + Tính hệ số DiD để ước lượng tác động nhân quả của Covid-19 lên lượt xem
      của các video về lĩnh vực y tế
     + Tính hệ số ước lượng của các tác nhân (lĩnh vực y tế, thời điểm Covid, yếu tố kết hợp - did) lên lượt xem
     + Sử dụng OLS để thống kê số liệu của các kết quả hồi quy từ dataset để có kết luận về giả thuyết đã đặt ra
5. Trực quan hóa dữ liệu
    + Trực quan để thấy rõ được sự thay đổi do tác động của Covid-19 lên số lượng video và lượt xem trung bình của các video về lĩnh vực y tế

**DISCUSSION**
Bộ dữ liệu:
- Kích thước khá lớn
- Dữ liệu không có nhiều missing values tuy nhiên định dạng của các thuộc tính không đồng nhất nên cần hiểu dữ liệu để chuyển đổi phù hợp
- Các thuộc tính chưa được tận dụng hết (không dùng thuộc tính 'likes')

Kết quả thống kê:
- Vì các video được lấy trong các danh sách phát được tạo ngẫu nhiên nên số lượng video trong từng thời điểm không đồng đều => Kết quả thống kê thu được chưa thực sự phản ánh đúng

**EXPLANATION**
1.  *Cào dữ liệu từ danh sách phát:*
- Vì youtube là web động và sử dụng thanh cuộn vô cực nên để cuộn và cào dữ liệu từ 3-4 năm trước thì sẽ phải yêu cầu bộ nhớ RAM rất lớn để lưu lại lượng thông tin khổng lồ trong hàng chục ngàn video, đồng thời số lượng video đó cũng quá lớn để xử lý
- Để crawl được ngày đăng cụ thể và lượt xem thì cần phải click 2 lần (click vào video và phần mô tả) và việc lặp lại nhiều lần như vậy thì bị Youtube chặn bằng cơ chế phát hiện bot
- Hơn nữa, nếu crawl dữ liệu từ đầu trang mà đường truyền bị gián đoạn thì phải tốn thời gian cuộn lại để tìm đến video bị gián đoạn. Trong khi đó, nếu cào bằng danh sách phát thì tụi em sẽ có được index cụ thể của video trong danh sách phát và dễ dàng tiếp tục cào.
2.   *Dùng LLM:*
- Do có quá nhiều dữ liệu nếu gắn nhãn bằng tay thì rất mất thời gian. Hơn nữa, lĩnh vực y tế cũng là lĩnh vực phổ biến nên dễ để phân loại.
3. *Không sử dụng thuộc tính "likes":*
- Mặc dù số lượt thích cũng phản ánh được mức độ tương tác, tuy nhiên trong bộ dữ liệu này số lượt thích là không đáng kể so với số lượt xem nên để bài toán kiểm định được chính xác hơn tụi em không sử dụng thuộc tính này.
4. *Xác định giai đoạn dịch Covid-19:*
- Bắt đầu: 30/1/2020 - Thời điểm phát hiện những ca nhiễm đầu tiên tại Việt Nam
- Kết thúc: 30/3/2022. Mặc dù, 7/7/2023 Bộ Y tế mới công bố hết dịch tại Việt Nam, nhưng tụi em chọn ngày kết thúc như trên vì đó là thời điểm mà xã hội đã được vận hành lại như trước khi có dịch và cũng như đã có vắc-xin kiểm soát.
5. *Chọn mô hình Diff-in-Diff:*
- Sythetic Control phù hợp với giả thuyết trên một nhóm video cụ thể và yêu cầu số lượng đơn vị không bị can thiệp (số video không trong giai đoạn Covid) đủ lớn
- Trong khi đó, Diff-in-Diff phù hợp với bộ dữ liệu có nhiều đơn vị quan sát và thời gian, cũng như đơn giản hơn.

**REFLECTION**
* Điểm mạnh:
  1. Crawl được thông tin từ web động và có thanh cuộn vô cực
  2. Crawl được thông tin từ nhiều lĩnh vực trải dài theo thời gian
  3. Đưa ra được kết luận với giả thuyết ban đầu
  4. Thực hiện đầy đủ các yêu cầu của project
* Điểm yếu:
  1. Do danh sách phát được tạo ngẫu nhiên nên có những khoảng thời gian số lượng video ít ảnh hưởng đến độ chính xác của bài toán kiểm định
  2. Chưa tận dụng được hết dữ liệu đã crawl
  3. Chưa tận dụng được tối đa chức năng của LLM

**LESSON LEARNED**
1. Biết được cách crawl dữ liệu và tầm quan trọng của việc lựa chọn web để crawl
2. Từ giả thuyết, xác định được những thông tin cần crawl
3. Hiểu được tập dữ liệu và giả thuyết để lựa chọn mô hình kiểm định phù hợp

**FINDINGS**

* Bảng các số liệu thống kê:

                            OLS Regression Results                            
==============================================================================
Dep. Variable:                  views   R-squared:                       0.066
Model:                            OLS   Adj. R-squared:                  0.066
Method:                 Least Squares   F-statistic:                     341.8
Date:                Fri, 21 Jun 2024   Prob (F-statistic):          1.86e-214
Time:                        09:06:38   Log-Likelihood:            -1.8709e+05
No. Observations:               14482   AIC:                         3.742e+05
Df Residuals:                   14478   BIC:                         3.742e+05
Df Model:                           3                                         
Covariance Type:            nonrobust                                         
=================================================================================
                    coef    std err          t      P>|t|      [0.025      0.975]
---------------------------------------------------------------------------------
Intercept      4.851e+04   1589.692     30.515      0.000    4.54e+04    5.16e+04
medical_field -1.489e+04   4943.320     -3.012      0.003   -2.46e+04   -5197.276
time_in_covid  5.672e+04   2055.426     27.594      0.000    5.27e+04    6.07e+04
did            1.245e+04   5322.773      2.338      0.019    2012.286    2.29e+04
==============================================================================
Omnibus:                     2659.137   Durbin-Watson:                   1.788
Prob(Omnibus):                  0.000   Jarque-Bera (JB):             4361.743
Skew:                           1.299   Prob(JB):                         0.00
Kurtosis:                       3.690   Cond. No.                         12.0
==============================================================================

Đây là kết quả của mô hình hồi quy OLS

* Intercept (hệ số chặn):
  
  + Hệ số : 4.851e+04
  
   Nghĩa là khi các biến độc lập khác đều bằng 0 thì lượt xem trung bình là 48510.

  + P-value: 0.000 - rất nhỏ => Hệ số chặn có ý nghĩa thống kê
* Medical_field:
  + Hệ số: -1.489e+04
   
   Cho thấy rằng, ngoài thời điểm Covid, các video về lĩnh vực y tế có số lượt xem trung bình ít hơn 14890 lượt so với các video không thuộc lĩnh vực y tế.
  + P-value: 0.003 - rất nhỏ < 0.05 => Cho thấy medical_field có tác động lên lượt xem.
* Time_in_covid:
  + Hệ số: 5.672e+04

  Cho thấy rằng, rằng trong thời điểm Covid,số lượt xem trung bình của tất cả các video tăng lên 56720 lượt.
  + P-value: 0.000 - rất nhỏ => Cho thấy Covid-19 có tác động đáng kể lên lượt xem
* did:
  + Hệ số: 1.245e+04
  Điều này chỉ ra rằng trong thời điểm Covid-19, số lượt xem trung bình của các video về lĩnh vực y tế tăng thêm 12450 lượt so với các video không thuộc lĩnh vực y tế.
  + P-value: 0.019 < 0.05
  Với độ tin cậy 95%, ta có thể chấp nhận giả thuyết "Các video về lĩnh vực y tế được quan tâm nhiều hơn trong thời điểm Covid bùng phát".

**CONCLUSION**
Kết quả cho thấy rằng trong thời điểm Covid-19, tất cả các video đều có số lượt xem tăng đáng kể, trong đó các video về lĩnh vực y tế được quan tâm nhiều hơn. Nghĩa là giả thuyết ban đầu đã được chấp nhận sau khi kiểm định với bộ dữ liệu trên.
