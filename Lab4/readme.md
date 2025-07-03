# Nhập Môn Xử Lý Ảnh Số - Lab 4  
## **Phân vùng ảnh**
**Sinh viên thực hiện:** Hà Đại Vĩ 
**MSSV:** 
**Môn học:** Nhập môn Xử lý ảnh số  
**Giảng viên:** Đỗ Hữu Quân  

---

## Giới thiệu

Bài Lab 4 giúp sinh viên hiểu và thực hành các thao tác:
- **Biến đổi hình học** như xoay, tịnh tiến, ánh xạ tọa độ
- **Phân đoạn ảnh** bằng các kỹ thuật Otsu, Adaptive Thresholding, Closing, Dilation, Erosion
- Kết hợp nhiều kỹ thuật xử lý để làm tiền xử lý ảnh, tách đối tượng, tăng cường chi tiết phục vụ cho các bài toán cao hơn như nhận dạng, phân loại ảnh.

---

## Công nghệ sử dụng

- **Python** – ngôn ngữ chính
- **OpenCV** – xử lý ảnh, ngưỡng hóa, biến đổi hình học
- **NumPy** – xử lý mảng ảnh
- **SciPy** – thực hiện toán tử morphological như Closing
- **Matplotlib** – hiển thị kết quả

---

## Nội dung thực hiện

### Bài 1: Phân đoạn vùng LangBiang bằng Otsu Thresholding

- Cắt vùng chữ "LangBiang" từ ảnh tổng
- Tịnh tiến sang phải 100px
- Áp dụng phương pháp Otsu (với hệ số ngưỡng nhân 0.3)
- Hiển thị và lưu ảnh kết quả

**Code chính:**
```python
langbiang_crop = img_rgb[0:300, 0:370]
translated_img = np.zeros_like(langbiang_crop)
translated_img[:, 100:] = langbiang_crop[:, :270]
gray = cv2.cvtColor(translated_img, cv2.COLOR_RGB2GRAY)
otsu_thresh, _ = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
adjusted_thresh = otsu_thresh * 0.3
_, binary_img = cv2.threshold(gray, adjusted_thresh, 255, cv2.THRESH_BINARY)
```

---

### Bài 2: Xoay ảnh Hồ Xuân Hương & Adaptive Thresholding

- Cắt vùng hồ Xuân Hương
- Xoay ảnh 45 độ quanh tâm
- Áp dụng Adaptive Threshold với ngưỡng 60
- Hiển thị & lưu ảnh kết quả

**Code chính:**
```python
ho_crop = img_rgb[250:500, 370:630]
rotated = rotate_image(ho_crop, 45)
gray = cv2.cvtColor(rotated, cv2.COLOR_RGB2GRAY)
adaptive = cv2.adaptiveThreshold(gray, 255,
    cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 11, 60)
```

---

### Bài 3: Quảng trường Lâm Viên & Binary Closing

- Cắt vùng Quảng trường Lâm Viên
- Ánh xạ tọa độ (coordinate mapping – ảnh âm bản)
- Áp dụng Binary Closing để lấp lỗ và kết nối vùng
- Hiển thị và lưu ảnh kết quả

**Code chính:**
```python
lam_vien_crop = img_rgb[0:250, 750:1110]
gray = cv2.cvtColor(lam_vien_crop, cv2.COLOR_RGB2GRAY)
mapped = 255 - gray
_, binary = cv2.threshold(mapped, 100, 255, cv2.THRESH_BINARY)
closed = binary_closing(binary, structure=np.ones((5, 5), dtype=bool))
closed_img = (closed * 255).astype(np.uint8)
```

---

### Bài 4: Tạo Menu xử lý ảnh linh hoạt

- Tạo menu với 2 nhóm chức năng:
  - `geometric_transformation`: coordinate_mapping, rotate, scale, shift
  - `segment`: adaptive_threshold, binary_dilation, binary_erosion, otsu
- Cho phép người dùng nhập lựa chọn (1 chức năng hoặc kết hợp 2)
- Thực thi tuần tự các thao tác trên ảnh mẫu (ảnh LangBiang)
- Hiển thị ảnh kết quả cuối cùng

**Code chính:**
```python
if choice == "coordinate_mapping":
    result = 255 - cv2.cvtColor(result, cv2.COLOR_RGB2GRAY)
elif choice == "rotate":
    result = rotate(result, angle=45)
elif choice == "scale":
    result = cv2.resize(result, None, fx=1.2, fy=1.2)
elif choice == "shift":
    result = shift(result, dx=50, dy=30)
elif choice == "adaptive_threshold":
    result = cv2.adaptiveThreshold(cv2.cvtColor(result, cv2.COLOR_RGB2GRAY),
               255, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 11, 5)
elif choice == "binary_dilation":
    result = binary_dilation_func(result)
elif choice == "binary_erosion":
    result = binary_erosion_func(result)
elif choice == "otsu":
    result = otsu(result)
```
---

## Cấu trúc file

```
├── main.ipynb      
├── image.png        
├── README.md       
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



