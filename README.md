# 🐦 Jumping Bird (flapping-bird)

Game kiểu Flappy Bird viết bằng **thuần JavaScript + HTML5 Canvas**, gói trong **một file `index.html`** (~45KB), không cần thư viện hay file âm thanh ngoài.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green)

## ✨ Tính năng

- 🎮 Plug & play: mở `index.html` bằng trình duyệt là chơi
- 🎨 5 skin nhân vật (Yellow, Red, Blue, Green, Purple)
- ⚡ 3 mức độ khó: Easy / Normal / Hard
- 🌅 Chu kỳ ngày/đêm, mặt trời, mặt trăng, sao
- 🏙️ Nền thành phố parallax 2 lớp
- 🔊 Âm thanh & nhạc nền 8-bit sinh bằng Web Audio API
- 📱 Responsive, điều khiển bằng chuột / phím / cảm ứng
- 🏆 Lưu điểm cao nhất bằng `localStorage`

## 🚀 Chạy game

```bash
git clone https://github.com/youngboi03/flapping-bird.git
cd flapping-bird
# mở index.html bằng trình duyệt, hoặc chạy server tĩnh:
python3 -m http.server 8000   # rồi vào http://localhost:8000
```

**Điều khiển:** `Space` / `↑` / click chuột / chạm màn hình để bay. `Esc` đóng menu.

Có thể bật **GitHub Pages** (Settings → Pages → Deploy from branch → `main` / root) để chơi online.

## ⚡ Các tối ưu so với bản gốc (v2.2.0 → v3.0.0)

| Vấn đề cũ | Cách xử lý |
|-----------|------------|
| Tốc độ game phụ thuộc tần số quét (màn 144Hz chạy nhanh ~2.4 lần) | Vòng lặp **fixed-timestep 60Hz** với accumulator; giới hạn bước thời gian khi tab bị ẩn |
| Vẽ lại chim bằng 5 `fillRect` + stroke mỗi frame | **Cache sprite** từng skin vào offscreen canvas, mỗi frame chỉ `drawImage` |
| Vẽ ~20 đường vân đất mỗi frame | **Texture mặt đất** vẽ sẵn, chỉ `drawImage` |
| `Math.sin` cho từng tòa nhà mỗi frame | **Bảng tòa nhà tính sẵn** (ring 1024 phần tử) |
| Tạo chuỗi màu `rgb()` + object theme mỗi frame | Chỉ **tính lại màu khi hệ số ngày/đêm đổi**, cache kết quả |
| Tạo object hitbox mới cho mỗi ống mỗi frame | Va chạm **inline, không cấp phát**; thoát sớm khi chết |
| `Array.splice` / `ctx.save/restore` trong particle | `swap-remove`, giới hạn tối đa 60 hạt, không save/restore |
| `clearRect` + vẽ nền phủ kín | Bỏ `clearRect`, canvas `alpha:false` |
| `mousedown` + `touchstart` riêng | Một sự kiện **`pointerdown`** |
| `AudioContext` tạo ngay khi tải trang, `getPlaylists()` tạo mảng mới mỗi lần | Tạo **lười** sau cử chỉ người dùng; playlist tạo một lần; ngắt node khi xong |
| `@import` font chặn render | `<link rel="preconnect">` + `display=swap` |

### 🐛 Lỗi đã sửa

- Click vào hộp thoại SKINS/LEVEL làm game bắt đầu ngoài ý muốn
- Giữ phím Space làm chim flap liên tục (key repeat)
- `localStorage` bị chặn làm game không chạy
- Mỗi lần resize canvas bị reset (clear + mất cấu hình)
- Nhạc tiếp tục chạy khi chuyển sang tab khác
- Nút độ khó mặc định chưa hiển thị trạng thái "selected"

## ⚙️ Tùy chỉnh

Mở `index.html`, tìm đối tượng `CONFIG`:

```javascript
const CONFIG = {
    gravity: 0.25,       // trọng lực
    jumpStrength: -4.5,  // lực nhảy (số âm = bay lên)
    pipeSpeed: 2,        // tốc độ ống
    difficulties: { /* gap, spawnRate */ },
    characters: [ /* thêm skin: { name, body, outline, wing } */ ]
};
```

## 📁 Cấu trúc

```
flapping-bird/
├── index.html   # toàn bộ game (CSS + HTML + JS)
├── README.md
├── LICENSE      # MIT
└── .gitignore
```

Trong `index.html`, phần JS gồm: `CONFIG` → `Utils` → `AudioSys` → `BirdSprite` → `Bird` → `Pipes` → `Particles` → `Environment` → `Game`.

## 📜 License

[MIT](LICENSE)
