# Report 1 page - Lab 6 AES-CBC Socket

## Thông tin nhóm

 - Thành viên 1: Đoàn Quốc Bảo - MSSV: 1871020071
 - Thành viên 2: Nguyễn Đăng Quang - MSSV: 1871020481

## Mục tiêu

- Mục tiêu: Xây dựng hệ thống mẫu gửi/nhận dữ liệu qua TCP socket, áp dụng AES-CBC với PKCS#7 padding, tách kênh khóa và kênh dữ liệu, và viết kiểm thử cho các tình huống đúng/sai. Phân tích điểm yếu bảo mật của thiết kế (key/IV gửi plaintext).

## Phân công thực hiện

- Đoàn Quốc Bảo: Phụ trách triển khai Sender, mã hóa, ghi log và một phần tests integration.
- Nguyễn Đăng Quang: Phụ trách triển khai Receiver, giải mã, lưu output và một phần tests integration.
- Cả hai: Thiết kế protocol, viết unit tests, hoàn thiện report và threat model.

## Cách làm

- Triển khai hàm padding/unpadding PKCS#7 trong aes_socket_utils.py.
- Sử dụng PyCryptodome AES-CBC để mã hóa/giải mã.
- Sender sinh (mô phỏng) key/IV hoặc dùng key truyền vào, xây dựng packet kênh khóa: [key_len(4)][key][iv(16)], gửi lên KEY_PORT.
- Sender gửi ciphertext với header độ dài: [len(4)][ciphertext] lên DATA_PORT.
- Receiver đọc header độ dài, nhận đúng số byte, và giải mã rồi lưu output nếu cần.

## Kết quả

- Các unit test (padding, key/data packet, tamper, wrong key) và integration test local sender/receiver chạy thành công.
- Có file log mẫu trong thư mục logs/ (logs/sender_success.log).
- Receiver có thể lưu bản tin gốc vào sample_output.txt khi chỉ định OUTPUT_FILE.

## Kết luận

- Hệ thống mẫu chứng minh cách tách kênh khóa và kênh dữ liệu, cách dùng header độ dài cho socket và cách triển khai AES-CBC với PKCS#7.
- Tuy nhiên, gửi key/IV plaintext là rủi ro bảo mật lớn; cần TLS hoặc cơ chế trao đổi khóa an toàn và cơ chế xác thực toàn vẹn (HMAC hoặc AES-GCM) cho hệ thống thực tế.
