<!-- Banner -->
<p align="center">
  <a href="https://www.uit.edu.vn/" title="Trường Đại học Công nghệ Thông tin" style="border: none;">
    <img src="https://i.imgur.com/WmMnSRt.png" alt="Trường Đại học Công nghệ Thông tin | University of Information Technology">
  </a>
</p>
<!-- Title -->
<h1 align="center"><b>ĐỒ ÁN MÔN HỌC</b></h1>
<h1 align="center"><b>Convolutional Neural Network - DenseNet</b></h1>


## GIỚI THIỆU ĐỒ ÁN
* **Chủ đề báo cáo:** Convolutional Neural Network - DenseNet
* **Tên môn học:** LẬP TRÌNH PYTHON CHO MÁY HỌC
* **Mã môn học:** CS116
* **Mã lớp:** CS116.M11
* **Giảng viên:** **TS. Nguyễn Vinh Tiệp** 

## GIỚI THIỆU NHÓM

| STT | Họ tên | MSSV |
| :---: | --- | --- |
| 1 | Nguyễn Xuân Minh | 19522054 | 
| 2 | Hà Văn Thanh | 19522224 |
| 3 | Nguyễn Đức Thắng | 19522206 |
# **MỤC LỤC**
- [**Giới thiệu tổng quan về mạng tích chập kết nối dày đặc - DenseNet**](#giới-thiệu-tổng-quan-về-mạng-tích-chập-kết-nối-dày-đặc---densenet)
- [**Nguyên lí hoạt động của DenseNet**](#nguyên-lí-hoạt-động-của-densenet)
- [**Ưu & nhược điểm của DenseNet**](#ưu--nhược-điểm-của-densenet)
  - [Ưu điểm](#ưu-điểm)
  - [Nhược điểm](#nhược-điểm)
- [**So sánh DenseNet với ResNet trên các tập dữ liệu**](#so-sánh-densenet-với-resnet-trên-các-tập-dữ-liệu)
  - [Bộ dữ liệu áp dụng](#bộ-dữ-liệu-áp-dụng)
  - [Nhận xét](#nhận-xét)
- [**Điều chỉnh siêu tham số cho mô hình DenseNet**](#điều-chỉnh-siêu-tham-số-cho-mô-hình-densenet)
  - [Các siêu tham số có trong mô hình & ý nghĩa](#các-siêu-tham-số-có-trong-mô-hình--ý-nghĩa)
    - [các siêu tham số liên quan đến cấu trúc mạng](#các-siêu-tham-số-liên-quan-đến-cấu-trúc-mạng)
  - [Các cách điều chỉnh siêu tham số cho mô hình](#các-cách-điều-chỉnh-siêu-tham-số-cho-mô-hình)
    - [Manual Search (Tìm kiếm thủ công)](#manual-search-tìm-kiếm-thủ-công)
    - [Grid Search (Xác nhận chéo)](#grid-search-xác-nhận-chéo)
    - [Random Search (Tìm kiếm ngẫu nhiên)](#random-search-tìm-kiếm-ngẫu-nhiên)
    - [Bayesian Optimization](#bayesian-optimization)
    - [Keras tuner](#keras-tuner)
- [**Áp dụng vào bài toán cụ thể**](#áp-dụng-vào-bài-toán-cụ-thể)
  - [Bài toán & bộ dữ liệu sử dụng](#bài-toán--bộ-dữ-liệu-sử-dụng)
    - [Lý do sử dụng DenseNet cho bài toán](#lý-do-sử-dụng-densenet-cho-bài-toán)
  - [Các Model khác để so sánh](#các-model-khác-để-so-sánh)
  - [Mã nguồn áp dụng & kết quả](#mã-nguồn-áp-dụng--kết-quả)
  - [kết quả](#kết-quả)
  - [Đánh giá](#đánh-giá)
- [**TÀI LIỆU THAM KHẢO**](#tài-liệu-tham-khảo)

## **Giới thiệu tổng quan về mạng tích chập kết nối dày đặc - DenseNet**
DenseNet (Dense connected convolutional network) là một mạng CNN mới cho nhận dạng đối tượng trực quan (visual object recognition). Nó cũng gần giống Resnet nhưng có một vài điểm khác biệt. Densenet có cấu trúc gồm các khối dày đặc (dense block) và các tầng chuyển tiếp( transition layer). Các khối dày đặc định nghĩa cách các đầu vào và đầu ra được nối với nhau, trong khi các tầng chuyển tiếp kiểm soát số lượng kênh sao cho nó không quá lớn.
![alt text](https://github.com/minz1337/CS116.M11/blob/main/image/densenet.png)
- Bài toán thường được sử dụng: Sử dụng được trong hầu hết các bài toán phát hiện (detect) và nhận diện (recognize).


## **Nguyên lí hoạt động của DenseNet**
Ở ResNet chúng ta phân tách hàm số thành một hàm xác định và một hàm phi tuyến:

<img src="https://latex.codecogs.com/svg.image?f(x)&space;=&space;x&space;&plus;&space;g(x)" title="f(x) = x + g(x)" />

Cùng nhắc lại công thức khai triển Taylor tại <img src="https://latex.codecogs.com/svg.image?x&space;=&space;0" title="x = 0" /> :

<img src="https://latex.codecogs.com/svg.image?f(x)&space;=&space;f(0)&space;&plus;&space;f'(x)x&space;&plus;&space;\frac{f''(x)}{2!}x^2&plus;...\frac{f^{(n)}(x)}{n!}x^n&space;&plus;&space;O(x^n)" title="f(x) = f(0) + f'(x)x + \frac{f''(x)}{2!}x^2+...\frac{f^{(n)}(x)}{n!}x^n + O(x^n)" />

Ta có thể thấy công thức của ResNet cũng gần tương tự như khai triển taylor tại đạo hàm bậc nhất, <img src="https://latex.codecogs.com/svg.image?g(x)" title="g(x)" />  tương ứng với thành phần số dư. Khai triển Taylor sẽ càng chuẩn xác nếu chúng ta phân rã được số dư thành nhiều đạo hàm bậc cao hơn.

Ý tưởng của DenseNet cũng như vậy, chúng ta sẽ sử dụng một mạng lưới các kết nối tắt dày đặc để liên kết các khối với nhau.

Từ đầu vào <img src="https://latex.codecogs.com/svg.image?x" title="x" /> ta sẽ áp dụng liên tiếp một chuỗi các ánh xạ liên tiếp với cấp độ phức tạp tăng dần:
![alt text](https://github.com/minz1337/CS116.M11/blob/main/image/pic14.png)

<img src="https://latex.codecogs.com/svg.image?x&space;\to&space;f_{1}(x)&space;x&space;\to&space;f_{2}(x,f_{1}(x))...&space;x&space;\to&space;f_{4}(x,f_{3}(x,f_{2}(x,f_{1}(x))))" title="x \to f_{1}(x) x \to f_{2}(x,f_{1}(x))... x \to f_{4}(x,f_{3}(x,f_{2}(x,f_{1}(x))))" />

DenseNet sẽ khác so với ResNet đó là chúng ta không cộng trực tiếp <img src="https://latex.codecogs.com/svg.image?x" title="x" /> vào <img src="https://latex.codecogs.com/svg.image?f(x)" title="f(x)" /> mà thay vào đó, các đầu ra của từng phép ánh xạ có cùng kích thước dài và rộng sẽ được concatenate với nhau thành một khối theo chiều sâu. Sau đó để giảm chiều dữ liệu chúng ta áp dụng tầng chuyển tiếp (translation layer). Tầng này là kết hợp của một layer tích chập giúp giảm độ sâu và một max pooling giúp giảm kích thước dài và rộng. 
![alt text](https://github.com/minz1337/CS116.M11/blob/main/image/pic15.png)
## **Ưu & nhược điểm của DenseNet**
### Ưu điểm
- DenseNet yêu cầu ít tham số đầu vào nhưng vẫn cho tỉ lệ chính xác cao. 
- DenseNet chống lại overfitting rất hiệu quả.
- Giảm được vanishing gradient (tình trạng biến mất đạo hàm ở các mạng neural nhiều lớp).
- DenseNet sử dụng lại đặc trưng hiệu quả hơn, duy trì được các tính năng phức tạp thấp.
### Nhược điểm
- DenseNet tiêu tốn rất nhiều bộ nhớ.
## **So sánh DenseNet với ResNet trên các tập dữ liệu**
### Bộ dữ liệu áp dụng
- Bộ dữ liệu quy mô nhỏ CIFAR - 10 (60 nghìn ảnh)

![alt text](https://github.com/minz1337/CS116.M11/blob/main/image/CIFAR-10.png)

- Bộ dữ liệu quy mô vừa CIFAR - 100 (80 triệu ảnh)

![alt text](https://github.com/minz1337/CS116.M11/blob/main/image/CIFAR-100.png)

- Bộ dữ liệu quy mô lớn : ImageNet (1 tỉ ảnh)

![alt text](https://github.com/minz1337/CS116.M11/blob/main/image/ImageNet.png)

### Nhận xét
- DenseNet hoạt động tốt hơn mô hình ResNet trên các tập dữ liệu CIFAR - 10, CIFAR - 100, ImageNet,...
- DenseNet, sử dụng các tính năng của tất cả các mức độ phức tạp nên việc học được tốt hơn, điều đó được chứng minh khi dữ liệu đào tạo không đủ thì DenseNet vẫn hoạt động tốt.


## **Điều chỉnh siêu tham số cho mô hình DenseNet**
### Các siêu tham số có trong mô hình & ý nghĩa
- **include_top**: có bao gồm lớp được kết nối đầy đủ ở đầu mạng hay không.
- **weights** : Trọng số của mô hình. một trong số None(khởi tạo ngẫu nhiên), 'imagenet' (đào tạo trước trên ImageNet) hoặc đường dẫn đến tệp weight.
- **input_tensor**: tensor Keras tùy chọn (tức là đầu ra của layers.Input()) để sử dụng làm đầu vào hình ảnh cho mô hình.
- **input_shape**: bộ hình dạng(shape) tùy chọn. Chỉ được chỉ định khi **include_top** là False.Nếu không hình dạng đầu vào phải có chính xác 3 kênh đầu vào và chiều rộng và chiều cao không được nhỏ hơn 32. Ví dụ: (200, 200, 3)sẽ là một giá trị hợp lệ.
- **pooling**: Bắt buộc khi **include_top** là False. Gồm một trong các giá trị:
  -  None: đầu ra của mô hình sẽ là đầu ra tensor 4D của khối chập cuối cùng. 
  -  'avg': trung bình cộng sẽ được áp dụng cho đầu ra của khối chập cuối cùng và do đó đầu ra của mô hình sẽ là một tensor 2D. 
  -  'max': tổng sẽ được áp dụng cho đầu ra của khối chập cuối cùng.
- **class** :số lớp tùy chọn để phân loại hình ảnh thành, chỉ được chỉ định nếu **include_top** là True và  **weights** không được chỉ định (None).
#### các siêu tham số liên quan đến cấu trúc mạng
- **Number of Hidden Layers and units**: số lớp ẩn của mô hình
- **Dropout**: bỏ bớt các lớp để tăng tổng quát hóa cho mô hình.
- **Activation function**: Hàm kích hoạt, gồm các giá trị
  - **Sigmoid**: được sử dụng trong lớp đầu ra trong khi phân lớp nhị phân (binary predictions).
  - **Softmax**: được sử dụng trong lớp đầu ra trong khi phân lớp nhiều lớp (multi-class predictions).
- **Learning Rate**: Tốc độ học của mô hình. Mặc định là 0.001.
- **Momentum**: Giúp ngăn chặn giao động (bị kẹt ở một cực trị địa phương). Thường có giá trị từ 0.5 đến 0.9
- **epochs**: số lần toàn bộ dữ liệu đào tạo được đưa vào mạng trong khi đào tạo.
- **Batch size**: số lượng mẫu được cung cấp cho mạng để cập nhật trọng số.
### Các cách điều chỉnh siêu tham số cho mô hình
#### Manual Search (Tìm kiếm thủ công)
 Ý tưởng là đầu tiên thực hiện các bước nhảy vọt về giá trị và sau đó là những bước nhảy nhỏ để tập trung vào một giá trị cụ thể hoạt động tốt hơn.
#### Grid Search (Xác nhận chéo)
Ý tưởng của Grid Search là thử tất cả các kết hợp đầy đủ của các giá trị tham số do chính mình cung cấp và chọn giá trị tốt nhất trong số đó.

    from sklearn.model_selection import GridSearchCV
    batch_size = [10, 20, 40, 60, 80, 100,200,500]
    epochs = [10, 50, 100]
    learn_rate = [0.001, 0.01, 0.1, 0.2, 0.3]
    momentum = [0.0, 0.2, 0.4, 0.6, 0.8, 0.9]
    init_mode = ['uniform', 'lecun_uniform', 'normal', 'zero', 'glorot_normal', 'glorot_uniform', 'he_normal', 'he_uniform']
    activation = ['softmax', 'softplus', 'softsign', 'relu', 'tanh', 'sigmoid', 'hard_sigmoid', 'linear']
    weight_constraint = [1, 2, 3, 4, 5]
    dropout_rate = [0.0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9]
    param_grid = dict(batch_size=batch_size, epochs=epochs,learn_rate=learn_rate, momentum=momentum,init_mode=init_mode,activation=activation,dropout_rate=dropout_rate,          weight_constraint=weight_constraint)
    grid = GridSearchCV(estimator=model, param_grid=param_grid, n_jobs=-1, cv=3)
    grid_result = grid.fit(X, Y)
    
    print("Best: %f using %s" % (grid_result.best_score_, grid_result.best_params_))
    means = grid_result.cv_results_['mean_test_score']
    stds = grid_result.cv_results_['std_test_score']
    params = grid_result.cv_results_['params']
    for mean, stdev, param in zip(means, stds, params):
    print("%f (%f) with: %r" % (mean, stdev, param))
#### Random Search (Tìm kiếm ngẫu nhiên)
Ý tưởng của Random Search cũng giống như Grid Search, tuy nhiên Grid Search phải thử **tất cả** các kết hợp tham số, còn Random Search chỉ có thể chọn một vài kết hợp **ngẫu nhiên** trong số tất cả các kết hợp có sẵn.

    
    from kerastuner import RandomSearch
    tuner = RandomSearch(build_model,
                        objective='val_accuracy',
                        max_trials = 5)

    tuner.search(train_df,train_labl,epochs=3,validation_data=(train_df,train_labl))
    model=tuner.get_best_models(num_models=1)[0]
    model.summary()
#### Bayesian Optimization
Ý tưởng của Bayesian Optimization là đưa ra một dự đoán thông minh về kết hợp tiếp theo sẽ được thử bằng cách xem kết quả của các kết hợp trước đó. Bất kỳ bộ siêu thông số nào tạo ra kết quả tốt hơn, nó sẽ hướng tới các giá trị đó. Do đó, tối ưu hóa việc lựa chọn các siêu tham số.

    from skopt import BayesSearchCV
    batch_size = [10, 20, 40, 60, 80, 100,200,500]
    epochs = [10, 50, 100]
    learn_rate = [0.001, 0.01, 0.1, 0.2, 0.3]
    momentum = [0.0, 0.2, 0.4, 0.6, 0.8, 0.9]
    init_mode = ['uniform', 'lecun_uniform', 'normal', 'zero', 'glorot_normal', 'glorot_uniform', 'he_normal', 'he_uniform']
    activation = ['softmax', 'softplus', 'softsign', 'relu', 'tanh', 'sigmoid', 'hard_sigmoid', 'linear']
    weight_constraint = [1, 2, 3, 4, 5]
    dropout_rate = [0.0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9]
    params = dict(batch_size=batch_size, epochs=epochs,learn_rate=learn_rate, momentum=momentum,init_mode=init_mode,activation=activation,dropout_rate=dropout_rate,          weight_constraint=weight_constraint)
    search = BayesSearchCV(estimator=model(), search_spaces=params, n_jobs=-1, cv=cv)
    model.summary()
#### Keras tuner
Vì Keras Tuner giúp dễ dàng xác định không gian tìm kiếm và tận dụng các thuật toán bao gồm để tìm các giá trị siêu tham số tốt nhất, do đó nhóm sử dụng keras tuner để tìm các siêu tham số.
1. pooling:
- ```pool=hp.Choice('pooling', values=['avg','max'])```
2. dropout:
- ```hp_dropout = hp.Choice('Dropout', values=[0.3, 0.4, 0.5])```
3. learning rate:
- ```hp_learning_rate = hp.Choice('learning_rate', values=[1e-2, 1e-3, 1e-4])```
## **Áp dụng vào bài toán cụ thể**
### Bài toán & bộ dữ liệu sử dụng
- Bài toán : phát hiện người bị nhiễm COVID thông qua ảnh chụp  CT Scan.
- Bộ dữ liệu sử dụng : [data](https://www.dropbox.com/s/7ck9lx3xf5t58d6/test.npz?dl=0)
#### Lý do sử dụng DenseNet cho bài toán
- Đây là bài toán phân lớp hình ảnh (phù hợp với model tìm hiểu).
- Bộ data đủ lớn và khá đa dạng.
### Các Model khác để so sánh 
- VGG16, ResNet: do đây là những Model kế trước của DenseNet, DenseNet được xây dựng dựa trên ResNet, nên chúng em sử dụng 2 Model này để tiện cho việc so sánh, kiểm thử xem ưu nhược điểm có giống như lý thuyết không.
### Mã nguồn áp dụng & kết quả 
[![Open in colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1u_Qj7UJ_cr2gZL9TunJ4qFFYrCNd3JLK?usp=sharing)
### kết quả


| STT | Model | accuracy | thời gian chạy (giây)|
| :---: | --- | --- | --- |
| 1 | DenseNet-169 | 0.8584 |  330   | 
| 2 | resNet50v2  | 0.8204 |  268    |
| 3 | VGG-16 | 0.8219 |    846    |
### Đánh giá
- DenseNet cho kết quả tốt nhất trong 3 mô hình nhưng thời gian chạy lại lâu hơn resNet.
- VGG16 có thời gian chạy lại lâu nhất trong số 3 mô hình thử nghiệm.
## **TÀI LIỆU THAM KHẢO**
[**[1] Đánh giá mạng DenseNet**](https://towardsdatascience.com/review-densenet-image-classification-b6631a8ef803)

[**[2] DenseNet Keras**](https://keras.io/api/applications/densenet/)

[**[3] DenseNet- Dense Convolutional Network**](https://towardsdatascience.com/review-densenet-image-classification-b6631a8ef803)


[**[4] Introduction to DenseNet with TensorFlow**](https://www.pluralsight.com/guides/introduction-to-densenet-with-tensorflow)
