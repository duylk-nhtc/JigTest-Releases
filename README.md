# Jig test Releases

Bộ cài và thông tin phát hành **Jig test** — ứng dụng Windows hỗ trợ kiểm tra thiết bị qua JIG USB-COM của Nhatrang Hitech.

Repository này dành cho **bộ cài và hướng dẫn sử dụng**, không chứa source code ứng dụng, dữ liệu kiểm tra hoặc cấu hình riêng của nhà máy.

## Tải phiên bản mới nhất

**[Mở trang tải bản mới nhất](https://github.com/duylk-nhtc/JigTest-Releases/releases/latest)**

Trong phần **Assets**, tải file có tên `JigTest-Setup-<version>.exe`, bản hiện tại **`JigTest-Setup-1.1.3.exe`**.

Không tải **Source code (zip)** hoặc **Source code (tar.gz)** để cài ứng dụng. Hai file đó do GitHub tự tạo, không phải bộ cài.

## Yêu cầu hệ thống

- Windows 10/11 64-bit, tương thích x64.
- Cổng USB và JIG có firmware tương thích NJP1.
- Driver USB-COM đúng loại chip của JIG, chẳng hạn CH340/CH341 hoặc CP210x.
- Internet để kiểm tra và tải bản cập nhật; chức năng tải ngưỡng Vota còn phụ thuộc server do kỹ thuật viên cấu hình.
- Quyền ghi thư mục cấu hình và dữ liệu của app.

Máy đích **không cần cài Python, pip hoặc các thư viện Python**. Bộ cài đã kèm môi trường chạy.

> **Driver cài riêng:** bộ cài 1.0.4 phát hành không kèm driver và bản 1.0.5 không tự cài driver USB-COM. Nếu Windows không nhận cổng COM, liên hệ kỹ thuật viên hoặc cài driver từ nhà sản xuất chip/Windows Update. Không dùng driver từ nguồn không rõ ràng.

## Cài đặt lần đầu

1. Tải bộ cài mới nhất trong trang Releases.
2. Mở file EXE và làm theo hướng dẫn.
3. Giữ vị trí cài mặc định nếu không có yêu cầu riêng từ kỹ thuật viên:
   `%LOCALAPPDATA%\Programs\NHTC\JigTest`.
4. Chọn tạo shortcut nếu cần, rồi hoàn tất cài đặt.
5. Mở **Jig test** và kết nối JIG.

Có thể cài thẳng phiên bản mới nhất, không cần cài lần lượt các phiên bản cũ.

Bộ cài hiện chưa được ký số; Windows có thể hiển thị cảnh báo. Chỉ sử dụng bản do đơn vị phát hành xác nhận. Không tắt phần mềm bảo mật hoặc bỏ qua chính sách bảo vệ của công ty; liên hệ IT nếu bị chặn.

## Cách sử dụng

### JIG Ethernet — `JIG-ETH-01`

1. Cắm JIG, chờ thông báo **CONNECTED** và bộ kiểm tra **Ethernet** xuất hiện.
2. Ô ID tự nhận focus sau khi kết nối. Nhập hoặc quét ID gồm **đúng 10 chữ số**, bắt đầu bằng **09**.
3. Nhấn nút **START vật lý trên JIG** để bắt đầu; nhập mã hoặc nhấn Enter không tự chạy test.
4. Theo dõi thanh tiến trình và kết quả:
   - **LAN link:** có kết nối vật lý.
   - **IPv4 address:** địa chỉ IPv4 hợp lệ và khác `0.0.0.0`.
   - **PASS** xanh lá khi cả hai mục đạt; **FAIL** đỏ nếu tiêu chí không đạt.
5. Nhấn **STOP vật lý trên JIG** để dừng lượt; app hiển thị **STOPPED**, không tự tạo kết quả nghiệm thu.
6. Sau khi có PASS/FAIL, nhấn **STOP trên JIG** hoặc **Space** để xóa ID, Overall, checklist, tiến trình và đồ thị, đưa con trỏ về ô ID. Space vẫn dùng được khi không chạy test. Lịch sử kết quả đã lưu không bị xóa.

Trong lúc chờ JIG, thanh tiến trình có thể chạy không xác định phần trăm. Quá thời gian chờ mặc định 70 giây sẽ hiển thị **NO RESULT**, không tự coi thiết bị là FAIL; firmware có thể vẫn đang test.

**LAN/IP PASS không có nghĩa là truy cập Internet thành công.** App hiện không kiểm tra DNS hoặc kết nối Internet của thiết bị đang test.

### Các bộ kiểm tra khác

| Bộ kiểm tra | Mã JIG | Nội dung |
|---|---|---|
| Ethernet | `JIG-ETH-01` | LAN link và IPv4; đã có luồng START/STOP vật lý |
| Vota | `JIG-VOTA-01` | 8 mục: Initialize ISP3080, LED, Motion Sensor, Serial ID, MAC ID, Battery Voltage, Battery Charging, SOS |
| Decode | `NJP1-DECODE-v1` | 38.4 MHz, 2 Hz, LOL và LOS; ID 10 chữ số đầu 08 |
| Jitter | `NJP1-SYNC-v1` | 38.4 MHz, 2 Hz, LOL và LOS; ID 10 chữ số đầu 06 hoặc 07 |

**Decode/Jitter:** nhập ID rồi nhấn START vật lý. App chờ 5 giây, đo 38.4 MHz và 2 Hz, lặp đủ 3 lần với 2 chu kỳ POWER/OFF (ACK rồi giữ 3 giây) và POWER/ON (ACK rồi chờ 5 giây). Chỉ gửi RESULT sau cùng. Firmware phải hỗ trợ POWER/POWERACK; driver và SDK Hantek x64 cài riêng. Cả hai sản phẩm dùng CH1 cho 38.4 MHz, CH2 cho 2 Hz và đầu dò x1 từ v1.1.1. Cần kiểm chứng băng thông đầu dò x1 tại 38.4 MHz, không mặc định Vmax chính xác ngoài băng thông.

**Giới hạn Vota:** còn cần server ngưỡng, bảng lệnh firmware và luồng khởi động được kỹ thuật viên xác nhận.

JIG chưa được hỗ trợ sẽ hiển thị thông báo chưa có bộ kiểm tra. Khi chưa kết nối hoặc đang test, ô ID bị khóa. ID sai độ dài/tiền tố không được nhận để bắt đầu test; ô nhập chỉ chấp nhận số 0–9.

## Cập nhật ứng dụng

Từ v1.1.3, mỗi lần mở app sẽ dọn bộ cài cũ trong `data\updates` trước khi kiểm tra bản mới. Chỉ xóa bộ cài có phiên bản ≤ bản đang chạy; giữ bản mới hơn và không xóa kết quả test.

### Bản 1.0.5 trở lên

1. Kết nối Internet rồi mở ứng dụng.
2. Trước màn hình chính, app kiểm tra bản cập nhật.
3. Nếu có bản mới, chọn:
   - **Update:** tải và kiểm tra bộ cài, sau đó mở trình cài đặt.
   - **Later:** tiếp tục dùng phiên bản hiện tại.
4. Hoàn tất trình cài đặt, giữ nguyên thư mục cũ rồi mở lại app.

App không cài ngầm hoàn toàn: người dùng phải xác nhận và hoàn tất bộ cài. Khi không có bản mới hoặc kiểm tra mạng lỗi, app tiếp tục mở bình thường.

### Bản 1.0.3 và 1.0.4

App kiểm tra ở chế độ nền sau khi mở. Có phiên bản mới thì hiện nút **Update to …** trong màn hình chính. Chỉ cập nhật khi không đang test.

### Bản 1.0.2 trở xuống

Chưa có updater. Tải và cài thủ công phiên bản mới nhất một lần.

### Giữ cấu hình và kết quả

Nâng cấp cùng thư mục thay thế ứng dụng cũ, không tạo hai app chạy song song. Shortcut cũ mở bản mới; không cần gỡ bản cũ trước.

- Giữ nguyên cấu hình trạm trong `config.json` và dữ liệu đã lưu.
- Từ v1.1.0, app nạp profile Decode/Sync mới từ mặc định đi kèm khi chạy, vẫn giữ token và cấu hình riêng trên đĩa.
- File Setup cũ trong Downloads hoặc các bản portable ở thư mục khác không bị tự xóa.
- Sao lưu dữ liệu trước khi nâng cấp. Không đổi vị trí cài nếu muốn nâng cấp tại chỗ.

Phiên bản được thể hiện trong tên cửa sổ và tiêu đề ứng dụng từ bản 1.0.5, ví dụ **Jig test v1.0.5**.

## Lịch sử phiên bản

### v1.1.3 — 13/11/2026

- Ethernet nhận định dạng firmware `ETH|Link:0,IP:0.0.0.0`, vẫn tương thích định dạng `[0][0.0.0.0]` cũ. Link mất hoặc IP `0.0.0.0` được ghi FAIL khi có lượt test hợp lệ đang chạy.
- Sau kết quả Ethernet hợp lệ, gửi `RESULT|PASS` hoặc `RESULT|FAIL` về ESP32 để firmware điều khiển đèn. STOP/ngắt kết nối/hết thời gian chờ không tự tạo kết quả DUT.
- STOP trên JIG sau PASS/FAIL xóa phần hiển thị giống Space; STOP khi đang test vẫn dừng lượt. Space giữ nguyên, đồng thời xóa checklist, tiến trình và đồ thị. Không xóa lịch sử đã lưu.
- Thanh tiến trình màu xanh lá; ô ID chỉ nhận tối đa 10 chữ số, vẫn kiểm tra tiền tố theo JIG.
- Tự dọn bộ cài `JigTest-Setup-x.x.x.exe` trong `data\updates` trước bước kiểm tra cập nhật: chỉ xóa phiên bản nhỏ hơn hoặc bằng app đang chạy. Giữ bộ cài mới hơn; file bị khóa được thử lại lần mở sau. Không dọn Downloads hay dữ liệu test.

### v1.1.2 — 13/11/2026

- Sửa lỗi khởi động `FileNotFoundError` do thiếu cấu hình mặc định nội bộ trong bản đóng gói.
- Dùng `release_defaults.json` riêng, kiểm tra bắt buộc tồn tại sau build; không bị loại bởi quy tắc bảo vệ `config.json` của trạm.
- Giữ AppId, cấu hình, token và dữ liệu của máy đã cài. Cấu hình mặc định đóng gói không chứa token trạm.
- Giữ cấu hình v1.1.1: **38.4 MHz CH1, 2 Hz CH2, probe x1**; không đổi ngưỡng, thời gian POWER hoặc firmware.

> **Máy 1.1.1 đang lỗi khởi động:** tải và chạy bộ cài 1.1.2 thủ công vào vị trí cài cũ. Lỗi xảy ra trước bước kiểm tra update nên máy này không tự tải bản vá được. Không xóa cấu hình hoặc dữ liệu trạm.


### v1.1.1

- Thống nhất Decode và Jitter: **38.4 MHz dùng CH1, 2 Hz dùng CH2**.
- Đổi hệ số đầu dò production từ x10 sang **x1**; không đổi ngưỡng tần số và thời gian POWER.
- Cần kiểm chứng đầu dò x1 đủ băng thông 38.4 MHz; hệ số phần mềm không khắc phục suy hao/méo do đầu dò.
- Bộ cài `JigTest-Setup-1.1.1.exe`, không kèm driver/DLL SDK Hantek.

### v1.1.0

- Tích hợp Hantek vào checklist Decode/Jitter; biểu đồ chỉ hiện khi nhận đúng JIG, hiển thị Vmax và tần số; Vmax chưa dùng để PASS/FAIL.
- Đo đủ 3 lượt, chờ START 5 giây, OFF 3 giây và ON ổn định 5 giây; chờ POWERACK trước bước tiếp theo.
- 38.4 MHz: mục tiêu 38.370–38.430 MHz, vùng capture 38.2–38.5 MHz; 2 Hz: 1–2.8 Hz, 11 quan sát, FAIL khi có quan sát không hợp lệ thứ ba.
- Cả ba lượt sóng phải PASS; LOL/LOS vòng cuối phải bằng 0 từ DATA hợp lệ. Lỗi thiết bị/ACK hoặc timeout hủy lượt không tự tạo PASS.
- Thêm Jitter ID đầu 06/07; giữ Decode đầu 08. Bỏ nhãn DHCP ở ETH, không đổi tiêu chí link/IP.
- Giữ AppId để nâng cấp tại chỗ; nạp profile Decode/Sync mới trong khi giữ cấu hình riêng của trạm.
- Cấu hình kênh/que của bản này đã được thay thế bởi v1.1.1. Chưa kiểm chứng phần cứng sản xuất.

### v1.0.5

- Hiển thị số phiên bản trong tên cửa sổ và tiêu đề app theo dạng `Jig test v1.0.5`.
- Chuyển kiểm tra cập nhật sang trước màn hình chính/kết nối JIG.
- Hỏi người dùng bằng hai lựa chọn **Update / Later**; bỏ nút Update trong màn hình test.
- Giữ chức năng tiếp tục dùng app khi không có bản mới hoặc lỗi mạng.
- Không kèm driver USB-COM.

### v1.0.4

- Duy trì kiểm tra cập nhật qua GitHub Releases và xác nhận của người dùng.
- Bản phân phối hiện tại **không kèm driver**; driver được cài riêng.
- Bản có driver từng được tạo để thử nghiệm đã được thay thế, không dùng bản đó để phát hành công khai.

### v1.0.3

- Thêm cập nhật từ repository public `duylk-nhtc/JigTest-Releases`.
- Kiểm tra phiên bản ở chế độ nền; hiện nút Update khi có bản mới.
- Kiểm tra kích thước và SHA-256 do GitHub cung cấp trước khi chạy bộ cài.
- Chỉ cho nhập chữ số trong ô ID.

### v1.0.2

- Tự đặt focus ô ID khi kết nối hoặc kết nối lại JIG được hỗ trợ.
- Space xóa ID cũ và Overall result, đưa con trỏ về ô ID; không ảnh hưởng file kết quả đã lưu.
- Bỏ dòng hướng dẫn “Scan or type…”.
- Căn giữa thông báo chờ kết nối trong vùng danh sách test.

### v1.0.1

- Khóa ô nhập ID khi chưa kết nối và trong lúc test.
- Yêu cầu ID đúng 10 chữ số.
- Kiểm tra tiền tố 08 cho Decode, 09 cho Ethernet/LAN; hiển thị cảnh báo khi không hợp lệ.

### v1.0.0

- Bộ cài Windows ban đầu, kèm Python và thư viện chạy ứng dụng.
- Giao diện xanh dương/trắng, logo NHTC, kết quả PASS/FAIL.
- Nhận diện bộ kiểm tra theo mã JIG, hỗ trợ luồng sự kiện Ethernet và ghi kết quả cục bộ.
- Cài theo tài khoản người dùng, có shortcut và chức năng gỡ cài đặt.

## Lưu trữ dữ liệu

Theo cấu hình mặc định, log và kết quả nằm trong thư mục `data` cạnh cấu hình ở vị trí cài app:

- `results_eth.csv`: kết quả Ethernet.
- `results.jsonl`: kết quả chi tiết theo từng dòng JSON.
- `application.log`: log ứng dụng.
- `threshold_cache.json`: cache ngưỡng khi sử dụng chức năng ngưỡng HTTP.

Không đăng các file này hoặc token/cấu hình riêng của trạm lên repository public. Gỡ cài đặt giữ lại cấu hình và dữ liệu; liên hệ kỹ thuật viên nếu cần xóa sau khi sao lưu.

## Xử lý sự cố

| Hiện tượng | Cách kiểm tra |
|---|---|
| App 1.1.1 báo thiếu file cấu hình khi mở | Tải bộ cài 1.1.2 và cài lại vào vị trí cũ, không xóa dữ liệu |
| Không nhận JIG | Kiểm tra cáp, driver, cổng COM và firmware; đóng phần mềm khác đang giữ COM |
| Không nhập được ID | Chưa kết nối JIG được hỗ trợ hoặc đang test |
| ID không hợp lệ | Đúng 10 số, tiền tố 09 cho LAN, 08 cho Decode, 06/07 cho Jitter |
| Không thấy bản cập nhật | Mở lại app, kiểm tra Internet, bản hiện tại và release mới nhất |
| Không tải/cài được cập nhật | Kiểm tra proxy/chính sách IT; có thể tải bộ cài thủ công từ trang Releases |
| Không có bài kiểm tra | Mã JIG chưa có profile tương ứng |
| Không lưu được kết quả | Kiểm tra quyền ghi, ổ đĩa và file CSV có đang bị ứng dụng khác khóa không |

## Lưu ý phát hành

- Bộ cài hướng tới Windows 10/11 x64, không bảo đảm tương thích mọi máy/chính sách doanh nghiệp.
- Các kiểm thử tự động không thay thế kiểm chứng trên Windows sạch và JIG thật.
- Cập nhật hiện dựa trên HTTPS và digest GitHub; chưa có chữ ký nhà phát hành độc lập. SHA-256 không thay thế chữ ký số.
- Các bản mới không kèm driver; đơn vị phát hành vẫn cần đáp ứng giấy phép của công cụ và các thư viện phân phối.
- Báo lỗi tại [Issues](https://github.com/duylk-nhtc/JigTest-Releases/issues) nếu repository đã bật chức năng này. Chỉ gửi thông tin đã loại bỏ token, mã sản phẩm và dữ liệu nội bộ nhạy cảm.
