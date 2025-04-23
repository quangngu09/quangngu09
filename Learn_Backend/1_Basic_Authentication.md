# Hiểu Biết về Authentication và Authorization: Tìm Hiểu Basic Authentication

## Authentication và Authorization là gì?

Trong lĩnh vực bảo mật công nghệ thông tin, **Authentication** (xác thực) và **Authorization** (ủy quyền) là hai khái niệm cốt lõi, nhưng chúng có vai trò khác nhau.

-   **Authentication** là quá trình xác minh danh tính của người dùng hoặc thiết bị, trả lời câu hỏi: "Bạn là ai?". Ví dụ, khi bạn đăng nhập vào một website bằng username và password, hệ thống sẽ kiểm tra xem thông tin này có khớp với dữ liệu lưu trữ hay không.
    
-   **Authorization** là bước tiếp theo, xác định quyền truy cập của người dùng sau khi danh tính đã được xác thực, trả lời câu hỏi: "Bạn được phép làm gì?". Ví dụ, một nhân viên bình thường có thể xem báo cáo, nhưng chỉ quản trị viên mới có quyền chỉnh sửa hoặc xóa dữ liệu.
    

### Sự khác biệt chính

-   **Authentication** tập trung vào việc xác minh danh tính, là bước đầu tiên trong quy trình bảo mật.
    
-   **Authorization** quyết định quyền hạn, chỉ xảy ra sau khi xác thực thành công.
    
-   **Ví dụ thực tế**: Authentication giống như việc kiểm tra thẻ nhân viên để vào tòa nhà công ty, trong khi Authorization xác định bạn được phép vào phòng họp hay khu vực quản lý.
    

## Basic Authentication: Kỹ thuật xác thực đơn giản

**Basic Authentication** là một phương pháp xác thực cổ điển và dễ triển khai, thường được sử dụng trong các ứng dụng web đơn giản. Phương pháp này sử dụng username và password để xác minh danh tính người dùng.

### Cách hoạt động của Basic Authentication

1.  **Gửi thông tin xác thực**:
    
    -   Người dùng nhập username và password vào trình duyệt.
        
    -   Trình duyệt mã hóa cặp username:password thành chuỗi Base64 và gửi qua HTTP header Authorization.
        
2.  **Kiểm tra phía server**:
    
    -   Server nhận header, giải mã chuỗi Base64 để lấy username và password.
        
    -   Server so sánh thông tin này với dữ liệu trong cơ sở dữ liệu.
        
    -   Nếu thông tin hợp lệ, server trả về tài nguyên yêu cầu; nếu không, một popup yêu cầu nhập lại thông tin sẽ xuất hiện.
        
3.  **Ví dụ mã nguồn**: Dưới đây là một đoạn mã Express.js minh họa cách triển khai Basic Authentication:
    
    ```javascript
    const express = require('express');
    const app = express();
    const port = 3000;
    
    function authenticate(req, res, next) {
        const authHeader = req.headers.authorization;
        if (authHeader) {
            const auth = Buffer.from(authHeader.split(' ')[1], 'base64').toString().split(':');
            const username = auth[0];
            const password = auth[1];
            if (username === 'admin' && password === 'password') {
                return next();
            }
        }
        res.set('WWW-Authenticate', 'Basic realm="401"');
        res.status(401).send('Authentication required.');
    }
    
    app.get('/', authenticate, (req, res) => {
        res.send('Welcome to the protected page!');
    });
    
    app.listen(port, () => {
        console.log(`Server running at http://localhost:${port}`);
    });
    ```
    

### Ưu điểm của Basic Authentication

-   **Đơn giản**: Dễ triển khai, không yêu cầu hệ thống phức tạp.
    
-   **Hiệu quả cho môi trường nội bộ**: Phù hợp để bảo vệ các trang staging hoặc quản trị nội bộ, nơi chỉ một số người được phép truy cập.
    
-   **Tích hợp sẵn**: Nhiều máy chủ web hỗ trợ Basic Authentication tự động.
    

### Nhược điểm của Basic Authentication

-   **Không thể logout dễ dàng**: Người dùng chỉ logout khi xóa cache trình duyệt hoặc đóng trình duyệt.
    
-   **Không phù hợp cho mobile**: Ứng dụng di động thường thiếu giao diện để nhập username/password liên tục.
    
-   **Bảo mật thấp**: Nếu không sử dụng HTTPS, thông tin xác thực có thể bị lộ do mã hóa Base64 dễ giải mã.
    

### Ứng dụng thực tế

Basic Authentication thường được sử dụng trong các trường hợp sau:

-   **Môi trường phát triển**: Bảo vệ các website staging để chỉ nhóm phát triển truy cập.
    
-   **Trang quản trị**: Hạn chế truy cập vào các URL nhạy cảm như /admin.
    
-   **API nội bộ**: Xác thực nhanh cho các API không yêu cầu bảo mật cao.
    

## Kết luận

**Authentication** và **Authorization** là hai trụ cột trong bảo mật hệ thống, với Authentication xác minh danh tính và Authorization kiểm soát quyền truy cập. **Basic Authentication**, dù đơn giản, là một công cụ hiệu quả trong các trường hợp cụ thể, nhưng cần được sử dụng cẩn thận với HTTPS để đảm bảo an toàn. Hiểu rõ sự khác biệt giữa hai khái niệm này và cách áp dụng Basic Authentication sẽ giúp bạn xây dựng hệ thống an toàn và hiệu quả hơn.
