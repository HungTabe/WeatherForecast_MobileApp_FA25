# 📄 Software Requirements Specification (SRS)
## Màn hình Dự Báo Theo Giờ (Hourly Forecast)

**Dự án:** Weather Forecast Mobile App  
**Phiên bản:** 1.0  
**Ngày:** 24/10/2025  
**Tác giả:** Weather Team

---

## 1. Giới thiệu

### 1.1 Mục đích
Tài liệu này mô tả chi tiết các yêu cầu chức năng và phi chức năng cho màn hình **Dự báo theo giờ** (Hourly Forecast) trong ứng dụng Weather Forecast Mobile App.

### 1.2 Phạm vi
Màn hình Dự báo theo giờ cung cấp thông tin thời tiết chi tiết cho 24-48 giờ tiếp theo, giúp người dùng lập kế hoạch cho các hoạt động trong ngày.

### 1.3 Định nghĩa và từ viết tắt
- **SRS**: Software Requirements Specification
- **UI/UX**: User Interface/User Experience
- **API**: Application Programming Interface
- **GPS**: Global Positioning System
- **PWA**: Progressive Web App

### 1.4 Tài liệu tham khảo
- README.md - Tổng quan dự án
- OpenWeatherMap API Documentation
- Material Design Guidelines
- WCAG 2.1 Accessibility Guidelines

---

## 2. Mô tả tổng quan

### 2.1 Bối cảnh sản phẩm
Màn hình Dự báo theo giờ là một trong các màn hình chính của ứng dụng Weather Forecast, được truy cập từ bottom navigation bar. Màn hình này bổ sung thông tin chi tiết hơn cho màn hình Thời tiết hiện tại và phục vụ nhu cầu lập kế hoạch ngắn hạn của người dùng.

### 2.2 Chức năng sản phẩm
- Hiển thị dự báo thời tiết theo từng giờ
- Cung cấp thông tin nhiệt độ, điều kiện thời tiết, xác suất mưa
- Hỗ trợ cuộn để xem dữ liệu 24-48 giờ
- Tự động cập nhật dữ liệu khi thay đổi vị trí
- Lưu cache để xem offline

### 2.3 Đặc điểm người dùng
- **Người dùng chính**: Cá nhân cần lập kế hoạch hoạt động trong ngày
- **Độ tuổi**: 16-65+
- **Kỹ năng**: Cơ bản đến trung bình về sử dụng smartphone
- **Tần suất sử dụng**: Hàng ngày, nhiều lần/ngày

### 2.4 Ràng buộc
- Phải hoạt động trên thiết bị mobile (iOS, Android)
- Kích thước màn hình tối thiểu: 320px width
- Yêu cầu kết nối internet để cập nhật dữ liệu mới
- Hỗ trợ offline mode với dữ liệu cache

### 2.5 Giả định và phụ thuộc
- Người dùng đã cấp quyền truy cập vị trí (hoặc đã chọn thành phố)
- API thời tiết (OpenWeatherMap) hoạt động ổn định
- Thiết bị có đủ bộ nhớ để cache dữ liệu

---

## 3. Yêu cầu chức năng

### 3.1 Hiển thị dữ liệu theo giờ

#### 3.1.1 Mô tả
Màn hình hiển thị danh sách các khung giờ với thông tin thời tiết chi tiết.

#### 3.1.2 Đầu vào
- Tọa độ GPS của vị trí hiện tại (lat, lon)
- Hoặc tọa độ của thành phố được chọn
- Số lượng giờ cần hiển thị (24 hoặc 48)

#### 3.1.3 Xử lý
1. Gọi API `weatherService.getHourlyForecast(lat, lon)`
2. Parse và format dữ liệu nhận được
3. Render danh sách theo thứ tự thời gian
4. Lưu vào cache với TTL 90 phút

#### 3.1.4 Đầu ra
Danh sách các card hiển thị thông tin:
- **Thời gian**: Định dạng "HH:00" (ví dụ: "14:00") hoặc "Bây giờ" cho giờ hiện tại
- **Nhiệt độ**: Số nguyên + ký hiệu độ (ví dụ: "28°")
- **Icon thời tiết**: Biểu tượng động phù hợp với điều kiện
- **Xác suất mưa**: Phần trăm (ví dụ: "60%") - chỉ hiển thị nếu > 0%

