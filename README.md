BÁO CÁO BÀI TẬP: TÀI LIỆU HỆ THỐNG SMART-FARM LANPHẦN 1. PHÂN TÍCH HỆ THỐNG1. Phân tích mô hình IPO (Input - Process - Output)Input (Đầu vào):Dữ liệu độ ẩm đất từ cảm biến điện dung (Capacitive Soil Moisture Sensor).Dữ liệu nhiệt độ & độ ẩm không khí từ cảm biến DHT22.Tín hiệu điều khiển thủ công từ giao diện Web/App người dùng.Process (Xử lý trung tâm):Vi điều khiển (SoC ESP32) đọc và chuyển đổi tín hiệu Analog/Digital.CPU so sánh dữ liệu thu thập với các ngưỡng an toàn đã cấu hình sẵn cho loài hoa Lan (VD: Độ ẩm đất < 60%, Nhiệt độ > 32°C).Đưa ra quyết định điều khiển thiết bị ngoại vi thông qua Module Relay.Output (Đầu ra):Lệnh bật/tắt máy bơm tưới phun sương/tưới gốc.Lệnh bật quạt thông gió hoặc đóng/mở lưới che nắng.Hiển thị trạng thái lên màn hình OLED và gửi dữ liệu nhật ký lên Cloud.2. Sơ đồ phối hợp phần cứng+-----------------------------------------------------------------------+
|                         TRANG TRẠI LAN (IOT)                          |
|                                                                       |
|  [Cảm biến độ ẩm đất] --(Analog)--> [                  ]              |
|  [Cảm biến DHT22]     --(Digital)--> [    SoC ESP32   ]              |
|  [Nút bấm thủ công]   --(GPIO)   --> [ (Xử lý trung tâm) ]            |
|                                             |                         |
|                                           (GPIO)                      |
|                                             v                         |
|                                     [ Module Relay ]                  |
|                                             |                         |
|                                             v                         |
|                                   [ Máy Bơm / Quạt Bơm ]              |
+-----------------------------------------------------------------------+
PHẦN 2. TRIỂN KHAI (FILE README.MD HOÀN CHỈNH)(Sao chép toàn bộ nội dung trong khung dưới đây để lưu thành file README.md nộp lên GitHub)# 🚀 Hệ Thống Quản Lý Trang Trại Lan Thông Minh (Smart-Farm)

*Dự án tự động hóa nông nghiệp IoT dành riêng cho trang trại Lan – Giám sát vi khí hậu, tối ưu hóa quy trình tưới tiêu và nâng cao năng suất cây trồng.*

---

## 📋 Mô tả hệ thống
**Smart-Farm Lan** là giải pháp toàn diện hỗ trợ người nông dân quản lý vi khí hậu vườn lan tự động. Bằng việc kết hợp giữa *vi điều khiển SoC*, *cảm biến chuẩn công nghiệp* và *mô hình xử lý dữ liệu thông minh*, hệ thống giúp đảm bảo độ ẩm, nhiệt độ luôn nằm trong ngưỡng tối ưu cho sự sinh trưởng của hoa Lan, đồng thời giảm thiểu **50% lượng nước tiêu thụ** và **công sức vận hành**.

---

## 🛠️ Hướng dẫn cài đặt

Để nạp chương trình điều khiển vào bo mạch SoC ESP32 và khởi chạy hệ thống, thực hiện các lệnh sau trên Terminal:

```bash
# Clone repository dự án từ GitHub
git clone https://github.com/nongdan-tech/smart-farm-lan.git

# Chuyển vào thư mục mã nguồn
cd smart-farm-lan

# Cài đặt các thư viện phần cứng cần thiết (sử dụng PlatformIO CLI)
pio lib install

# Biên dịch và nạp firmware lên vi điều khiển ESP32
pio run --target upload
✨ Tính năng cốt lõi[x] Tưới nước tự động: Tự động kích hoạt máy bơm khi độ ẩm đất xuống dưới ngưỡng cài đặt.[x] Giám sát thời gian thực: Cập nhật liên tục nhiệt độ, độ ẩm không khí và ánh sáng.[x] Cảnh báo khẩn cấp: Gửi tín hiệu cảnh báo qua còi và ứng dụng khi nhiệt độ vượt quá 35°C.[ ] Điều khiển bằng giọng nói: Tích hợp trợ lý ảo tiếng Việt (Tính năng đang phát triển ở phiên bản v2.0).📊 Bảng thông số kỹ thuật phần cứngLinh kiện / Thiết bịMã hiệu (Model)Chức năng chínhThông số vận hànhSoC Điều khiểnESP32-WROOM-32Xử lý trung tâm & Kết nối Wi-Fi/Bluetooth3.3V DC, 240MHzCảm biến độ ẩm đấtCapacitive v1.2Đo độ ẩm trong chậu LanSignal Analog (0 - 3.3V)Cảm biến nhiệt độ - độ ẩmDHT22 / AM2302Đo vi khí hậu nhà kính-40 đến 80°C (±0.5°C)Relay điều khiển2-Channel Relay 5VĐóng/mắt nguồn điện máy bơm10A - 250V AC⚙️ Quy trình xử lý lệnh tưới Lan tại CPU (SoC)Chu kỳ Fetch – Decode – Execute khi CPU nhận lệnh tưới:Fetch (Lấy lệnh): CPU truy xuất mã lệnh kích hoạt van tưới từ bộ nhớ RAM/Flash của SoC ESP32.Decode (Giải mã lệnh): Bộ giải mã lệnh (Instruction Decoder) trong CPU chuyển lệnh thành các tín hiệu vi điều khiển, xác định chân GPIO tương ứng với rơ-le máy bơm.Execute (Thực thi lệnh): CPU xuất xung điện áp mức CAO (HIGH - 3.3V) ra chân GPIO được chỉ định, kích hoạt Relay đóng mạch và cấp điện cho máy bơm phun sương.🔺 Mô hình Kim tự tháp Tri thức (DIKW) trong tưới Lan       / \
      /   \     [ WISDOM ] -> Quyết định: Điều chỉnh lịch tưới chuyển sang 6h sáng
     /     \                   để tối ưu hóa sự hấp thụ dinh dưỡng của Lan.
    /_______\
   /         \    [ KNOWLEDGE ] -> Đánh giá: Độ ẩm đất < 50% kết hợp Nhiệt độ > 32°C 
  /           \                    khiến rễ Lan bị sốc nhiệt -> Cần tưới ngay.
 /_____________\
/               \   [ INFORMATION ] -> Thông tin đã xử lý: "Độ ẩm đất hiện tại 42% (Khô), 
/                 \                     Nhiệt độ không khí 33.5°C (Nóng)".
/___________________\
/                     \  [ DATA ] -> Dữ liệu thô thu thập từ cảm biến: 
/                       \           { raw_adc: 2840, temp_val: 33.5, humidity_val: 68 }.
/_________________________\
⚠️ Lưu ý vận hành & An toànKiểm tra toàn bộ các mối nối dây điện trước khi cấp nguồn, tránh tình trạng chập cháy do độ ẩm cao trong nhà kính.Xử lý an toàn khi bộ nhớ RAM bị đầy dữ liệu nhật ký (RAM Full):Hiện tượng: Màn hình hiển thị bị đơ, vi điều khiển tự động lặp lại quá trình khởi động (Reboot loop) do đầy bộ đệm log.Cách xử lý:Ngắt nguồn điện chính của máy bơm để đảm bảo an toàn.Kết nối SoC ESP32 với máy tính qua cổng USB, mở Terminal và gõ lệnh reset bộ đệm: pio device monitor --echo.Tiến hành định dạng lại vùng nhớ SPIFFS/LittleFS bằng cách nhấn giữ nút BOOT trên ESP32 trong 5 giây để xóa sạch file nhật ký tạm (Log files).Khởi động lại hệ thống.🖼️ Sơ đồ kiến trúc tổng quan
---

## PHẦN 3. ĐÁNH GIÁ VÀ KIỂM THỬ (TEST CASE)

* **Trường hợp kiểm thử:** Giả định RAM bị đầy dữ liệu nhật ký (Log data full).
* **Cập nhật trong README:** Đã tí
