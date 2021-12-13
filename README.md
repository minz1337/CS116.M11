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

## Giới thiệu tổng quan về Convolutional Neural Network - CNN


## Giới thiệu tổng quan về mạng tích chập kết nối dày đặc - DenseNet
DenseNet(Dense connected convolutional network) là một mạng CNN mới cho nhận dạng đối tượng trực quan (visual object recognition). Nó cũng gần giống Resnet nhưng có một vài điểm khác biệt. Densenet có cấu trúc gồm các khối dày đặc (dense block) và các tầng chuyển tiếp( transition layer). Các khối dày đặc định nghĩa cách các đầu vào và đầu ra được nối với nhau, trong khi các tầng chuyển tiếp kiểm soát số lượng kênh sao cho nó không quá lớn.
- Bài toán thường được sử dụng: Sử dụng được trong hầu hết các bài toán phát hiện (detect) và nhận diện (recognize).


## Nguyên lí hoạt động của DenseNet
Ở ResNet chúng ta phân tách hàm số thành một hàm xác định và một hàm phi tuyến:

<img src="https://latex.codecogs.com/svg.image?f(x)&space;=&space;x&space;&plus;&space;g(x)" title="f(x) = x + g(x)" />

Cùng nhắc lại công thức khai triển Taylor tại <img src="https://latex.codecogs.com/svg.image?x&space;=&space;0" title="x = 0" /> :

<img src="https://latex.codecogs.com/svg.image?f(x)&space;=&space;f(0)&space;&plus;&space;f'(x)x&space;&plus;&space;\frac{f''(x)}{2!}x^2&plus;...\frac{f^{(n)}(x)}{n!}x^n&space;&plus;&space;O(x^n)" title="f(x) = f(0) + f'(x)x + \frac{f''(x)}{2!}x^2+...\frac{f^{(n)}(x)}{n!}x^n + O(x^n)" />

Ta có thể thấy công thức của ResNet cũng gần tương tự như khai triển taylor tại đạo hàm bậc nhất, <img src="https://latex.codecogs.com/svg.image?g(x)" title="g(x)" />  tương ứng với thành phần số dư. Khai triển Taylor sẽ càng chuẩn xác nếu chúng ta phân rã được số dư thành nhiều đạo hàm bậc cao hơn.

Ý tưởng của DenseNet cũng như vậy, chúng ta sẽ sử dụng một mạng lưới các kết nối tắt dày đặc để liên kết các khối với nhau.

Từ đầu vào <img src="https://latex.codecogs.com/svg.image?x" title="x" /> ta sẽ áp dụng liên tiếp một chuỗi các ánh xạ liên tiếp với cấp độ phức tạp tăng dần:

<img src="https://latex.codecogs.com/svg.image?x&space;\to&space;f_{1}(x)&space;x&space;\to&space;f_{2}(x,f_{1}(x))...&space;x&space;\to&space;f_{4}(x,f_{3}(x,f_{2}(x,f_{1}(x))))" title="x \to f_{1}(x) x \to f_{2}(x,f_{1}(x))... x \to f_{4}(x,f_{3}(x,f_{2}(x,f_{1}(x))))" />

DenseNet sẽ khác so với ResNet đó là chúng ta không cộng trực tiếp <img src="https://latex.codecogs.com/svg.image?x" title="x" /> vào <img src="https://latex.codecogs.com/svg.image?f(x)" title="f(x)" /> mà thay vào đó, các đầu ra của từng phép ánh xạ có cùng kích thước dài và rộng sẽ được concatenate với nhau thành một khối theo chiều sâu. Sau đó để giảm chiều dữ liệu chúng ta áp dụng tầng chuyển tiếp (translation layer). Tầng này là kết hợp của một layer tích chập giúp giảm độ sâu và một max pooling giúp giảm kích thước dài và rộng. 
## Ưu & nhược điểm của DenseNet
### Ưu điểm
- DenseNet yêu cầu ít tham số đầu vào nhưng vẫn cho tỉ lệ chính xác cao. 
- DenseNet chống lại overfitting rất hiệu quả.
- Giảm được vanishing gradient( tình trạng biến mất đạo hàm ở các mạng neural nhiều lớp).
- DenseNet sử dụng lại đặc trưng hiệu quả hơn, duy trì được các tínhnăng phức tạp thấp.
### Nhược điểm
- DenseNet tiêu tốn rất nhiều bộ nhớ.
## Điều chỉnh siêu tham số cho mô hình DenseNet
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
  - **Sigmoid**: được sử dụng trong lớp đầu ra trong khi phân lớp nhị phân(binary predictions).
  - **Softmax**: được sử dụng trong lớp đầu ra trong khi phân lớp nhiều lớp(multi-class predictions).
- **Learning Rate**: Tốc độ học của mô hình. Mặc định là 0.001.
- **Momentum**: Giúp ngăn chặn giao động (bị kẹt ở một cực trị địa phương). Thường có giá trị từ 0.5 đến 0.9
- **epochs**: số lần toàn bộ dữ liệu đào tạo được đưa vào mạng trong khi đào tạo.
- **Batch size**: số lượng mẫu được cung cấp cho mạng để cập nhật trọng số.
### Các cách điều chỉnh siêu tham số cho mô hình
#### **Manual Search (Tìm kiếm thủ công)**:
 Ý tưởng là đầu tiên thực hiện các bước nhảy vọt về giá trị và sau đó là những bước nhảy nhỏ để tập trung vào một giá trị cụ thể hoạt động tốt hơn.
#### **Grid Search (Xác nhận chéo)**:
Ý tưởng của Grid Search là thử tất cả các kết hợp đầy đủ của các giá trị tham số do chính mình cung cấp và chọn giá trị tốt nhất trong số đó.
#### **Random Search(Tìm kiếm ngẫu nhiên)**:
Ý tưởng của Random Search cũng giống như Grid Search, tuy nhiên Grid Search phải thử **tất cả** các kết hợp tham số, còn Random Search chỉ có thể chọn một vài kết hợp **ngẫu nhiên** trong số tất cả các kết hợp có sẵn.
#### **Bayesian Optimization**:
Ý tưởng của Bayesian Optimization là đưa ra một dự đoán thông minh về kết hợp tiếp theo sẽ được thử bằng cách xem kết quả của các kết hợp trước đó. Bất kỳ bộ siêu thông số nào tạo ra kết quả tốt hơn, nó sẽ hướng tới các giá trị đó. Do đó, tối ưu hóa việc lựa chọn các siêu tham số.
## **Áp dụng vào bài toán cụ thể**
### Bài toán & bộ dữ liệu sử dụng
- Bài toán : phát hiện người bị nhiễm COVID thông qua ảnh chụp  CT Scan.
- Bộ dữ liệu sử dụng : [data](https://www.kaggle.com/maedemaftouni/large-covid19-ct-slice-dataset)
#### **Lý do sử dụng DenseNet cho bài toán**:
- Đây là bài toán phân lớp hình ảnh (phù hợp với model tìm hiểu).
- Bộ data đủ lớn và khá đa dạng.
### Mã nguồn áp dụng & kết quả 
