
#  Nhập Môn Xử Lý Ảnh Số – Lab 3  
**Chủ đề:** Biến đổi hình học trên ảnh xám  
**Sinh viên thực hiện:** Hà Đại Vĩ  **MSSV:** 2374802010551  
**Môn học:** Nhập môn Xử lý ảnh số  
**Giảng viên:** Thầy Đỗ Hữu Quân  

---

#  Mục tiêu bài Lab  
Trong bài lab này, em thực hành các phép biến đổi hình học cơ bản như: tịnh tiến, xoay, phóng to – thu nhỏ ảnh, và xem tọa độ pixel. Các thao tác này rất hay dùng khi xử lý ảnh thật, ví dụ như chỉnh bố cục ảnh, ghép ảnh, hoặc tạo hiệu ứng.  
---
##  Công cụ sử dụng  
- **Python**: ngôn ngữ chính để viết chương trình  
- **NumPy**: xử lý ảnh như mảng số  
- **SciPy (ndimage)**: dùng để xoay, phóng to, tịnh tiến...  
- **ImageIO**: đọc ảnh  
- **Matplotlib**: hiển thị ảnh và lấy tọa độ  
---
## Các phép biến đổi đã làm
### 1. Tịnh tiến ảnh  
Di chuyển ảnh sang trái/phải hoặc lên/xuống bằng cách thay đổi tọa độ điểm ảnh.  
```python
dich = nd.shift(img, shift=(dx, dy))
```

---
### 2. Chuyển ảnh sang ảnh xám  
Nếu ảnh màu thì chuyển sang xám bằng cách lấy trung bình R, G, B  
```python
gray = img.mean(axis=2).astype(np.uint8)
```
---
### 3. Xoay ảnh  
Xoay ảnh một góc tùy ý (ví dụ xoay 20 độ)  
```python
xoay = nd.rotate(img, 20)
```
---
### 4. Phóng to ảnh  
Cắt một phần nhỏ của ảnh rồi zoom lớn lên  
```python
cat = img[y1:y2, x1:x2]
zoom = nd.zoom(cat, zoom=2)
```
---
### 5. Thu nhỏ ảnh  
Làm nhỏ ảnh lại bằng cách giảm kích thước  
```python
nho = nd.zoom(img, zoom=0.5)
```
---
## 6. Hiển thị tọa độ  
Mục tiêu là khi chỉ chuột vào ảnh, chương trình sẽ hiện ra tọa độ của điểm đó. Cái này giúp biết chính xác vị trí pixel nào trên ảnh.  
---
---
##  chạy chương trình  
1. Cài thư viện cần thiết:  
```bash
pip install numpy scipy matplotlib imageio
```
2. Mở `main.ipynb` trên VSCode 
3. Chạy từng ô và xem kết quả ảnh sau mỗi biến đổi  
4. Có thể thay đổi góc xoay, độ zoom, độ dịch chuyển để thử nghiệm thêm
---

##  Tài liệu tham khảo  
- file pdf thực hành – Trường ĐH Văn Lang

