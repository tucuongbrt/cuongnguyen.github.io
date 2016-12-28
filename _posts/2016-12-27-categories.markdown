---
layout: post
comments: true
title:  "Phân loại các thuật toán Machine Learning"
date:   2016-12-26 15:22:00
mathjax: true
---

<div class="imgcap">
<div >
<a href = "/2016/12/26/categories/">
    <img src="/assets/categories/alphago.jpeg" width = "800"></a>
    <!-- <img src="/assets/rl/mdp.png" height="206"> -->
</div>
<div class="thecap">AlphaGo chơi cờ vây với Lee Sedol. AlphaGo là một ví dụ của Reinforcement learning.</div>
</div>

Có hai cách phổ biến phân loại các thuật toán Machine learning. Một là dựa trên phương thức học (learning style), hai là dựa trên chức năng (function) (của mỗi thuật toán).

## Dựa trên phương thức học

Theo phương thức học, các thuật toán Machine Learning thường được chia làm 4 loại: Supervise learning, Unsupervised learning, Semi-supervised lerning và Reinforcement learning. _Có một số cách phân loại không có Semi-supervised learning hoặc Reinforcement learning._

### Supervised Learning (Học có giám sát) 
Supervised learning là thuật toán dự đoán đầu ra (outcome) của một dữ liệu mới (new input) dựa trên các cặp (_input, outcome_) mẫu có sẵn. Cặp dữ liệu này còn được gọi là (_data. label_), tức (_dữ liệu, nhãn_). Supervised learning là loại phổ biến nhất trong các thuật toán Machine Learning. 

Một cách toán học, Supervised learning là khi chúng ra có một tập hợp biến đầu vào \\( \mathbf{X} = [\mathbf{x}_1, \mathbf{x}_2, \dots, \mathbf{x}_N] \\) và một tập hợp nhãn tương ứng \\( \mathbf{Y} = [\mathbf{y}_1, \mathbf{y}_2, \dots, \mathbf{y}_N] \\), trong đó \\( \mathbf{x}_i, \mathbf{y}_i \\) là các vector. Chúng ta cần tạo ra một hàm số ánh xạ từ tập \\(\mathbf{X}\\) sang tập \\(\mathbf{Y}\\):
\\[ \mathbf{y}_i = f(\mathbf{x}_i), ~~ \forall i = 1, 2, \dots, N\\] 
Mục đích là xấp xỉ hàm số \\(f\\) thật tốt để khi có một dữ liệu \\(\mathbf{x}\\) mới, chúng ta có thể tính được nhãn tương ứng của nó \\( \mathbf{y} = f(\mathbf{x}) \\).
    
**Ví dụ 1:** trong nhận dạng chữ viết tay, ta có ảnh của hàng nghìn ví dụ của mỗi chữ số được viết bởi nhiều người khác nhau. Chúng ta đưa các bức ảnh này vào trong một thuật toán và chỉ cho nó biết mỗi bức ảnh tương ứng với chữ số nào. Sau khi thuật toấn tạo ra (học được) một mô hình, tức một hàm số mà đầu vào là một bức ảnh và đầu ra là một chữ số, khi nhận được một bức ảnh mới mà mô hình **chưa nhìn thấy bao giờ**, nó sẽ dự đoán bức ảnh đó chứa chữ số nào.

Ví dụ này khá giống với cách học của con người khi còn nhỏ. Ta đưa bảng chữ cái cho một đứa trẻ và chỉ cho chúng đây là chữ A, đây là chữ B. Sau một vài lần được dạy thì trẻ có thể nhận biết được đâu là chữ A, đâu là chữ B trong một cuốn sách mà chúng chưa nhìn thấy bao giờ. 

**Ví dụ 2:** Thuật toán dò các khuôn mặt trong một bức ảnh đã có từ rất lâu (dựa vào vị trí tương đối của mắt, mũi, miệng và các bộ phận khác trong mỗi khuôn mặt). Những ngày đầu, facebook sử dụng thuật toán này để chỉ ra các khuôn mặt trong một bức ảnh và yêu cầu người dùng _tag friends_ - tức gán nhãn cho mỗi khuôn mặt. Số lượng cặp dữ liệu (_khuôn mặt, tên người_) càng lớn, độ chính xác ở những lần tự động _tag_ tiếp theo sẽ càng lớn.

**Ví dụ 3:** Bản thân thuật toán dò tìm các khuôn mặt trong 1 bức ảnh cũng là một thuật toán **Supervised learning** với training data (dữ liệu học) là hàng ngàn cặp (_ảnh, mặt người_) và (_ảnh, không phải mặt người_) được đưa vào. Chú ý là dữ liệu này chỉ phân biệt _mặt người_ và _không phải mặt ngưòi_ mà không phân biệt khuôn mặt của những người khác nhau.

Thuật toán supervised learning còn được tiếp tục chia nhỏ ra thành hai loại chính: 

