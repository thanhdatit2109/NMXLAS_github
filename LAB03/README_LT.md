✨ GIẢI THÍCH TừaNG ĐOẠN CODE - 5 BÀI Xử LÝ ẢNH

🌿 BÀI 1: Kiwi Wave Effect

kiwi = Image.open("kiwi.jpg")
kiwi_np = np.array(kiwi)

➤ Mở file ảnh kiwi và chuyển sang mảng NumPy.

def wave(coords):
    y, x = coords
    offset = 20.0 * np.sin(x / 30.0)
    return (y + offset, x)

➤ Định nghĩa hàm biến đổi tọa độ - tạo độ lệch theo hàm sin (hiệu ứng gợn sóng).

warped = geometric_transform(kiwi_np, wave, output_shape=kiwi_np.shape[:2])

➤ Áp dụng hàm wave cho tụng pixel để tạo hiệu ứng.

Image.fromarray(warped).save("kiwi_wave.jpg")

➤ Lưu kết quả đã biến đổi.

🍏 BÀI 2: Gradient Đu Đủ và Dưa Hấu

def apply_gradient_blended(image, color_start, color_end):
    arr = np.array(image)

➤ Chuyển ảnh RGBA sang NumPy.

    h, w = arr.shape[:2]
    alpha = arr[..., 3]
    rgb_original = arr[..., :3]

➤ Lấy chiều cao, rộng và tách kênh RGB, alpha.

    gradient = np.linspace(0, 1, h).reshape(h, 1, 1)

➤ Tạo gradient từ trên xuống dưới.

    rgb_gradient = ((1 - gradient) * color_start + gradient * color_end).astype(np.uint8)

➤ Tính toán màu theo gradient (nội suy từ 2 màu).

    blended = ((0.5 * rgb_original + 0.5 * rgb_gradient)).astype(np.uint8)

➤ Pha trộn màu gốc và gradient (50/50).

    rgba = np.concatenate([blended, alpha.reshape(h, w, 1)], axis=2)

➤ Ghép lại với kênh alpha.

🚣 BÀI 3: Xoay và Phản chiếu Núi/Thuyền

rotated = image.rotate(45, expand=False)

➤ Xoay 45° giữ nguyên kích thước.

mirrored = rotated.transpose(Image.FLIP_TOP_BOTTOM)

➤ Tạo ảnh phản chiếu dọc.

canvas = Image.new("RGB", (w, h), (255, 255, 255))
canvas.paste(rotated, (0, 0))
canvas.paste(mirrored, (0, rotated.height))

➤ Ghép 2 ảnh lên nền trắng.

⛩️ BÀI 4: Warping Chùa

pagoda_big = pagoda.resize((w*5, h*5))

➤ Phóng to ảnh lên 5 lần.

def warp(coords):
    y, x = coords
    return (y + 20 * np.sin(x / 50.0), x)

➤ Tạo hàm biến đổi sin theo chiều ngang (uốn).

warped = geometric_transform(arr, warp, output_shape=(h, w))

➤ Áp dụng biến đổi hình học.

🎨 BÀI 5: Menu Biến Đổi Tương Tác

print("1. Tinh tien\n2. Xoay\n3. Zoom ...")
choice = input("Nhap lua chon: ")

➤ Menu chọn lệnh tương tác.

if choice == "1":
    dx = int(input("Nhap dx: "))
    dy = int(input("Nhap dy: "))
    img = ImageChops.offset(img, dx, dy)

➤ Xử lý tịnh tiến.

elif choice == "2":
    angle = float(input("Nhap goc: "))
    reshape = input("Mo rong? y/n: ") == 'y'
    img = img.rotate(angle, expand=reshape)

➤ Xoay theo góc nhập.

elif choice == "4":
    sigma = float(input("Nhap sigma: "))
    img = Image.fromarray(gaussian_filter(np.array(img), sigma))

➤ Áp dụng làm mờ Gaussian.