# Nhập Môn Xử Lý Ảnh Số - Lab 5  
## **Xác định đối tượng trong ảnh**
**Sinh viên thực hiện:** Hà Đại Vĩ 
**MSSV:2374802010551** 
**Môn học:** Nhập môn Xử lý ảnh số  
**Giảng viên:** Đỗ Hữu Quân  

---

## Giới thiệu

Bài Lab  giúp sinh viên hiểu và thực hành các thao tác:
- Ngưỡng hóa ảnh bằng các phương pháp Otsu, Adaptive Threshold
- Biến đổi hình học: xoay, tịnh tiến, ánh xạ tọa độ
- Phân đoạn ảnh, gán nhãn và trích xuất thông tin từ các đối tượng
- Áp dụng Harris Corner Detector và Hough Transform để phát hiện đặc trưng và trích xuất biên
- Kết hợp kỹ thuật mô tả và so khớp để thực hiện image matching

---

## Công nghệ sử dụng

- **Python** – ngôn ngữ chính
- **OpenCV** – xử lý ảnh, ngưỡng hóa, biến đổi hình học
- **NumPy** – xử lý mảng ảnh
- **SciPy** – thực hiện toán tử morphological như Closing
- **Matplotlib** – hiển thị kết quả
- **Scikit-image** – thư viện xử lý ảnh nâng cao

---

## Nội dung thực hiện

### Bài 2.1: Gán nhãn và vẽ Bounding Box cho các đối tượng bằng Otsu Threshold

- Chuyển ảnh sang xám, áp dụng ngưỡng Otsu
- Phân vùng ảnh nhị phân
- Gán nhãn các vùng liên thông
- Vẽ khung giới hạn quanh các đối tượng được phát hiện

**Code chính:**
```python
a = np.asarray(Image.open('geometric.png').convert('L'))
thres = threshold_otsu(a)
b = a > thres
c = label(b)
d = regionprops(c)
for i in d:
    lr, lc, ur, uc = i['BoundingBox']
    rect = mpatches.Rectangle((lc, lr), uc - lc, ur - lr, fill=False, edgecolor='black', linewidth=2)
    ax.add_patch(rect)
```

---

### Bài 2.2: Phát hiện biên bằng phép dịch ảnh
- Dùng toán tử dịch ảnh sang phải
- Trừ 2 ảnh để phát hiện sự thay đổi => biên

**Code chính:**
```python
data = Image.open('geometric.png').convert('L')
bmg = abs(data - nd.shift(data, (0, 1), order=0))
```

---

### Bài 2.3: Phát hiện biên bằng Sobel filter
- Dùng Sobel theo 2 hướng (x, y)
- Lấy tổng độ lớn gradient để xác định biên
**Code chính:**
```python
a = nd.sobel(data, axis=0)
b = nd.sobel(data, axis=1)
bmg = abs(a) + abs(b)
```

---

### Bài 2.4: Harris Corner Detection thủ công

- Tính đạo hàm ảnh theo Sobel
- Tính ma trận đặc trưng và phản hồi Harris
- Hiển thị kết quả

**Code chính:**
```python
x = nd.sobel(indata, 0)
y = nd.sobel(indata, 1)
xl = nd.gaussian_filter(x**2, 3)
yl = nd.gaussian_filter(y**2, 3)
xy = nd.gaussian_filter(abs(x * y), 3)
R = xl * yl - 2 * xy - alpha * (xl + yl) ** 2
```

---

### Bài 2.5.1:  Line Detection với Hough Transform

- Tạo ảnh chứa điểm trắng
- Tính tích lũy trong không gian Hough
- Hiển thị không gian Hough

**Code chính:**
```python
for i in range(len(rh)):
    if 0 <= rh[i] < R and 0 <= tp[i] < 90:
        ho[int(rh[i]), int(tp[i])] += mx
```
---

### Bài 2.5.2: Phát hiện góc bằng corner_harris

- Đọc ảnh màu, chuyển sang ảnh xám
- Áp dụng corner_harris để phát hiện điểm góc

**Code chính:**
```python
image_gray = rgb2gray(iio.imread('bird.png'))
coordinate = corner_harris(image_gray, k = 0.001)
```
---

### Bài 2.6: Image Matching

- Dùng Harris để tìm điểm góc ở cả hai ảnh
- Cắt vùng lân cận quanh mỗi điểm để tạo descriptor
- So khớp descriptor giữa hai ảnh bằng khoảng cách Euclidean
- Vẽ các đường nối giữa cặp điểm tương đồng

**Code chính:**
```python
desc1, keypoints1 = extract_descriptors(img1, corners1)
desc2, keypoints2 = extract_descriptors(img2, corners2)
matches = match_descriptors(desc1, desc2)

for idx1, idx2 in matches:
    y1, x1 = keypoints1[idx1]
    y2, x2 = keypoints2[idx2]
    ax.plot([x1, x2 + w1], [y1, y2], 'r-')
```
---

## Cấu trúc file

```
├── geometric.png
├── bird.png
├── label_output.jpg
└── README.md     
```

## Hướng dẫn

### 1. Cài đặt môi trường

Cài Python, sau đó cài các thư viện:

```bash
pip install opencv-python matplotlib scipy numpy pillow
```

### 2. Chạy notebook

- Mở Jupyter Notebook trên VSCode/Colab
- Chạy từng cell để xem kết quả của các thuật toán
- Nếu có lỗi không tìm thấy ảnh, đảm bảo file ảnh đã được đặt đúng vị trí

### 3. Tùy chỉnh tham số

- Thay đổi giá trị `gamma, c, bit-plane` trong từng cell để quan sát tác động thực tế

---

## Tài liệu tham khảo

- [Digital Image Processing - Rafael C. Gonzalez](https://www.amazon.com/Digital-Image-Processing-Rafael-Gonzalez/dp/013168728X)
- [Scikit-image: Image processing in Python](https://scikit-image.org/)
- [OpenCV Documentation](https://docs.opencv.org/)
- Slide bài giảng Nhập môn Xử lý ảnh số - Văn Lang University

---