#### 3.1.5 Tiêu chí chấp nhận
- [ ] Hiển thị đủ 24 giờ cho gói cơ bản
- [ ] Có thể mở rộng lên 48 giờ
- [ ] Thời gian được hiển thị theo múi giờ địa phương
- [ ] Icon thời tiết chính xác với từng điều kiện
- [ ] Performance: Render dưới 1 giây

### 3.2 Thông tin header

#### 3.2.1 Mô tả
Hiển thị thông tin ngữ cảnh cho dữ liệu đang xem.

#### 3.2.2 Nội dung hiển thị
- **Tiêu đề**: "Dự báo theo giờ"
- **Vị trí**: Tên thành phố/khu vực đang xem
- **Ngày**: "Hôm nay" hoặc ngày cụ thể

#### 3.2.3 Tiêu chí chấp nhận
- [ ] Header cố định ở đầu màn hình
- [ ] Tên vị trí chính xác với vị trí được chọn
- [ ] Responsive trên mọi kích thước màn hình

### 3.3 Tương tác người dùng

#### 3.3.1 Cuộn danh sách
- **Hành động**: Vuốt dọc để xem các giờ tiếp theo
- **Yêu cầu**: Smooth scrolling, không lag
- **Hiệu ứng**: Fade-in animation khi card xuất hiện

#### 3.3.2 Làm mới dữ liệu
- **Hành động**: Pull-to-refresh từ đầu danh sách
- **Yêu cầu**: Hiển thị loading indicator
- **Kết quả**: Cập nhật dữ liệu mới nhất từ API

#### 3.3.3 Xem chi tiết (optional)
- **Hành động**: Tap vào card giờ cụ thể
- **Kết quả**: Hiển thị dialog/bottom sheet với thông tin chi tiết
  - Nhiệt độ cảm nhận
  - Tốc độ gió
  - Độ ẩm
  - Áp suất khí quyển
  - UV index (nếu có)

#### 3.3.4 Tiêu chí chấp nhận
- [ ] Cuộn mượt mà, không giật lag
- [ ] Pull-to-refresh hoạt động trên cả iOS và Android
- [ ] Tap vào card có phản hồi trực quan (hover effect)

### 3.4 Hiển thị điều kiện đặc biệt

#### 3.4.1 Icon thời tiết động
Hệ thống icon phải phản ánh chính xác điều kiện:
- **Sunny/Clear**: ☀️ Mặt trời
- **Partly Cloudy**: ⛅ Mây ít
- **Cloudy**: ☁️ Nhiều mây
- **Rainy**: 🌧️ Mưa
- **Drizzle**: 🌦️ Mưa phùn
- **Thunderstorm**: ⛈️ Giông bão
- **Snow**: ❄️ Tuyết (nếu áp dụng)

#### 3.4.2 Màu sắc tùy chỉnh
- Xác suất mưa cao (>70%): Màu xanh dương đậm
- Nhiệt độ cao (>35°C): Màu đỏ/cam
- Nhiệt độ thấp (<15°C): Màu xanh lạnh

#### 3.4.3 Tiêu chí chấp nhận
- [ ] Icon chính xác 100% với API response
- [ ] Màu sắc hợp lý, dễ phân biệt
- [ ] Tương thích với cả light mode và dark mode

### 3.5 Cache và Offline Support

#### 3.5.1 Cơ chế cache
- **Storage**: LocalStorage hoặc IndexedDB
- **Key**: `weather_hourly_{location_id}`
- **TTL**: 90 phút
- **Size**: Tối đa 5 vị trí gần nhất

#### 3.5.2 Offline behavior
- Hiển thị dữ liệu cache kèm timestamp
- Thông báo "Dữ liệu được cập nhật lúc HH:MM"
- Disable pull-to-refresh khi offline
- Icon/badge chỉ báo trạng thái offline

