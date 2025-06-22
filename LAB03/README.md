# Giải thích LAB3



##  Bài 1: Kiwi 

```python
import numpy as np
import scipy.ndimage as nd
import imageio.v2 as iio
import matplotlib.pylab as plt
```

➤ Khai báo các thư viện cần thiết.

```python
data = iio.imread('exercise/colorful-ripe-tropical-fruits.jpg')
kiwiImg = data[920:1100, 390:570]
```

➤ Đọc ảnh từ thư mục và cắt ảnh kiwi.

```python
kiwi_shifted = nd.shift(kiwi, (0, 30, 0))
```

➤ Dịch chuyển hình kiwi sang phải 30 pixel.

```python
output = data.copy()
output[920:1100, 390:570] = kiwi_shifted
```

➤ Dán ảnh đã dịch chuyển vào vị trí cũ.

```python
plt.imshow(output)
plt.show()
```

➤ Hiển thị ảnh kết quả.

##  Bài 2: Đổi màu Đu Đủ và Dưa Hấu

```python
data = iio.imread('exercise/colorful-ripe-tropical-fruits.jpg')
dudu = data[330:810, 125:670]
duahau = data[315:1100, 1635:2100]
```

➤ Cắt ảnh đu đủ và dưa hấu từ ảnh gốc.

```python
dudu_changed = dudu[:, :, ::-1]
duahau_changed = duahau[:, :, ::-1]
```

➤ Đổi màu bằng cách đảo kênh màu RGB thành BGR.

```python
plt.subplot(1, 2, 1)
plt.imshow(dudu_changed)
plt.subplot(1, 2, 2)
plt.imshow(duahau_changed)
plt.show()
```

➤ Hiển thị 2 ảnh kết quả cạnh nhau.

##  Bài 3: Xoay và Phản Chiếu

```python
data = iio.imread('exercise/quang_ninh.jpg')
nui = data[25:330, 445:655]
thuyen = data[455:545, 490:655]
```

➤ Cắt ảnh núi và thuyền từ ảnh gốc.

```python
nui_xoay = nd.rotate(nui, 45, reshape=True)
thuyen_xoay = nd.rotate(thuyen, 45, reshape=True)
```

➤ Xoay ảnh 45 độ, reshape=True giúp không bị cắt góc.

```python
plt.imsave('nui_xoay.jpg', nui_xoay)
plt.imsave('thuyen.jpg', thuyen_xoay)
```

➤ Lưu ảnh đã xoay vào máy.

```python
plt.subplot(1, 2, 1)
plt.imshow(nui_xoay)
plt.subplot(1, 2, 2)
plt.imshow(thuyen_xoay)
plt.show()
```

➤ Hiển thị ảnh sau biến đổi.

##  Bài 4: Zoom Chùa

```python
data = iio.imread('exercise/pagoda.jpg')
chua = data[125:320, 0:570]
```

➤ Cắt ảnh chùa từ ảnh gốc.

```python
chua_zoom = nd.zoom(chua, (5, 5, 1))
```

➤ Phóng to ảnh lên 5 lần.

```python
plt.imsave('chua_zoom.jpg', chua_zoom)
```

➤ Lưu ảnh phóng to vào máy.

```python
plt.subplot(1, 2, 1)
plt.imshow(chua)
plt.subplot(1, 2, 2)
plt.imshow(chua_zoom)
plt.show()
```

➤ Hiển thị ảnh gốc và ảnh đã zoom.

## 🎛️ Bài 5: Menu Biến Đổi Tương Tác

```python
img_folder = "exercise"
image_files = ["colorful-ripe-tropical-fruits.jpg", "pagoda.jpg", "quang_ninh.jpg"]
```

➤ Tạo danh sách ảnh để người dùng chọn.

```python
print("Chọn thao tác:")
print("T - Tịnh tiến")
print("X - Xoay")
print("P - Phóng to")
print("H - Thu nhỏ")
print("C - Coordinate Map")
choice = input("Nhập lựa chọn (T/X/P/H/C): ").upper()
```

➤ Nhập lựa chọn từ bàn phím.

```python
if choice not in ['T', 'X', 'P', 'H', 'C']:
    print("Lựa chọn không hợp lệ.")
    exit()
```

➤ Thoát nếu nhập sai.

```python
print("Chọn ảnh (1, 2 hoặc 3):")
img_index = int(input("Nhập số: ")) - 1
```

➤ Người dùng chọn ảnh.

```python
if img_index not in [0, 1, 2]:
    print("Số ảnh không hợp lệ.")
    exit()
img_path = os.path.join(img_folder, image_files[img_index])
img = iio.imread(img_path)
```

➤ Lấy đường dẫn ảnh và đọc vào.

```python
if choice == 'T':
    result = nd.shift(img, (0, 30, 0))
elif choice == 'X':
    result = nd.rotate(img, 45, reshape=True)
elif choice == 'P':
    result = nd.zoom(img, (5, 5, 1))
elif choice == 'H':
    result = nd.zoom(img, (1/5, 1/5, 1))
```

➤ Thực hiện thao tác biến đổi tương ứng.

```python
elif choice == 'C':
    H, W, C = img.shape
    M = np.indices((H, W))
    d = 5
    q = 2 * d * np.random.rand(*M.shape) - d
    mp = (M + q).astype(int)
    mp[0] = np.clip(mp[0], 0, H - 1)
    mp[1] = np.clip(mp[1], 0, W - 1)
    result = np.zeros_like(img)
    for i in range(C):
        result[:, :, i] = img[mp[0], mp[1], i]
```

➤ Coordinate mapping: tạo hiệu ứng nhiễu bằng cách thay đổi toạ độ pixel.

```python
plt.subplot(1, 2, 1)
plt.title("Ảnh Gốc")
plt.imshow(img)
plt.subplot(1, 2, 2)
plt.title("Ảnh Sau Biến Đổi")
plt.imshow(result)
plt.show()
```

➤ Hiển thị ảnh gốc và ảnh sau khi biến đổi.

