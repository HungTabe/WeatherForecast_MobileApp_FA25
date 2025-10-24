# Weather Forecast Mobile App 🌤️

Ứng dụng dự báo thời tiết hiện đại được xây dựng với React, TypeScript và Tailwind CSS. Thiết kế mobile-first với giao diện đẹp và thân thiện với người dùng.

## ✨ Tính năng chính

### 🌡️ Thời tiết hiện tại
- Hiển thị nhiệt độ, trạng thái thời tiết, độ ẩm, tốc độ gió
- Cảm giác như (feels like) và tầm nhìn
- Hỗ trợ vị trí hiện tại qua GPS

### ⏰ Dự báo theo giờ
- Dự báo 24 giờ tiếp theo
- Nhiệt độ, trạng thái và xác suất mưa
- Giao diện cuộn ngang trực quan

### 📅 Dự báo 7 ngày
- Nhiệt độ cao/thấp hàng ngày
- Trạng thái thời tiết và xác suất mưa
- Hiển thị theo thứ và ngày

### 🔍 Tìm kiếm thành phố
- Tìm kiếm nhanh với autocomplete
- Hỗ trợ các thành phố Việt Nam và quốc tế
- Thêm vào danh sách yêu thích

### ⭐ Quản lý yêu thích
- Lưu tối đa 10 thành phố yêu thích
- Đặt thành phố mặc định
- Xóa và sắp xếp lại thứ tự

### ⚙️ Cài đặt tùy chỉnh
- Đổi đơn vị nhiệt độ (°C/°F)
- Đổi đơn vị tốc độ gió (km/h, m/s, mph)
- Theme sáng/tối/tự động
- Hỗ trợ tiếng Việt và tiếng Anh

### 🔄 Làm việc offline
- Lưu cache dữ liệu thời tiết
- Hiển thị dữ liệu gần nhất khi offline
- Đồng bộ tự động khi có mạng

## 🛠️ Công nghệ sử dụng

- **React 18** với hooks và TypeScript
- **Tailwind CSS** cho styling hiện đại
- **Shadcn/ui** components
- **Lucide React** icons
- **LocalStorage** cho cache và settings
- **Geolocation API** cho vị trí hiện tại
- **OpenWeatherMap API** (demo với mock data)

## 🗃️ Tóm tắt schema

- `locations` – lưu toạ độ, timezone, cờ `is_current_location`
- `favorites` – danh sách yêu thích (có `sort_order`)
- `search_history` – lịch sử gõ tìm kiếm
- `weather_current` – thời tiết hiện tại (1 dòng/location, `ON CONFLICT REPLACE`)
- `weather_hourly` – dự báo theo giờ (unique theo `location_id, ts`)
- `weather_daily` – dự báo theo ngày (unique theo `location_id, date_ts`)
- `alert_rules`, `alert_events` – quy tắc & log cảnh báo
- `app_settings` – cặp key/value cài đặt app
- `api_cache` – cache raw JSON (nếu muốn lưu phản hồi API)
- Views: `v_favorites_current`, `v_hourly_upcoming`, `v_daily_upcoming`

> Gốc thời gian dùng **epoch seconds**; bật **FOREIGN KEY** bằng PRAGMA.

---

## 🚀 Chạy dự án

### Yêu cầu
- Android Studio mới nhất
- JDK 11 hoặc 17
- AVD **Android 13/14 (API 33/34 – Google APIs, x86_64)** khuyến nghị

### Bước chạy nhanh
1. Mở project bằng Android Studio.
2. Tạo AVD (Tools → Device Manager) → Pixel 6/7, API 33+.
3. **Run (▶)** để cài app.
   - Lần đầu mở app, DB sẽ được tạo từ `assets/sql/weather_schema.sql`.
4. Mở **View → Tool Windows → App Inspection → Databases** để xem `weather.db`.

> Nếu dùng **Debug (🐞)** và app đứng ở log "Waiting for debugger…", hãy chạy bằng **Run (▶)** hoặc tắt "Wait for debugger" trong Developer Options của emulator.

---

## 🧪 Lưu ý về FTS5