#### 3.5.3 Tiêu chí chấp nhận
- [ ] Dữ liệu cache hiển thị ngay lập tức (<200ms)
- [ ] Người dùng biết rõ dữ liệu đang offline
- [ ] Tự động sync khi có kết nối trở lại

---

## 4. Yêu cầu phi chức năng

### 4.1 Performance

#### 4.1.1 Thời gian tải
- **First Load**: < 2 giây (kể cả API call)
- **Cached Load**: < 500ms
- **Scroll FPS**: ≥ 60 FPS

#### 4.1.2 Memory Usage
- **Maximum**: 50 MB
- **Typical**: 20-30 MB

#### 4.1.3 Battery Impact
- Minimal - tối ưu re-render
- Không polling liên tục

### 4.2 Usability

#### 4.2.1 Accessibility
- [ ] Hỗ trợ screen reader
- [ ] Contrast ratio ≥ 4.5:1 (WCAG AA)
- [ ] Font size có thể scale (accessibility settings)
- [ ] Touch target ≥ 44x44 points

#### 4.2.2 Responsive Design
- **Mobile Portrait**: 320px - 428px
- **Mobile Landscape**: 568px - 926px
- **Tablet**: 768px - 1024px
- **Desktop**: 1024px+ (preview mode)

#### 4.2.3 Internationalization
- Hỗ trợ tiếng Việt và tiếng Anh
- Format thời gian theo locale
- Format nhiệt độ theo cài đặt (°C/°F)

### 4.3 Reliability

#### 4.3.1 Error Handling
- **Network Error**: Hiển thị thông báo thân thiện, đề xuất dùng dữ liệu cache
- **API Error**: Retry tự động 3 lần với exponential backoff
- **Parse Error**: Log error, hiển thị dữ liệu fallback

#### 4.3.2 Uptime
- **Target**: 99.5% (phụ thuộc vào API provider)

#### 4.3.3 Data Accuracy
- Dữ liệu phải chính xác với API source
- Không làm tròn quá mức (tối đa 1 chữ số thập phân)

### 4.4 Security

#### 4.4.1 Data Privacy
- Không lưu trữ vị trí GPS lâu dài
- Cache data không chứa thông tin nhạy cảm
- Tuân thủ GDPR (nếu có người dùng EU)

#### 4.4.2 API Security
- API key được bảo vệ (environment variables)
- HTTPS cho mọi request

### 4.5 Maintainability

#### 4.5.1 Code Quality
- TypeScript strict mode
- ESLint + Prettier
- Unit test coverage ≥ 70%

#### 4.5.2 Documentation
- JSDoc cho functions quan trọng
- README cho component

---

## 5. Thiết kế giao diện (UI Specifications)

### 5.1 Layout

#### 5.1.1 Cấu trúc
```
┌─────────────────────────┐
│  [Header]               │
│  Dự báo theo giờ        │
│  Hà Nội • Hôm nay       │
├─────────────────────────┤
│                         │
│  ┌───────────────────┐  │
│  │ Bây giờ  ☁️  28° │  │ <- Card 1
│  │           10%     │  │
│  └───────────────────┘  │
│                         │
│  ┌───────────────────┐  │
│  │ 14:00    ☀️  29° │  │ <- Card 2
│  │            5%     │  │
│  └───────────────────┘  │
│                         │
│  ... (scrollable)       │
│                         │
├─────────────────────────┤
│  [Bottom Navigation]    │
└─────────────────────────┘
```

#### 5.1.2 Spacing & Padding
- **Container padding**: 16px (mobile), 24px (tablet+)
- **Card gap**: 8px - 12px
- **Card padding**: 12px (mobile), 16px (tablet+)

### 5.2 Typography

#### 5.2.1 Header
- **Title**: 24px (mobile), 32px (tablet), font-weight: 700
- **Subtitle**: 14px, font-weight: 400, color: muted-foreground

#### 5.2.2 Card Content
- **Time**: 14px (mobile), 16px (tablet), font-weight: 500
- **Temperature**: 20px (mobile), 24px (tablet), font-weight: 600
- **Precipitation**: 12px, font-weight: 500, color: primary

