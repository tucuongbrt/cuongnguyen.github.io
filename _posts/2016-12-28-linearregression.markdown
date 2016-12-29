---
layout: post
comments: true
title:  "Linear Regression"
date:   2016-12-28 15:22:00
mathjax: true
---

## Bài viết đang trong giai đoạn xây dựng, sẽ được hoàn thành sớm. 

<div class="imgcap">
<div >
<a href = "/2016/12/28/linearregression/">
    <img src ="/assets/LR/output_5_1.png"  width = "500"></a>
    <!-- <img src="/assets/rl/mdp.png" height="206"> -->
</div>
<div class="thecap"> Linear Regression <br> (Nguồn: <a href ="https://en.wikipedia.org/wiki/Linear_regression">Wikipedia</a>)</div>
</div>

Trong bài này, tôi sẽ giới thiều một trong những thuật toán cơ bản nhất (và đơn giản nhất) của Machine Learning. Đây là một thuật toán _Supervised learning_ có tên **Linear Regression** (Hồi Quy Tuyến Tính). Bài toán này đôi khi được gọi là _Linear Fitting_ hoặc _Linear Least Square_

Trong trang này:
<!-- MarkdownTOC -->

- [1. Giới thiệu](#-gioi-thieu)
- [2. Phân tích toán học](#-phan-tich-toan-hoc)
    - [Dạng của Linear Regression](#dang-cua-linear-regression)
    - [Sai số dự đoán](#sai-so-du-doan)
    - [Hàm mất mát](#ham-mat-mat)
    - [Nghiệm cho bài toán Linear Regression](#nghiem-cho-bai-toan-linear-regression)
- [3. Ví dụ trên Python](#-vi-du-tren-python)
- [4. Thảo luận](#-thao-luan)
    - [Output là một vector nhiều biến](#output-la-mot-vector-nhieu-bien)
    - [Mô hình là một đa thức bậc cao](#mo-hinh-la-mot-da-thuc-bac-cao)
    - [Hạn chế của Linear Regression](#han-che-cua-linear-regression)
    - [Các phương pháp tối ưu](#cac-phuong-phap-toi-uu)
- [5. Tài liệu tham khảo](#-tai-lieu-tham-khao)

<!-- /MarkdownTOC -->

<!-- ========================== New Heading ==================== -->
<a name="-gioi-thieu"></a>

## 1. Giới thiệu

Quay lại [ví dụ đơn giản được nêu trong bài trước](/2016/12/27/categories/): một căn nhà rộng \\(x_1 ~ \text{m}^2\\), có \\(x_2\\) phòng ngủ và cách trung tâm thành phố \\(x_3~ \text{km}\\) có giá là bao nhiêu. Giả sử chúng ta đã có số liệu thống kê từ 1000 căn nhà trong thành phố đó, liệu rằng khi có một căn nhà mới với các thông số về diện tích, số phòng ngủ và khoảng cách tới trung tâm, chúng ta có thể dự đoán được giá của căn phòng đó không? Nếu có thì hàm dự đoán \\(y = f(\mathbf{x}) \\) sẽ có dạng như thế nào. Ở đây \\(\mathbf{x} = [x_1; x_2; x_3] \\) là một vector
cột chứa thông tin _input_, \\(y\\) là một số vô hướng (scalar) biểu diễn _output_ (tức giá của căn nhà trong ví dụ này).

**Lưu ý về ký hiệu toán học:** _trong các bài viết của tôi, các số vô hướng được biểu diễn bởi các chữ cái viết ở dạng không in đậm, có thể viết hoa, ví dụ \\(x_1, N, y, k\\). Các vector được biểu diễn bằng các chữ cái thường in đậm, ví dụ \\(\mathbf{y}, \mathbf{x}_1 \\). Các ma trận được biểu diễn bởi các chữ viết hoa in đậm, ví dụ \\(\mathbf{X, Y, W} \\)._

Một cách đơn giản nhất, chúng ta có thể thấy rằng: i) diện tích nhà càng lớn thì giá nhà càng cao; ii) số lượng phòng ngủ càng lớn thì giá nhà càng cao; iii) càng xa trung tâm thì giá nhà càng giảm. Một hàm số đơn giản nhất có thể mô tả mối quan hệ giữa giá nhà và 3 đại lượng đầu vào là: 

\\[ \text{ giá nhà } \approx w_1 (\text{diện tích}) + w_2 (\text{số phòng}) + w_3 (\text{ khoảng cách}) + w_0 \\] 

trong đó, \\(w_1, w_2\\) là các hằng số dương, \\(w_3\\) là một hằng số âm, \\(w_0\\) là một hằng số được gọi là bias. Hay nói cách khác: 

\\[y \approx w_1 x_1 + w_2 x_2 + w_3 x_3 + w_0 = f(\mathbf{x}) = \hat{y}~~~~ (1)\\]

Mối quan hệ \\(y \approx f(\mathbf{x})\\) bên trên là một mối quan hệ tuyến tính (linear). Bài toán chúng ta đang làm là một bài toán thuộc loại regression. Bài toán đi tìm các hệ số tối ưu \\( \\{w_1, w_2, w_3, w_0 \\}\\) chính vì vậy được gọi là bài toán Linear Regression. 

**Chú ý 1:** \\(y\\) là giá trị thực của _outcome_ (dựa trên số liệu thống kê chúng ta có trong tập _training data_), trong khi \\(\hat{y}\\) là giá trị mà mô hình Linear Regression dự đoán được. Nhìn chung, \\(y\\) và \\(\hat{y}\\) là hai giá trị khác nhau do có sai số mô hình, tuy nhiên, chúng ta mong muốn rằng sự khác nhau này rất nhỏ.

**Chú ý 2:** _Linear_ hay _tuyến tính_ hiểu một cách đơn giản là _thẳng, phẳng_. Trong không gian hai chiều, một hàm số được gọi là _tuyến tính_ nếu đồ thị của nó có dạng một _đường thẳng_. Trong không gian ba chiều, một hàm số được goi là _tuyến tính_ nếu đồ thị của nó có dạng một _mặt phẳng_. Trong không gian nhiều hơn 3 chiều, khái niệm _mặt phẳng_ không còn phù hợp nữa, thay vào đó, một khái niệm khác ra đời được gọi là _siêu mặt phẳng_ (_hyperplane_). Các hàm số tuyến tính là các hàm đơn giản nhất, vì chúng thuận tiện trong việc hình dung và tính toán. Chúng ta sẽ được thấy trong các bài viết sau, _tuyến tính_ rất quan trọng và hữu ích trong các bài toán Machine Learning. Kinh nghiệm cá nhân tôi cho thấy, trước khi hiểu được các thuật toán _phi tuyến_ (non-linear, không phẳng), chúng ta cần nắm vững các kỹ thuật cho các mô hình _tuyến tính_.




<!-- ========================== New Heading ==================== -->
<a name="-phan-tich-toan-hoc"></a>

## 2. Phân tích toán học



<!-- ========================== New Heading ==================== -->
<a name="dang-cua-linear-regression"></a>

### Dạng của Linear Regression 

Trong phương trình \\((1)\\) phía trên, nếu chúng ta đặt \\(\mathbf{w} = [w_1; w_2; w_3, w_0] = \\) là vector hệ số cần phải tối ưu và \\(\mathbf{\bar{x}} = [x_1; x_2; x_3; 1]\\) (đọc là _x bar_ trong tiếng Anh) là vector dữ liệu đầu vào _mở rộng_. Số \\(1\\) ở cuối được thêm vào để thuận tiện cho việc tính toán. Khi đó, phương trình (1) có thể được viết lại dưới dạng:

\\[y \approx \mathbf{w}^T\mathbf{\bar{x}} = \hat{y}\\]




<!-- ========================== New Heading ==================== -->
<a name="sai-so-du-doan"></a>

### Sai số dự đoán 

Chúng ta mong muốn rằng sự sai khác \\(e\\) giữa giá trị thực \\(y\\) và giá trị dự đoán \\(\hat{y}\\) (đọc là _y hat_ trong tiếng Anh) là nhỏ nhất. Nói cách khác, chúng ta muốn giá trị sau đây càng nhỏ càng tốt: 

\\[
\frac{1}{2}e^2 = \frac{1}{2}(y - \hat{y})^2 = \frac{1}{2}(y - \mathbf{w}^T\mathbf{\bar{x}})^2
\\]

trong đó hệ số \\(\frac{1}{2} \\) là để thuận tiện cho việc tính toán (tính đạo hàm mà tôi sẽ trình bày ở phía dưới). Chúng ta cần \\(e^2\\) vì \\(e = y - \hat{y} \\) có thể là một số âm, việc nói \\(e\\) nhỏ nhất sẽ không đúng vì khi \\(e = - \infty\\) là rất nhỏ nhưng sự sai lệch là rất lớn. Bạn đọc có thể tự đặt câu hỏi: **tại sao không dùng trị tuyệt đối \\( \|e\| \\) mà lại dùng bình phương \\(e^2\\) ở đây?** Câu trả lời sẽ có ở phần sau. 





<!-- ========================== New Heading ==================== -->
<a name="ham-mat-mat"></a>

### Hàm mất mát

Điều tương tự xảy ra với tất cả các cặp _(input, outcome)_ \\( (\mathbf{x}_i, y_i), i = 1, 2, \dots, N \\), với \\(N\\) là số lượng dữ liệu quan sát được. Điều chúng ta muốn, tổng sai số là nhỏ nhất, tương đương với việc tìm \\( \mathbf{w} \\) để hàm số sau đạt giá trị nhỏ nhất:

\\[ \mathcal{L}(\mathbf{w}) = \frac{1}{2}\sum_{i=1}^N (y_i - \mathbf{w}^T\mathbf{\bar{x}}_i)^2 ~~~~~(2) \\] 

Trong đó \\(\mathbf{w}^T\\) là chuyển vị (transpose) của vector \\(\mathbf{w}\\).
Hàm số \\(\mathcal{L}(\mathbf{w}) \\) được gọi là __hàm mất mát__ (loss function) của bài toán Linear Regression. Chúng ta luôn mong muốn rằng sự mất mát càng nhỏ càng tốt, điều đó đồng nghĩa với việc giá trị của hàm mất mát này càng nhỏ càng tốt. Giá trị của \\(\mathbf{w}\\) làm cho hàm mất mát đạt giá trị nhỏ nhất được gọi là _điểm tối ưu_ (optimal point), ký hiệu:

\\[ \mathbf{w}^* = \arg\min_{\mathbf{w}} \mathcal{L}(\mathbf{w})  \\] 

Trước khi đi tìm lời giải, chúng ta tối giản phép toán trong phương trình hàm mất mát \\((2)\\). Đặt \\(\mathbf{y} = [y_1, y_2, \dots, y_N]\\) là một vector hàng chứa tất cả các _output_ của _training data_; \\( \mathbf{\bar{X}} = [\mathbf{\bar{x}}_1, \mathbf{\bar{x}}_2, \dots, \mathbf{\bar{x}}_N ] \\) là ma trận dữ liệu đầu vào (mở rộng). Khi đó hàm số mất mát \\(\mathcal{L}(\mathbf{w})\\) được viết dưới dạng ma trận đơn giản hơn: 

\\[
\mathcal{L}(\mathbf{w}) 
= \frac{1}{2}\sum_{i=1}^N (y_i - \mathbf{w}^T\mathbf{\bar{x}}_i)^2 
= \frac{1}{2} \\|\mathbf{y} - \mathbf{w}^T \mathbf{\bar{X}}\\|_2^2 
= \frac{1}{2} \\|\mathbf{y}^T - \mathbf{\bar{X}}^T\mathbf{w} \\|_2^2
~~~(3)
\\]

với \\( \\| \mathbf{z} \\|_2 \\) là Euclidean norm (chuẩn Euclid, hay khoảng cách Euclid) và \\( \\| \mathbf{z} \\|_2^2 \\) là tổng của bình phương mỗi phần tử của vector \\(\mathbf{z}\\). Tới đây, ta đã có một dạng đơn giản của hàm mất mát được viết như phương trình \\((3)\\).




<!-- ========================== New Heading ==================== -->
<a name="nghiem-cho-bai-toan-linear-regression"></a>

### Nghiệm cho bài toán Linear Regression

__Cách phổ biến nhất để tìm nghiệm cho một bài toán tối ưu (chúng ta đã biết từ khi học cấp 3) là giải phương trình đạo hàm (gradient) bằng 0!__ Tất nhiên đó là khi việc tính đạo hàm và việc giải phương trình đạo hàm bằng 0 không quá phức tạp. Thật may mắn, với các mô hình tuyến tính, hai việc này là khả thi. 

Đạo hàm theo \\(\mathbf{w} \\) của hàm mất mát là: 
\\[
\frac{\partial{\mathcal{L}(\mathbf{w})}}{\partial{\mathbf{w}}} 
= \mathbf{\bar{X}}(\mathbf{y}^T - \mathbf{\bar{X}}^T\mathbf{w}) 
\\]

Các bạn có thể tham khảo bảng đạo hàm theo vector hoặc ma trận của một hàm số trong [mục D.2 của tài liệu này](https://ccrma.stanford.edu/~dattorro/matrixcalc.pdf). _Đến đây tôi xin quay lại câu hỏi ở phần [Sai số dự đoán](#sai so du doan) phía trên về việc tại sao không dùng trị tuyệt đối mà lại dùng bình phương. Câu trả lời là hàm bình phương có đạo hàm tại mọi nơi, trong khi hàm trị tuyệt đối thì không (đạo hàm không xác định tại 0)_

Phương trình đạo hàm bằng 0 tương đương với: 
\\[
\mathbf{\bar{X}}\mathbf{\bar{X}}^T\mathbf{w} = \mathbf{\bar{X}}\mathbf{y}^T \triangleq \mathbf{b} 
~~~ (4)
\\]
(ký hiệu \\(\mathbf{\bar{X}}\mathbf{y}^T \triangleq \mathbf{b} \\) nghĩa là _đặt_ \\(\mathbf{\bar{X}}\mathbf{y}^T\\) _bằng_ \\(\mathbf{b}\\) ).

Nếu ma trận vuông \\( \mathbf{A} \triangleq \mathbf{\bar{X}}\mathbf{\bar{X}}^T\\) khả nghịch (non-singular hoặc inversable) thì phương trình \\((4)\\) có nghiệm duy nhất: \\( \mathbf{w} = \mathbf{A}^{-1}\mathbf{b}  \\).

Vậy nếu ma trận \\(\mathbf{A} \\) không khả nghịch (có định thức bằng 0) thì sao? Nếu các bạn vẫn nhớ các kiến thức về hệ phương trình tuyến tính, trong trường hợp này thì hoặc phương trinh \\( (4) \\) vô nghiệm, hoặc là có vô số nghiệm. Khi đó, chúng ta sử dụng khái niệm [_giả nghịch đảo_](https://vi.wikipedia.org/wiki/Giả_nghịch_đảo_Moore–Penrose) \\( \mathbf{A}^{\dagger} \\) (đọc là _A dagger_ trong tiếng Anh). (_Giả nghịch đảo (pseudo inverse) là trường hợp tổng quát của nghịch đảo khi ma trận không khả nghịch hoặc thậm chí không vuông. Trong khuôn khổ bài viết này, tôi xin phép được lược bỏ phần này, nếu các bạn thực sự quan tâm, tôi sẽ viết một bài khác chỉ nói về giả nghịch đảo. Xem thêm: [Least Squares, Pseudo-Inverses, PCA
& SVD](http://www.sci.utah.edu/~gerig/CS6640-F2012/Materials/pseudoinverse-cis61009sl10.pdf)._)

Với khái niệm giả nghịch đảo, điểm tối ưu của bài toán Linear Regression có dạng:

<!-- Từ đây chúng ta có thể suy ra nghiệm tối ưu \\(\mathbf{w}\\) như sau. Nếu ma trận vuông \\( \mathbf{A} \triangleq \mathbf{\bar{X}}\mathbf{\bar{X}}^T\\) khả nghịch (non-singular hoặc inversable) thì  -->
<!-- \\( \mathbf{w} = \mathbf{A}^{-1}\mathbf{b}  \\). Ngược lại, nếu ma trận đó không khả nghịch, chúng ta sử dụng [_giả nghịch đảo_](https://vi.wikipedia.org/wiki/Giả_nghịch_đảo_Moore–Penrose) \\( (\mathbf{A})^{\dagger} \\) (đọc là _A dagger_ trong tiếng Anh). (_Giả nghịch đảo (pseudo inverse) là trường hợp tổng quát của nghịch đảo khi ma trận không khả nghịch hoặc thậm chí không vuông_). Tóm lại, nghiệm của bài toán Linear Regression là:  -->

\\[
\mathbf{w} = \mathbf{A}^{\dagger}\mathbf{b} = (\mathbf{\bar{X}}\mathbf{\bar{X}}^T)^{\dagger} \mathbf{\bar{X}}\mathbf{y}^T
~~~ (5)
\\]



<!-- ========================== New Heading ==================== -->
<a name="-vi-du-tren-python"></a>

## 3. Ví dụ trên Python



Trong phần này, tôi sẽ chọn một ví dụ đơn giản về việc giải bài toán Linear Regression trong Python. Tôi cũng sẽ so sánh nghiệm của bài toán khi giải theo phương trình (5) và nghiệm tìm được khi dùng thư viện [scikit-learn](http://scikit-learn.org/stable/) của Python. (_Đây là thư viện Machine Learning được sử dụng rộng rãi trong Python_). Trong ví dụ này, dữ liệu đầu vào chỉ có 1 giá trị (1 chiều) để thuận tiện cho việc minh hoạ trong mặt phẳng. 

Chúng ta có 1 bảng dữ liệu về chiều cao và cân nặng của 15 người như dưới đây:

| **Chiều cao (cm)** |  147   |  150   |  153   |  155   |  158   |  160   |  163   |  165   |  168   |  170   |  173   |  175   |  178   |  180   |  183   |
|--------------------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|
| **Cân nặng (kg)**  | **49** | **50** | **51** | **52** | **54** | **56** | **58** | **59** | **60** | **62** | **63** | **64** | **66** | **67** | **68** |


Bài toán đặt ra là: liệu có thể dự đoán cân nặng của một người dựa vào chiều cao của họ không? (_Trên thực tế, tất nhiên là không, vì cân nặng còn phụ thuộc vào nhiều yếu tố khác nữa, thể tích chẳng hạn_). Vì blog này nói về các thuật toán Machine Learning đơn giản nên tôi sẽ giả sử như chúng ta có thể dự đoán được.

Chúng ta có thể thấy là cân nặng sẽ tỉ lệ thuận với chiều cao (càng cao càng nặng), nên có thể sử dụng Linear Regression model cho việc dự đoán này. Để kiểm tra độ chính xác của model tìm được, chúng ta sẽ giữ lại cột 155 và 160 cm để kiểm thử, các cột còn lại được sử dụng để huấn luyện (train) model.

Trước tiên, chúng ta cần có hai thư viện [numpy](http://www.numpy.org/) cho đại số tuyến tính và [matplotlib](http://matplotlib.org/) cho việc vẽ hình. 


```python
import numpy as np 
import matplotlib.pyplot as plt
```

Tiếp theo, chúng ta khai báo và biểu diễn dữ liệu trên một đồ thị.


```python
# height (cm)
X = np.array([[147, 150, 153, 158, 163, 165, 168, 170, 173, 175, 178, 180, 183]])
# weight (kg)
y = np.array([[ 49, 50, 51,  54, 58, 59, 60, 62, 63, 64, 66, 67, 68]])
# Visualize data 
plt.plot(X, y, 'ro')
plt.axis([140, 190, 45, 75])
plt.xlabel('Height (cm)')
plt.ylabel('Weight (kg)')
plt.show()
```


<!-- ![png](/assets/LR/output_3_0.png) -->
<div class="imgcap">
<img src ="/assets/LR/output_3_0.png" align = "center">
</div>


Từ đồ thị này ta thấy rằng dữ liệu được sắp xếp gần như theo 1 đường thẳng, vậy mô hình Linear Regression nhiều khả năng sẽ cho kết quả tốt:

(cân nặng) = w_1*(chiều cao) + w_0)

Tiếp theo, chúng ta sẽ tính toán các hệ số a và b dựa vào công thức (5).


```python
# Building Xbar 
one = np.ones((1,X.shape[1]))
Xbar = np.concatenate((X, one), axis = 0)

# Calculating weights of the fitting line 
A = np.dot(Xbar, Xbar.T)
b = np.dot(Xbar, y.T)
w = np.dot(np.linalg.pinv(A), b)
print 'w = ', w
# Preparing the fitting line 
w_1 = w[0][0]
w_0 = w[1][0]
x0 = np.linspace(145, 185, 2, endpoint=True)
y0 = w_1*x0 + w_0

# Drawing the fitting line 
plt.plot(X.T, y.T, 'ro')     # data 
plt.plot(x0, y0)               # the fitting line
plt.axis([140, 190, 45, 75])
plt.xlabel('Height (cm)')
plt.ylabel('Weight (kg)')
plt.show()

```

    w =  [[  0.55920496]
     [-33.73541021]]



<!-- ![pnet/asset/LR/output_5_1.png) -->
<div class="imgcap">
<img src ="/assets/LR/output_5_1.png" align = "center">
</div>

Từ đồ thị bên trên ta thấy rằng các điểm dữ liệu màu đỏ nằm khá gần đường thẳng dự đoán màu xanh. Vậy mô hình Linear Regression hoạt động tốt với tập dữ liệu _training_. Bây giờ, chúng ta sử dụng mô hình này để dự đoán cân nặng của hai người có chiều cao 155 và 160 cm mà chúng ta đã không dùng khi tính toán nghiệm.


```python
y1 = w_1*155 + w_0
y2 = w_1*160 + w_0

print u'Dự đoán cân nặng của người có chiều cao 155 cm: %.2f (kg), số liệu thật: 52 (kg)'  %(y1) 
print u'Dự đoán cân nặng của người có chiều cao 160 cm: %.2f (kg), số liệu thật: 56 (kg)'  %(y2) 
```

    Dự đoán cân nặng của người có chiều cao 155 cm: 52.94 (kg), số liệu thật: 52 (kg)
    Dự đoán cân nặng của người có chiều cao 160 cm: 55.74 (kg), số liệu thật: 56 (kg)



Chúng ta thấy rằng kết quả dự đoán khá gần với số liệu thực tế.


Tiếp theo, chúng ta sẽ sử dụng thư viện Scikit-learn của Python để tìm nghiệm. 


```python
from sklearn import datasets, linear_model

# fit the model by Linear Regression
regr = linear_model.LinearRegression(fit_intercept=False) # fit_intercept = False for calculating the bias
regr.fit(Xbar.T, Y.T)

# Compare two results
print u'Nghiệm tìm được bằng scikit-learn  : ', regr.coef_ 
print u'Nghiệm tìm được từ phương trình (5): ', W.T
```

    Nghiệm tìm được bằng scikit-learn  :  [[  0.55920496 -33.73541021]]
    Nghiệm tìm được từ phương trình (5):  [[  0.55920496 -33.73541021]]


Chúng ta thấy rằng hai kết quả thu được như nhau! (_Nghĩa là tôi đã không mắc lỗi nào trong cách tìm nghiệm ở phần trên_)



<!-- ========================== New Heading ==================== -->
<a name="-thao-luan"></a>

## 4. Thảo luận




<!-- ========================== New Heading ==================== -->
<a name="output-la-mot-vector-nhieu-bien"></a>

### Output là một vector nhiều biến

\\(f(\mathbf{x})\\) là một đa thức bậc cao. 




<!-- ========================== New Heading ==================== -->
<a name="mo-hinh-la-mot-da-thuc-bac-cao"></a>

### Mô hình là một đa thức bậc cao




<!-- ========================== New Heading ==================== -->
<a name="han-che-cua-linear-regression"></a>

### Hạn chế của Linear Regression
* Nhạy cảm với nhiễu 
* Không biểu diễn được các mô hình phức tạp 






<!-- ========================== New Heading ==================== -->
<a name="cac-phuong-phap-toi-uu"></a>

### Các phương pháp tối ưu

<!-- Giả sử chúng ta có các cặp (_input, outcome_) \\( (\mathbf{x}_1, \mathbf{y}_1), \dots, (\mathbf{x}_N, \mathbf{y}_N) \\), chúng ta phải tìm một hàm  -->



<!-- ========================== New Heading ==================== -->
<a name="-tai-lieu-tham-khao"></a>

## 5. Tài liệu tham khảo

[Data](http://people.sc.fsu.edu/~jburkardt/datasets/regression/regression.html)
[Least Squares, Pseudo-Inverses, PCA & SVD](http://www.sci.utah.edu/~gerig/CS6640-F2012/Materials/pseudoinverse-cis61009sl10.pdf)