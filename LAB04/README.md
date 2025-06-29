
 Nhập Môn Xử Lý Ảnh Số - Lab 04
Phân Vùng Ảnh & Biến Đổi Hình Học

Sinh viên thực hiện: Nguyễn Thành Đạt 
MSSV: 207CT58538
Môn học: Nhập môn Xử lý ảnh số  
Giảng viên: Đỗ Hữu Quân

---

 🎯 Giới thiệu

Bài lab này nhằm mục đích thực hành các kỹ thuật:
- Phân vùng ảnh (Image Segmentation)
- Biến đổi hình học (Geometric Transformation)
- Tách đối tượng, trích xuất vùng quan tâm (ROI)
- Ứng dụng các phép co giãn (Dilation, Erosion), mở (Opening), đóng (Closing)

Giúp sinh viên:
- Hiểu và vận dụng các phương pháp ngưỡng hóa (Otsu, Adaptive Thresholding)
- Thao tác tịnh tiến, xoay, scale, mapping tọa độ
- Tiền xử lý dữ liệu ảnh phục vụ các bài toán phân tích nâng cao

 ⚙️ Công nghệ sử dụng

- Python: Ngôn ngữ chính
- Pillow (PIL): Đọc, chuyển đổi, lưu ảnh
- NumPy: Biểu diễn & xử lý dữ liệu ảnh dạng mảng số học
- ImageIO: Đọc file ảnh nhiều định dạng
- Matplotlib: Hiển thị ảnh trực quan
- Scikit-Image (skimage): Thuật toán Otsu, Adaptive Thresholding

 🧩 Chi tiết các phương pháp

 1️⃣ Phân vùng theo Otsu
- Mục đích: Tìm ngưỡng toàn cục tự động tách foreground & background.
- Nguyên lý: Tìm ngưỡng sao cho phương sai giữa các lớp được tối đa.
- Ứng dụng: Phân vùng LangBiang với ngưỡng Otsu * 0.3.

\`\`\`python
from skimage.filters import threshold_otsu

thres = threshold_otsu(image)
binary = image > (0.3 * thres)
\`\`\`

 2️⃣ Phân vùng theo Adaptive Thresholding
- Mục đích: Tính ngưỡng riêng cho từng vùng nhỏ.
- Ưu điểm: Phù hợp ảnh nền phức tạp, chi tiết nhỏ.
- Ứng dụng: Phân vùng Hồ Xuân Hương.

\`\`\`python
from skimage.filters import threshold_local

thresh = threshold_local(image, block_size=35, offset=10)
binary = image > thresh
\`\`\`

 3️⃣ Biến đổi hình học

- Tịnh tiến (Shift)

\`\`\`python
translated = np.zeros_like(image)
translated[y:y+h, x+dx:x+dx+w] = roi
\`\`\`

- Xoay (Rotate)

\`\`\`python
rotated = nd.rotate(roi, 45, reshape=True)
\`\`\`

- Co giãn (Dilation)

\`\`\`python
dilated = nd.binary_dilation(binary_image, iterations=5)
\`\`\`

- Erosion

\`\`\`python
eroded = nd.binary_erosion(binary_image, iterations=5)
\`\`\`

- Đóng (Closing)

\`\`\`python
closed = nd.binary_closing(binary_image, iterations=5)
\`\`\`

 4️⃣ Bài tập thực hành

1️⃣ LangBiang: Chọn vùng LangBiang, tịnh tiến sang phải 100px, phân vùng Otsu, lưu lang_biang.jpg.  
2️⃣ Hồ Xuân Hương: Chọn vùng Hồ Xuân Hương, xoay 45°, phân vùng Adaptive Thresholding, lưu ho_xuan_huong.jpg.  
3️⃣ Quảng trường Lâm Viên: Chọn vùng Quảng trường Lâm Viên, áp dụng Coordinate Mapping + Binary Closing, lưu quan_truong_lam_vien.jpg.  
4️⃣ Tạo menu: Cho phép nhập chức năng: Rotate, Scale, Shift, Otsu, Adaptive, Dilation, Erosion...



 ✅ Hướng dẫn cài đặt

\`\`\`bash
pip install pillow numpy matplotlib imageio scikit-image
\`\`\`

Đặt file ảnh đúng thư mục, chạy script Python.
Thay đổi tham số block_size, offset, iterations để quan sát kết quả.