* **Classification** (Phân loại): Một bài toán được gọi là _classification_ nếu _label_ của mỗi _input data_ được chia thành các nhóm. Ví dụ: xác định xem một email có phải là spam hay không; các hãng tín dụng xác định xem một khách hàng có khả năng thanh toán nợ hay không. Ba ví dụ phía trên được chia vào loại này. 
* **Regression** (tiếng Việt dịch là _Hồi quy_, tôi không thích cách dịch này vì bản thân không hiểu nó nghĩa là gì): Nếu _label_ không được chia thành các nhóm mà là một giá trị thực cụ thể. Ví dụ: giá cho thuê của một căn nhà rộng \\(x ~ \text{m}^2\\), có \\(y\\) phòng ngủ và cách trung tâm thành phố \\(z~ \text{km}\\) có giá là bao nhiêu. 
 
Gần đây [Microsoft có một ứng dụng dự đoán giới tính và tuổi](http://how-old.net/). Phần dự đoán giới tính có thể coi là thuật toán **Classification**, phần dự đoán tuổi có thể coi là thuật toán **Regression**. _Chú ý rằng phần dự đoán tuổi cũng có thể coi là **Classification** nếu ta coi tuổi là một số nguyên dương không lớn hơn 150, chúng ta sẽ có 150 class (lớp) khác nhau._

### Unsupervised Learning (Học không giám sát)
Trong thuật toán này, chúng ta không biết được _outcome_ hãy _nhãn_ mã chỉ có dữ liệu đầu vào. Thuật toán unsupervised learning sẽ dựa vào cấu trúc của dữ liệu để thực hiện một công việc nào đó, ví dụ như phân nhóm (clustering) hoặc giảm số chiều của dữ liệu (dimention reduction). 

Một cách toán học, Unsupervised learning là khi chúng ta chỉ có dữ liệu vào \\(\mathbf{X} \\) mà không biết _nhãn_ \\(\mathbf{Y}\\) tương ứng. 

Những thuật toán loại này được gọi là Unsupervised learning vì không giống như Supervised learning, chúng ta không biết câu trả lời chính xác cho mỗi dữ liệu đầu vào, hay nói cách khác, không có thầy cô giáo nào chỉ cho ta biết đó là chữ A hay chữ B. Cụm _không giám sát_ được đặt theo nghĩa này. 

Các bài toán Unsupervised learning được tiếp tục chia nhỏ thành hai loại: 

* **Clustering** (phân nhóm): một bài toán phân loại toàn bộ dữ liệu \\(\mathbf{X}\\) thành các nhóm nhỏ dựa trên sự liên quan giữa các dữ liệu trong mỗi nhóm. Ví dụ: phân nhóm khách hàng dựa trên hành vi mua hàng. Điều này cũng giống như việc ta đưa cho một đứa trẻ rất nhiều mảnh ghép với các hình thù và màu sắc khác nhau, ví dụ tam giác, vuông, tròn với màu xanh và đỏ, sau đó yêu cẩu trẻ nhóm chúng vào thành từng nhóm. Mặc dù không cho trẻ biết mảnh nào tương ứng với hình nào hoặc màu nào, nhiều khả năng chúng vẫn có thể phân loại các mảnh ghép theo màu hoặc hình dạng. 

* **Association**: Là bài toán khi bạn muốn khám phá ra một quy luật dữa trên nhiều dữ liệu cho trước. Ví dụ: những khách hàng nam mua quần áo thường có xu hướng mua thêm đồng hồ hoặc thắt lưng; những khán giả xem phim Spider Man thường có xu hướng xem thêm phim Bat Man, dựa vào đó tạo ra một hệ thống gợi ý khách hàng (Recommendation System), thúc đẩy nhu cầu mua sắm. 

### Semi-Supervised Learning (Học bán giám sát)
Các bài toán khi chúng ta có một lượng lớn dữ liệu \\(\mathbf{X}\\) nhưng chỉ một phần trong chúng được gán nhãn được gọi là Semi-Supervised Learning. Những bài toán thuộc nhóm này nằm giữa hai nhóm được nêu bên trên. 

Một ví dụ điển hình của nhóm này là chỉ có một lượng lớn ảnh hoặc văn bản được gán nhãn (ví dụ bức ảnh về người, động vật hoặc các văn bản khoa học, chính trị) và phần lớn các bức ảnh/văn bản khác chưa được gán nhãn được thu thập từ internet. Thực tế cho thấy rất nhiều các bài toàn Machine Learning thuộc vào nhóm này vì việc thu thập dữ liệu có nhãn tốn rất nhiều thời gian và chi phí cao và rất nhiều loại dữ liệu thậm chí cần phải có chuyên gia mới gán nhãn được (ảnh y học chẳng hạn). Ngược lại, dữ liệu chưa có nhãn có thể được thu thập với chi phí thấp từ internet. 


### Reinforcement Learning (Học Củng Cố)
Reinforcement learning là các bài toán giúp cho một hệ thống tự động xác định hành vi dựa trên hoàn cảnh để đạt được lợi ích cao nhất (maximizing the performance). Hiện tại, Reinforcement learning chủ yếu được áp dụng vào Lý Thuyết Trò Chơi (Game Theory), các thuật toán cần xác định nưóc đi tiếp theo để đạt được điểm số cao nhất.


**Ví dụ 1:** [AlphaGo gần đây nổi tiếng với việc chơi cờ vây thắng cả con người](https://gogameguru.com/tag/deepmind-alphago-lee-sedol/). [Cờ vây được xem là có độ phức tạp cực kỳ cao](https://www.tastehit.com/blog/google-deepmind-alphago-how-it-works/) với tổng số nước đi là xấp xỉ \\(10^{761} \\) so với cờ vua là \\(10^{120} \\) và tổng số nguyên tử trong toàn vũ trụ là khoảng \\(10^{80}\\)!! Vì vậy, thuật toán phải chọn ra 1 nước đi tối ưu trong số hàng nhiều tỉ tỉ lựa chọn, và tất nhiên, không thể áp dụng thuật toán tương tự như [IBM Deep Blue](https://en.wikipedia.org/wiki/Deep_Blue_(chess_computer)) (IBM Deep Blue đã thắng con người trong môn cờ vua 20 năm trước). Về cơ bản, Alphago bao gốm các thuật toán thuộc cả Supervised learning và Reinforcement learning. Trong phần Supervised learning, dữ liệu từ các ván cờ do con người chơi với nhau. Tuy nhiên, mục đích cuối cùng của AlphaGo không phải là chơi như con người mà phải thậm chí thắng cả con người. Vì vậy, sau khi _học_ xong các ván cơ của con người, AlphaGo tự chơi với chính nó với hàng triệu ván chơi để tìm ra các nước đi mới tối ưu hơn. Thuật toán trong phần tự chơi này được xếp vào loại Reinforcement learning. (Xem thêm tại [Google DeepMind's AlphaGo: How it works](https://www.tastehit.com/blog/google-deepmind-alphago-how-it-works/)).

**Ví dụ 2:** [Huấn luyện cho máy tính chơi game Mario.](https://www.youtube.com/watch?v=qv6UVOQ0F44). Đây là một chương trình thú vị dạy máy tính chơi game Mario. Game này đơn giản hơn cờ vây vì tại một thời điểm, người chơi phải bấm một số lượng nhỏ các nút (di chuyển, nhảy, bắn đạn) hoặc không cần bấm nút nào. Đồng thời, phản ứng của máy cũng đơn giản hơn và lặp lại ở mỗi lần chơi (tại thời điểm cụ thể sẽ xuất hiện một chướng ngại vật cố định ở một vị trí cố định). Đầu vào của thuật toán là sơ đồ của màn hình tại thời điểm hiện tại, nhiệm vụ của thuật toán là với đầu vào đó, tổ hợp phím nào nên được bấm. Việc huấn luyện này được dựa trên điểm số cho việc di chuyển được bao xa trong thời gian bao lâu trong game, càng xa và càng nhanh thì được điểm càng cao. Thông qua huẩn luyện, thuật toán sẽ tìm ra một cách tối ưu để tối đa số điểm trên, qua đó đạt được mục đích cuối cùng là cứu công chúa. Click vào hình dưới để xem video.


<div class="imgcap">
<div >
<a href = "https://www.youtube.com/watch?v=qv6UVOQ0F44">
    <img src="https://img.youtube.com/vi/qv6UVOQ0F44/0.jpg" width = "800"></a>
</div>
<div class="thecap">Lập trình cho máy tính chơi game Mario</div>
</div>

## Dựa trên chức năng 

Có một cách phân loại thứ hai dựa trên 
Trong phần này, tôi xin chỉ liệt kê các thuật toán. Thông tin cụ thể sẽ được trình bày trong các bài viết khác tại blog này. Trong quá trình viết, tôi có thể sẽ thêm bớt một số thuật toán. 

### Regression Algorithms

1. Linear Regression
2. Logistic Regression 
3. Stepwise Regression

### Classification Algorithms 

1. Linear Classifier 
2. Support Vector Machine (SMV)
3. Kernel SVM 
4. Sparse Represntation-based classification (SRC)
5. Neural Networks

### Instance-based Algorithms 

1. k-Nearest Neighbor (kNN)
2. Learnin Vector Quantization (LVQ)

### Regularization Algorithms 

1. Ridge Regression 
2. Least Absolute Shringkage and Selection Operator (LASSO)
3. Least-Angle Regression (LARS)

### Baysian Algorithms

1. Naive Bayes
2. Gausian Naive Bayes 

### Clustering Algorithms

1. k-Means 
2. k-Medians 
3. Expectation Maximisation (EM) 

### Artificial Neural Network Algorithms 

1. Perceptron
2. Back-Propagation 

### Dimensionality Reduction Algorithms 

1. Principal Component Analysis (PCA)
2. Linear Discriminant Analysis (LDA)

### Ensemble Algorithms 

1. Boosting
2. AdaBoost 
3. Randome Forest 

Và còn rất nhiều các thuật toán khác. 

## Tài liệu tham khảo 
1. [A Tour of Machine Learning Algorithms](http://machinelearningmastery.com/a-tour-of-machine-learning-algorithms/)
2. [Điểm qua các thuật toán Machine Learning hiện đại](https://ongxuanhong.wordpress.com/2015/10/22/diem-qua-cac-thuat-toan-machine-learning-hien-dai/)