### 5.3 Colors (Semantic Tokens)

#### 5.3.1 Light Mode
- **Background**: `hsl(var(--background))`
- **Card**: `hsl(var(--card))`
- **Text**: `hsl(var(--foreground))`
- **Primary**: `hsl(var(--primary))` - Sky blue theme

#### 5.3.2 Dark Mode
- **Background**: `hsl(var(--background))` - Dark navy
- **Card**: `hsl(var(--card))` - Slightly lighter
- **Text**: `hsl(var(--foreground))` - Near white

### 5.4 Animations

#### 5.4.1 Card Entrance
```css
animation: fade-in 0.3s ease-out;
animation-delay: ${index * 0.05}s;
```

#### 5.4.2 Hover Effect
```css
transition: all 0.2s ease;
transform: scale(1.02);
box-shadow: 0 4px 12px rgba(0,0,0,0.1);
```

#### 5.4.3 Loading State
- Skeleton screens cho cards
- Shimmer effect khi đang load

---

## 6. Tích hợp API

### 6.1 Endpoint

#### 6.1.1 Get Hourly Forecast
```typescript
weatherService.getHourlyForecast(lat: number, lon: number): Promise<HourlyData[]>
```

#### 6.1.2 Response Schema
```typescript
interface HourlyData {
  time: string;           // Format: "HH:00"
  temperature: number;    // Celsius
  condition: string;      // 'clear' | 'cloudy' | 'rain' | etc.
  precipitation: number;  // 0-100 (%)
}
```

### 6.2 Error Cases

#### 6.2.1 Network Error (No Connection)
```json
{
  "error": "NETWORK_ERROR",
  "message": "Không có kết nối internet",
  "fallback": "cache"
}
```

#### 6.2.2 API Error (Server 500)
```json
{
  "error": "API_ERROR",
  "message": "Lỗi từ server thời tiết",
  "retry": true
}
```

#### 6.2.3 Invalid Location
```json
{
  "error": "INVALID_LOCATION",
  "message": "Không tìm thấy dữ liệu cho vị trí này"
}
```

### 6.3 Rate Limiting
- **Free tier**: 1000 calls/day
- **Implementation**: Queue requests, show cached data khi rate limit

---

## 7. Database Schema (SQLite)

### 7.1 Table: weather_hourly

```sql
CREATE TABLE weather_hourly (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  location_id INTEGER NOT NULL,
  ts INTEGER NOT NULL,           -- Unix timestamp (epoch seconds)
  temperature REAL NOT NULL,
  feels_like REAL,
  condition TEXT NOT NULL,
  precipitation REAL DEFAULT 0,
  humidity INTEGER,
  wind_speed REAL,
  wind_direction INTEGER,
  pressure REAL,
  uv_index REAL,
  created_at INTEGER DEFAULT (strftime('%s', 'now')),
  
  FOREIGN KEY (location_id) REFERENCES locations(id) ON DELETE CASCADE,
  UNIQUE(location_id, ts)
);

CREATE INDEX idx_hourly_location_ts ON weather_hourly(location_id, ts);
```

### 7.2 View: v_hourly_upcoming

```sql
CREATE VIEW v_hourly_upcoming AS
SELECT 
  h.*,
  l.city_name,
  l.country_code,
  datetime(h.ts, 'unixepoch', 'localtime') as local_time
FROM weather_hourly h
JOIN locations l ON h.location_id = l.id
WHERE h.ts >= strftime('%s', 'now')
ORDER BY h.ts ASC
LIMIT 48;
```

### 7.3 Cache Strategy
- **Insert**: `INSERT OR REPLACE INTO weather_hourly ...`
- **Cleanup**: Xóa dữ liệu cũ hơn 48 giờ
- **Query**: Chỉ lấy dữ liệu `ts >= current_time`

---

## 8. Test Cases

### 8.1 Functional Tests

#### TC-01: Hiển thị dữ liệu cơ bản
- **Precondition**: Có kết nối internet, vị trí được xác định
- **Steps**:
  1. Mở app
  2. Navigate đến tab "Dự báo theo giờ"