Schema có bảng FTS5 `places_fts`. Một số emulator/ROM không bật module `fts5`, khi đó:
- Code đã bọc `CREATE VIRTUAL TABLE … fts5` trong `try/catch` để **không crash**.
- Nếu muốn **bật FTS5 thật**:
  - Dùng emulator **API 33/34 – Google APIs (x86_64)**, **Cold Boot** lại.
  - Hoặc comment 2 dòng FTS5 trong `weather_schema.sql` nếu không dùng search offline.

---

## 🔧 Câu lệnh Gradle

Build debug:
```bash
./gradlew assembleDebug
```

## 🔧 Cấu hình API

### Sử dụng OpenWeatherMap (khuyến nghị)

1. Đăng ký tài khoản miễn phí tại [OpenWeatherMap](https://openweathermap.org/api)
2. Lấy API key miễn phí
3. Cập nhật `src/services/weatherService.ts`:

```typescript
const API_KEY = 'your_api_key_here'; // Thay thế 'demo'
```

### Sử dụng Open-Meteo (không cần API key)

Open-Meteo cung cấp API hoàn toàn miễn phí không cần đăng ký. Bạn có thể tích hợp bằng cách:

```typescript
// Trong weatherService.ts
const OPEN_METEO_URL = 'https://api.open-meteo.com/v1';
```

## 📱 Chuyển đổi thành Mobile App

Ứng dụng này được thiết kế mobile-first và có thể dễ dàng chuyển đổi thành ứng dụng mobile native sử dụng:

### Capacitor (khuyến nghị)
```bash
npm install @capacitor/core @capacitor/cli
npx cap init
npx cap add android
npx cap add ios
npx cap run android
```

### PWA (Progressive Web App)
Ứng dụng đã được tối ưu để chạy như PWA với:
- Responsive design
- Offline support
- Mobile-friendly navigation

## 🎨 Thiết kế & UX

### Design System
- **Colors**: Sky blue theme với gradient đẹp mắt
- **Typography**: Clean và dễ đọc
- **Animation**: Smooth transitions và micro-interactions
- **Mobile-first**: Optimized cho điện thoại trước

### Theme Support
- **Light mode**: Giao diện sáng với gradient xanh dương
- **Dark mode**: Giao diện tối với màu night sky
- **System**: Tự động theo cài đặt hệ thống

## 📁 Cấu trúc dự án

```
src/
├── components/
│   ├── weather/           # Weather components
│   ├── layout/            # Layout components  
│   └── ui/                # Shadcn UI components
├── services/              # API services
├── hooks/                 # Custom hooks
├── assets/                # Images and static files
└── pages/                 # Main pages
```

## 🔒 Quyền truy cập

### Location Permission
Ứng dụng yêu cầu quyền truy cập vị trí để:
- Lấy thời tiết vị trí hiện tại
- Cung cấp dự báo chính xác nhất

Nếu từ chối, ứng dụng sẽ sử dụng thành phố mặc định (TP.HCM).

## 💾 Lưu trữ dữ liệu

### LocalStorage
- **Favorites**: Danh sách thành phố yêu thích
- **Settings**: Cài đặt người dùng
- **Cache**: Dữ liệu thời tiết gần nhất

### TTL (Time To Live)
- **Current weather**: 90 phút
- **Daily forecast**: 6 giờ
- Tự động làm mới khi hết hạn

## 🌐 Hỗ trợ ngôn ngữ

- **Tiếng Việt** (mặc định)
- **English** 
- Sẵn sàng mở rộng cho các ngôn ngữ khác

## 📊 Tính năng nâng cao (Có thể mở rộng)

- [ ] Push notifications cho cảnh báo thời tiết
- [ ] Widget màn hình chính
- [ ] Biểu đồ chi tiết (áp suất, UV index)
- [ ] Radar thời tiết
- [ ] Social sharing
- [ ] Voice commands

## 🤝 Đóng góp

Chào mừng các đóng góp! Vui lòng:

1. Fork repository
2. Tạo feature branch
3. Commit changes
4. Push và tạo Pull Request

## 📄 License

MIT License - xem file LICENSE để biết thêm chi tiết.

## 👥 Team

Được phát triển bởi nhóm Weather Team với ❤️

---

**Lưu ý**: Đây là phiên bản demo sử dụng mock data. Để sử dụng trong production, vui lòng cấu hình API key thật từ OpenWeatherMap hoặc Open-Meteo.