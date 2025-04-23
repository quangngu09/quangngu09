# Tìm Hiểu JWT Authentication: Giải Pháp Xác Thực Hiện Đại

## JSON Web Token (JWT) là gì?

**JSON Web Token (JWT)** là một tiêu chuẩn mở (RFC 7519) để truyền thông tin xác thực giữa client và server dưới dạng chuỗi JSON mã hóa. JWT gồm 3 phần chính, được phân tách bằng dấu chấm (`.`):

-   **Header**: Xác định loại token (JWT) và thuật toán mã hóa (ví dụ: HMAC SHA256).
-   **Payload**: Chứa các thông tin (claims) như `user_id`, `role`, thời gian phát hành (`iat`), và thời gian hết hạn (`exp`).
-   **Signature**: Được tạo bằng cách mã hóa Header và Payload với một khóa bí mật, đảm bảo token không bị giả mạo.

Ví dụ một JWT:  
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTIzNDU2Nzg5MCIsImlhdCI6MTY4MjA4MjUwNCwiZXhwIjoxNjkwNzIyNTA0fQ.tWlX7E7NPNftg37fXrdsXvkgEWB_8zaHIQmryAXzElY`

## Cách hoạt động của JWT Authentication

JWT Authentication sử dụng **Access Token** và **Refresh Token** để quản lý xác thực người dùng:

1.  **Đăng nhập**:
    
    -   Người dùng gửi thông tin đăng nhập (username/password) đến server.
    -   Server xác minh, tạo một **Access Token** (thời hạn ngắn, e.g., 5 phút) và một **Refresh Token** (thời hạn dài, e.g., 1 tháng), sau đó gửi về client.
    -   Client lưu token (thường trong localStorage hoặc cookie).
2.  **Truy cập tài nguyên**:
    
    -   Client gửi Access Token trong header `Authorization: Bearer <token>` cho mỗi yêu cầu.
    -   Server xác minh chữ ký của token và kiểm tra thời hạn (`exp`). Nếu hợp lệ, server cấp quyền truy cập.
3.  **Làm mới token**:
    
    -   Khi Access Token hết hạn, client gửi Refresh Token đến server.
    -   Server kiểm tra Refresh Token, nếu hợp lệ, tạo Access Token mới và gửi lại client.

### Ví dụ mã nguồn

Dưới đây là triển khai JWT Authentication với Node.js/Express và thư viện `jsonwebtoken`:

```javascript
const express = require('express');
const jwt = require('jsonwebtoken');
const app = express();
const port = 3000;

app.use(express.json());

const SECRET_KEY = 'my-secret-key';
const REFRESH_SECRET = 'my-refresh-secret';

// Giả lập cơ sở dữ liệu người dùng
const users = [{ id: 1, username: 'admin', password: 'password' }];

// Đăng nhập
app.post('/login', (req, res) => {
    const { username, password } = req.body;
    const user = users.find(u => u.username === username && u.password === password);
    if (!user) return res.status(401).send('Invalid credentials');

    const accessToken = jwt.sign({ user_id: user.id }, SECRET_KEY, { expiresIn: '5m' });
    const refreshToken = jwt.sign({ user_id: user.id }, REFRESH_SECRET, { expiresIn: '30d' });
    res.json({ accessToken, refreshToken });
});

// Làm mới token
app.post('/refresh', (req, res) => {
    const { refreshToken } = req.body;
    try {
        const payload = jwt.verify(refreshToken, REFRESH_SECRET);
        const accessToken = jwt.sign({ user_id: payload.user_id }, SECRET_KEY, { expiresIn: '5m' });
        res.json({ accessToken });
    } catch (error) {
        res.status(401).send('Invalid refresh token');
    }
});

// Route bảo vệ
app.get('/protected', (req, res) => {
    const token = req.headers.authorization?.split(' ')[1];
    try {
        const payload = jwt.verify(token, SECRET_KEY);
        res.send(`Welcome, user ${payload.user_id}!`);
    } catch (error) {
        res.status(401).send('Invalid or expired token');
    }
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
});

```

### Cách chạy mã nguồn

1.  Cài đặt dependencies: `npm install express jsonwebtoken`.
2.  Lưu mã vào `index.js`.
3.  Chạy: `node index.js`.
4.  Gửi POST request đến `http://localhost:3000/login` với body `{ "username": "admin", "password": "password" }` để nhận token.
5.  Sử dụng Access Token trong header `Authorization: Bearer <token>` để truy cập `http://localhost:3000/protected`.

## Ưu điểm của JWT Authentication

-   **Stateless**: Server không lưu trữ token, giảm tải cơ sở dữ liệu.
-   **Hiệu suất cao**: Xác minh token nhanh hơn so với tra cứu session trong cơ sở dữ liệu.
-   **Khả năng mở rộng**: Phù hợp cho hệ thống phân tán, vì token tự chứa thông tin xác thực.
-   **Đa nền tảng**: Dễ tích hợp với web, mobile, hoặc API.

## Nhược điểm của JWT Authentication

-   **Không thể hủy token**: Token vẫn hợp lệ cho đến khi hết hạn, trừ khi dùng cơ chế blacklist.
-   **Kích thước lớn**: JWT dài hơn Session ID, có thể làm tăng kích thước request.
-   **Bảo mật**: Payload có thể bị giải mã Base64, lộ thông tin nếu không dùng HTTPS.
-   **Quản lý Refresh Token**: Yêu cầu lưu trữ Refresh Token phía server, làm mất tính stateless.

## Ứng dụng thực tế

JWT Authentication lý tưởng cho:

-   **API RESTful**: Xác thực người dùng trong các hệ thống không trạng thái.
-   **Single Page Applications (SPA)**: Như React, Vue, Angular.
-   **Ứng dụng mobile**: Hỗ trợ xác thực trên iOS/Android.
-   **Hệ thống phân tán**: Dễ dàng tích hợp giữa các dịch vụ khác nhau.

## Kết luận

**JWT Authentication** là một giải pháp xác thực mạnh mẽ, phù hợp cho các ứng dụng hiện đại nhờ tính stateless và khả năng mở rộng. Tuy nhiên, cần sử dụng HTTPS để bảo vệ dữ liệu và quản lý Refresh Token cẩn thận để duy trì bảo mật. Hiểu rõ cách triển khai JWT sẽ giúp bạn xây dựng các hệ thống an toàn và hiệu quả hơn.[](https://duthanhduoc.com/blog/p3-giai-ngo-authentication-jwt)