- **Expected**: Hiển thị 24 cards với đầy đủ thông tin

#### TC-02: Pull to refresh
- **Precondition**: Đang ở màn Hourly
- **Steps**:
  1. Kéo xuống từ đầu danh sách
  2. Thả tay
- **Expected**: Loading indicator → Dữ liệu cập nhật

#### TC-03: Offline mode
- **Precondition**: Đã có cache data, tắt internet
- **Steps**:
  1. Navigate đến Hourly
- **Expected**: Hiển thị cache + thông báo offline + timestamp

#### TC-04: Thay đổi vị trí
- **Precondition**: Đang xem Hà Nội
- **Steps**:
  1. Chuyển sang TP.HCM từ Favorites
- **Expected**: Dữ liệu cập nhật cho TP.HCM

### 8.2 Performance Tests

#### TC-05: First load performance
- **Steps**: Đo thời gian từ mount đến render hoàn tất
- **Expected**: < 2 seconds

#### TC-06: Scroll performance
- **Steps**: Scroll nhanh qua 24 cards
- **Expected**: 60 FPS, không drop frames

### 8.3 UI/UX Tests

#### TC-07: Responsive design
- **Steps**: Test trên 320px, 375px, 414px, 768px
- **Expected**: Layout không bị vỡ, text không bị cắt

#### TC-08: Dark mode
- **Steps**: Chuyển đổi theme dark/light
- **Expected**: Colors thay đổi, contrast đảm bảo

#### TC-09: Accessibility
- **Steps**: Bật VoiceOver/TalkBack
- **Expected**: Screen reader đọc được tất cả thông tin

---

## 9. Triển khai kỹ thuật

### 9.1 Component Structure

```
src/pages/Hourly.tsx              # Main page
src/components/weather/
  ├── HourlyForecast.tsx          # Reusable component
  ├── HourlyCard.tsx              # Individual hour card (new)
  ├── WeatherIcon.tsx             # Icon component
  └── LoadingSkeleton.tsx         # Loading state (new)
```

### 9.2 State Management

#### 9.2.1 Local State (useState)
```typescript
const [hourlyData, setHourlyData] = useState<HourlyData[]>([]);
const [loading, setLoading] = useState(false);
const [error, setError] = useState<string | null>(null);
const [lastUpdated, setLastUpdated] = useState<Date | null>(null);
```

#### 9.2.2 Cache State (LocalStorage)
```typescript
const CACHE_KEY = 'weather_hourly_cache';
const CACHE_TTL = 90 * 60 * 1000; // 90 minutes

const getCachedData = () => {
  const cached = localStorage.getItem(CACHE_KEY);
  if (!cached) return null;
  
  const { data, timestamp } = JSON.parse(cached);
  if (Date.now() - timestamp > CACHE_TTL) return null;
  
  return data;
};
```

### 9.3 Data Flow

```
User opens Hourly screen
         ↓
Check cache (LocalStorage)
         ↓
    ┌────┴────┐
    │         │
  Valid?    Invalid/Empty
    │         │
    │    Fetch from API
    │         │
    │    Update cache
    │         │
    └────┬────┘
         ↓
   Render data
         ↓
   Start 90min timer
         ↓
   Auto refresh when expired
```

### 9.4 Optimization Techniques

#### 9.4.1 Virtual Scrolling (Optional)
- Chỉ render 10-15 cards visible
- Lazy load cards khi scroll

#### 9.4.2 Memoization
```typescript
const MemoizedHourlyCard = React.memo(HourlyCard);
```

#### 9.4.3 Image Optimization
- SVG icons (nhỏ, scalable)
- Icon sprite sheet nếu dùng raster images

---

## 10. Roadmap & Future Enhancements

### 10.1 Phase 1 (MVP) - Current
- [x] Hiển thị 24 giờ cơ bản
- [x] Pull to refresh
- [x] Cache & offline support
- [x] Responsive design

