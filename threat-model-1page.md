# Threat Model - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Đoàn Quốc Bảo - MSSV: 1871020071
- Thành viên 2: Nguyễn Đăng Quang - MSSV: 1871020481

## Assets

- Plaintext (bản tin) cần bảo vệ.
- AES key và IV (nhạy cảm).
- Ciphertext truyền qua DATA_PORT.
- File đầu vào/đầu ra, và file log.

## Attacker model

- Kẻ tấn công có khả năng nghe lén lưu lượng mạng (eavesdrop), chặn/gửi lại gói tin (MITM/replay) và chỉnh sửa ciphertext; cũng có thể truy cập vào file log trên máy nếu bị xâm nhập.

## Threats

- Key disclosure: Key/IV được gửi plaintext trên kênh khóa giả lập -> kẻ nghe lén có thể lấy key.
- Tampering: Ciphertext có thể bị chỉnh sửa trên kênh dữ liệu dẫn tới giải mã sai hoặc lỗi.
- Replay attack: Thiếu nonce/sequence cho phép kẻ tấn công replay gói tin cũ.
- Log leakage: Key/IV hoặc ciphertext có thể bị lộ qua file log nếu ghi nhầm.
- Lack of authentication: Receiver không xác thực nguồn, dễ bị MITM.

## Mitigations

- Không truyền key/IV plaintext trong môi trường thực; dùng TLS để bảo vệ kênh hoặc thực hiện key exchange an toàn (DH, PKI).
- Sử dụng chế độ có xác thực như AES-GCM hoặc kết hợp AES-CBC với HMAC để đảm bảo tính toàn vẹn và xác thực.
- Tránh ghi key/IV vào log; chỉ ghi các thông tin không nhạy cảm.
- Thêm nonce, sequence number hoặc timestamp để chống replay.
- Thực hiện xác thực hai chiều (mutual authentication) nếu cần.

## Residual risks

- Hệ thống demo vẫn còn rủi ro trong môi trường thực: key/IV truyền plaintext (mô phỏng), thiếu xác thực và các cơ chế chống replay hoàn chỉnh.