### 10.2 Phase 2 (Enhancements)
- [ ] Mở rộng lên 48 giờ
- [ ] Chi tiết khi tap vào card (bottom sheet)
- [ ] Biểu đồ nhiệt độ (line chart)
- [ ] Cảnh báo thời tiết đặc biệt (in-line alerts)

### 10.3 Phase 3 (Advanced)
- [ ] So sánh nhiều vị trí (split view)
- [ ] Notifications cho giờ cụ thể
- [ ] Export dữ liệu (PDF/CSV)
- [ ] Widget home screen (mobile native)

---

## 11. Acceptance Criteria (Tổng kết)

### 11.1 Functional
- ✅ Hiển thị đầy đủ 24 giờ với thời gian, nhiệt độ, icon, xác suất mưa
- ✅ Pull-to-refresh cập nhật dữ liệu thành công
- ✅ Offline mode hiển thị cache kèm timestamp
- ✅ Thay đổi vị trí cập nhật dữ liệu tương ứng
- ✅ Error handling thân thiện với người dùng

### 11.2 Non-Functional
- ✅ Load time < 2s (first load), < 500ms (cached)
- ✅ 60 FPS khi scroll
- ✅ Responsive trên 320px - 1024px+
- ✅ WCAG AA compliance (contrast, accessibility)
- ✅ Cache TTL 90 phút hoạt động chính xác

### 11.3 Quality
- ✅ Zero crash rate
- ✅ Unit test coverage ≥ 70%
- ✅ Code review passed
- ✅ QA testing passed

---

## 12. Dependencies & External Resources

### 12.1 NPM Packages
- `react` ^18.3.1
- `react-router-dom` ^6.30.1
- `lucide-react` ^0.462.0 (icons)
- `date-fns` ^3.6.0 (time formatting)
- `@radix-ui/react-scroll-area` (smooth scrolling)

### 12.2 External APIs
- **OpenWeatherMap API** (hoặc Open-Meteo)
  - Endpoint: `/forecast/hourly`
  - Rate limit: 1000 calls/day (free tier)

### 12.3 Browser/Device APIs
- Geolocation API (optional)
- LocalStorage API
- Network Information API (để detect offline)

---

## 13. Risks & Mitigations

### 13.1 API Rate Limiting
- **Risk**: Vượt quá 1000 calls/day
- **Mitigation**: 
  - Cache 90 phút
  - Queue requests
  - Alert user nếu đạt limit

### 13.2 Poor Network Performance
- **Risk**: Load chậm trên 3G
- **Mitigation**:
  - Hiển thị skeleton screens
  - Timeout sau 10s
  - Fallback to cache

### 13.3 Battery Drain
- **Risk**: Polling liên tục drain pin
- **Mitigation**:
  - Chỉ refresh khi user mở app
  - Không background polling
  - Debounce user interactions

---

## 14. Sign-off

### 14.1 Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | [Name] | ________ | __/__/__ |
| Lead Developer | [Name] | ________ | __/__/__ |
| QA Lead | [Name] | ________ | __/__/__ |
| UX Designer | [Name] | ________ | __/__/__ |

### 14.2 Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 24/10/2025 | Weather Team | Initial SRS document |

---

**End of Document**

---

## Phụ lục A: Mockups & Wireframes

*(Đính kèm file Figma hoặc ảnh mockup)*

## Phụ lục B: API Response Examples

```json
// Success Response
{
  "success": true,
  "data": [
    {
      "time": "14:00",
      "temperature": 29,
      "condition": "sunny",
      "precipitation": 5
    },
    // ... 23 items nữa
  ],
  "metadata": {
    "location": "Hà Nội",
    "timezone": "Asia/Ho_Chi_Minh",
    "generated_at": "2025-10-24T13:00:00Z"
  }
}
```

## Phụ lục C: Glossary

- **Pull-to-refresh**: Gesture kéo xuống để làm mới dữ liệu
- **TTL**: Time To Live - thời gian tồn tại của cache
- **Skeleton screen**: Placeholder UI khi đang load
- **Responsive**: Giao diện tự động điều chỉnh theo kích thước màn hình
- **FPS**: Frames Per Second - số khung hình/giây
