#  Personal Finance Manager

**Personal Finance Manager** là ứng dụng web toàn diện giúp bạn quản lý tài chính cá nhân một cách hiện đại, trực quan và bảo mật. Dự án hỗ trợ quản lý thu chi, đặt mục tiêu tiết kiệm, kiểm soát ngân sách theo thời gian thực, giao dịch định kỳ tự động, thống kê chi tiết bằng biểu đồ, và nhiều tính năng tiện ích khác.

---

##  Tính năng nổi bật

###  Quản lý giao dịch
-  Thêm, sửa, xóa giao dịch thu/chi
-  Phân loại theo danh mục tùy chỉnh (icon, màu sắc)
-  Lọc giao dịch theo loại, danh mục, khoảng thời gian, số tiền
-  Tìm kiếm giao dịch theo từ khóa
-  Phân trang danh sách giao dịch
-  Xuất báo cáo PDF/Excel

###  Quản lý ngân sách
-  Thiết lập ngân sách theo danh mục hoặc tổng thể
-  Theo dõi ngân sách theo chu kỳ (hàng tuần, tháng, năm)
-  Cảnh báo tự động khi vượt ngưỡng (80%, 100%)
-  Hiển thị tiến độ chi tiêu bằng biểu đồ tròn
-  Thống kê số dư còn lại

###  Mục tiêu tài chính
-  Đặt mục tiêu tiết kiệm với icon, màu sắc tùy chỉnh
-  Theo dõi tiến độ đạt mục tiêu theo thời gian thực
-  Thêm tiền vào mục tiêu dần dần
-  Ưu tiên mục tiêu (cao, trung bình, thấp)
-  Hiển thị số ngày còn lại đến deadline
-  Thông báo khi hoàn thành mục tiêu

###  Giao dịch định kỳ
-  Tạo giao dịch tự động lặp lại (hàng ngày, tuần, tháng, năm)
-  Tạm dừng/kích hoạt giao dịch định kỳ
-  Thực hiện thủ công khi cần
-  Xem danh sách giao dịch sắp tới (30 ngày)
-  Theo dõi số lần đã thực hiện

###  Thống kê & Biểu đồ
-  Dashboard tổng quan với AI insights
-  Thống kê theo thời gian (hôm nay, tuần, tháng, năm)
-  Biểu đồ cột (Bar Chart) xu hướng thu chi 6 tháng
-  Biểu đồ tròn (Pie Chart) chi tiêu theo danh mục
-  Bảng xu hướng tài chính với % thay đổi
-  Giao dịch gần đây

###  Xác thực & Bảo mật ⭐
-  **JWT Authentication**: Token-based authentication với thời hạn 7 ngày
-  **Password Hashing**: Mã hóa mật khẩu bằng bcryptjs (10 salt rounds)
-  **Protected Routes**: Middleware authentication bảo vệ tất cả API endpoints
-  **Auto Logout**: Tự động đăng xuất khi token hết hạn (401 Unauthorized)
-  **Password Reset**: Hệ thống quên mật khẩu với token hết hạn 10 phút
-  **Input Validation**: Mongoose schema validation + regex email
-  **CORS Security**: Cấu hình CORS với origin cụ thể
-  **Error Handling**: Centralized error middleware
-  **Secure Password Storage**: Password không trả về trong API response

###  Giao diện & Trải nghiệm
-  Dark Mode/Light Mode
-  Responsive design (mobile, tablet, desktop)
-  Thông báo toast (success, error, warning)
-  Loading states & skeleton screens
-  Animations & transitions mượt mà
-  Multi-currency support (VND, USD, EUR)

---

##  Công nghệ sử dụng

### Frontend
- **React 18** - Thư viện UI
- **Vite** - Build tool & dev server
- **TailwindCSS** - Utility-first CSS framework
- **React Router v6** - Client-side routing
- **Axios** - HTTP client
- **React Context API** - State management
- **Recharts** - Biểu đồ React
- **Chart.js** - Biểu đồ thống kê
- **React Icons** - Icon library
- **React Toastify** - Thông báo toast
- **jsPDF & jsPDF-AutoTable** - Xuất PDF
- **XLSX** - Xuất Excel

### Backend
- **Node.js** - JavaScript runtime
- **Express** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB ODM
- **JWT (jsonwebtoken)** - Authentication
- **bcryptjs** - Password hashing
- **Nodemailer** - Email service
- **cors** - Cross-origin resource sharing
- **dotenv** - Environment variables

### Kiến trúc
- **RESTful API** - API architecture
- **MVC Pattern** - Model-View-Controller
- **Middleware** - Authentication & error handling
- **Protected Routes** - Route guards

---

## 🔐 Kỹ Thuật Bảo Mật (Security Implementation)

> **Tại sao bảo mật quan trọng trong ứng dụng tài chính?**  
> Ứng dụng quản lý tài chính cá nhân chứa thông tin nhạy cảm (thu nhập, chi tiêu, mục tiêu tài chính). Việc bảo vệ dữ liệu người dùng không chỉ là yêu cầu kỹ thuật mà còn là trách nhiệm đạo đức và pháp lý.

---

### 1. **Authentication & Authorization** 
**🎯 Mục đích:** Xác định danh tính người dùng và kiểm soát quyền truy cập  
**👤 Lợi ích cho người dùng:** Đảm bảo chỉ chủ tài khoản mới xem được dữ liệu tài chính của mình  
**🔒 Bảo vệ khỏi:** Truy cập trái phép, giả mạo danh tính, đánh cắp dữ liệu
#### JWT (JSON Web Token)
```javascript
// Tạo token khi login/register
const generateToken = (id) => {
  return jwt.sign({ id }, process.env.JWT_SECRET, {
    expiresIn: '7d'
  });
};
```

#### Middleware Protection
```javascript
// Middleware bảo vệ route
export const protect = async (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  req.user = await User.findById(decoded.id).select('-password');
  next();
};
```

- **JWT là gì?** Token chứa thông tin user được mã hóa, không cần lưu session trên server (stateless)
- **Tại sao dùng JWT?** Scalable, dễ implement API cho mobile/web, không tốn bộ nhớ server
- **Cơ chế hoạt động:** 
  1. User login → Server tạo token (chứa user ID + secret key)
  2. Client lưu token → Gửi kèm mỗi request (Authorization header)
  3. Server verify token → Cho phép truy cập nếu hợp lệ
- **Token expire 7 ngày:** Cân bằng giữa UX (không phải login liên tục) và security (limit thời gian token bị đánh cắp)

---

### 2. **Password Security** 
**🎯 Mục đích:** Bảo vệ mật khẩu người dùng ngay cả khi database bị hack  
**👤 Lợi ích cho người dùng:** Mật khẩu không bao giờ bị lộ dạng plain text, kể cả admin không thể xem  
**🔒 Bảo vệ khỏi:** Database breach, inside attack, password leak
#### Bcrypt Hashing
```javascript
// Tự động hash password trước khi lưu
userSchema.pre('save', async function(next) {
  if (!this.isModified('password')) return next();
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
});

// So sánh password khi login
userSchema.methods.comparePassword = async function(enteredPassword) {
  return await bcrypt.compare(enteredPassword, this.password);
};
```

- **Bcrypt là gì?** Thuật toán one-way hashing (không thể decrypt ngược lại)
- **Salt là gì?** Chuỗi random thêm vào password trước khi hash → Cùng password sẽ có hash khác nhau
- **10 rounds là gì?** Số lần lặp thuật toán (càng cao càng an toàn nhưng chậm hơn), 10 rounds = cân bằng tốt
- **Tại sao không dùng MD5/SHA1?** Quá nhanh, dễ bị brute force. Bcrypt chậm hơn = an toàn hơn
- **select: false:** Password field không trả về khi query User → Tránh lộ hash qua API

**🔐 Kịch bản thực tế:**
- Kẻ tấn công hack database → Chỉ thấy hash (`$2a$10$xyz...`)
- Không thể dùng hash để login (server so sánh bằng bcrypt.compare)
- Brute force 1 password mất hàng năm với bcrypt

---

### 3. **Data Validation** 
**🎯 Mục đích:** Đảm bảo dữ liệu đầu vào hợp lệ, ngăn chặn tấn công injection  
**👤 Lợi ích cho người dùng:** Dữ liệu luôn chính xác, tránh lỗi hệ thống do input sai  
**🔒 Bảo vệ khỏi:** NoSQL Injection, XSS, malformed data
#### Mongoose Schema Validation
```javascript
const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: [/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/, 'Email không hợp lệ']
  },
  password: {
    type: String,
    required: true,
    minlength: [6, 'Mật khẩu phải có ít nhất 6 ký tự'],
    select: false // Không trả về trong query
  }
});
```

- **Validation ở đâu?** Schema level (MongoDB) + Controller level (optional với express-validator)
- **Tại sao validate email format?** Ngăn spam accounts, đảm bảo có thể gửi email reset password
- **lowercase: true:** Tránh trùng lặp email (user@gmail.com = USER@gmail.com)
- **required fields:** Đảm bảo không có missing data trong database
- **NoSQL Injection prevention:** Mongoose tự động escape special characters trong query

**🔐 Ví dụ tấn công bị chặn:**
```javascript
// Kẻ tấn công gửi: { "email": { "$gt": "" }, "password": "123" }
// Mongoose validation reject → Không thể bypass authentication
```

---

### 4. **CORS Configuration** 
**🎯 Mục đích:** Kiểm soát domain nào được phép gọi API  
**👤 Lợi ích cho người dùng:** Ngăn website độc hại đánh cắp data thông qua browser  
**🔒 Bảo vệ khỏi:** CSRF attacks, unauthorized API access từ domains khác
```javascript
app.use(cors({
  origin: process.env.CLIENT_URL || 'http://localhost:5173',
  credentials: true
}));
```

- **CORS là gì?** Cross-Origin Resource Sharing - Cơ chế bảo mật của browser
- **Tại sao cần?** Mặc định browser chặn request từ domain khác (security feature)
- **origin: CLIENT_URL:** Chỉ cho phép frontend chính thức gọi API
- **credentials: true:** Cho phép gửi cookies/authorization headers
- **Không set origin:** Mọi website đều gọi được API → Nguy hiểm!

**🔐 Kịch bản bảo vệ:**
- User đang login trên `financeapp.com`
- Vào website độc hại `evil.com` → Website này TRY gọi API `financeapp.com/api/transactions`
- Browser chặn request vì `evil.com` không trong whitelist CORS
- Data tài chính được bảo vệ

---

### 5. **Token Management (Client-side)** 
**🎯 Mục đích:** Quản lý token an toàn trên trình duyệt, tự động xử lý authentication  
**👤 Lợi ích cho người dùng:** Trải nghiệm mượt mà (auto login), tự động logout khi token hết hạn  
**🔒 Bảo vệ khỏi:** Stale token attacks, manual token handling errors
#### Axios Interceptor
```javascript
// Tự động thêm token vào mỗi request
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Auto logout khi token hết hạn
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem('token');
      localStorage.removeItem('user');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

- **Axios Interceptor là gì?** Middleware cho HTTP requests (chạy trước/sau mỗi request)
- **Request Interceptor:** Tự động thêm `Authorization: Bearer <token>` vào header → Không cần manually ở mỗi API call
- **Response Interceptor:** Catch 401 errors → Auto logout + redirect login
- **localStorage vs Cookie:** localStorage dễ implement hơn, nhưng httpOnly cookie an toàn hơn (immune to XSS)

**🔄 Flow hoạt động:**
1. User login → Save token vào localStorage
2. Mọi API call → Interceptor tự động thêm token vào header
3. Token expired → Server trả 401 → Interceptor clear storage + redirect login
4. User không cần làm gì, trải nghiệm seamless

---

### 6. **Protected Routes (Frontend)** 
**🎯 Mục đích:** Ngăn user chưa login truy cập trang protected  
**👤 Lợi ích cho người dùng:** Luôn được điều hướng đúng (login nếu chưa auth)  
**🔒 Bảo vệ khỏi:** Unauthorized access, manual URL manipulation
```javascript
const PrivateRoute = () => {
  const { isAuthenticated, loading } = useAuth();
  
  if (loading) return <LoadingSpinner />;
  
  return isAuthenticated ? <Outlet /> : <Navigate to="/login" />;
};
```

- **React Router v6 pattern:** Dùng `<Outlet />` để render nested routes
- **isAuthenticated:** Check từ Context (có user + token hay không)
- **Loading state:** Tránh flash redirect khi app đang check authentication
- **Client-side protection:** Chỉ là UX, vẫn cần backend middleware để bảo vệ API

**🔐 Defense in Depth:**
- **Frontend Protected Routes:** UX tốt, ngăn user vào trang không được phép
- **Backend Middleware:** Security thật sự, vì frontend có thể bypass
- Cả 2 layers cùng hoạt động = Secure + Good UX

---

### 7. **Error Handling** 
**🎯 Mục đích:** Xử lý lỗi tập trung, không expose sensitive information  
**👤 Lợi ích cho người dùng:** Thông báo lỗi dễ hiểu, không thấy stack trace đáng sợ  
**🔒 Bảo vệ khỏi:** Information disclosure, debugging info leak
```javascript
// Centralized error middleware
export const errorHandler = (err, req, res, next) => {
  // Mongoose duplicate key
  if (err.code === 11000) {
    return res.status(400).json({ message: 'Email đã tồn tại' });
  }
  
  // Mongoose validation error
  if (err.name === 'ValidationError') {
    const message = Object.values(err.errors).map(val => val.message);
    return res.status(400).json({ message });
  }
  
  res.status(500).json({ 
    message: err.message || 'Lỗi server',
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  });
};
```

- **Centralized error handling:** Một nơi xử lý tất cả errors → Consistent response format
- **Generic error messages:** Không expose database structure, code paths
- **Development vs Production:** Show stack trace trong dev để debug, hide trong production
- **Specific error types:** Duplicate key, validation error → User-friendly messages

**🔐 Ví dụ:**
```javascript
// ❌ Bad: "MongoError: E11000 duplicate key error collection: users index: email_1"
// ✅ Good: "Email đã tồn tại"

// ❌ Bad: Show full stack trace to user
// ✅ Good: Log to server, show friendly message to user
```

---

### 8. **Password Reset Security** 
**🎯 Mục đích:** Cho phép user reset password an toàn khi quên  
**👤 Lợi ích cho người dùng:** Lấy lại tài khoản dễ dàng mà không lo bị hack  
**🔒 Bảo vệ khỏi:** Brute force token, token replay attacks, account takeover
```javascript
// Tạo token reset password (6 số, hết hạn sau 10 phút)
userSchema.methods.createPasswordResetToken = function() {
  const resetToken = Math.floor(100000 + Math.random() * 900000).toString();
  this.resetPasswordToken = bcrypt.hashSync(resetToken, 10);
  this.resetPasswordExpire = Date.now() + 10 * 60 * 1000;
  return resetToken;
};
```

- **6-digit token:** Dễ nhập (UX), đủ an toàn với timeout ngắn (10 phút)
- **Hash token trước khi lưu DB:** Ngay cả token cũng không lưu plain text
- **10 phút expiration:** Cân bằng UX (đủ thời gian nhập) vs Security (limit brute force)
- **One-time use:** Token bị xóa sau khi reset thành công

**🔐 Attack scenarios bị chặn:**
1. **Brute force:** 1,000,000 combinations / 10 phút = Không khả thi
2. **Token reuse:** Token deleted after use → Cannot reuse
3. **Token theft:** Hash stored in DB → Không thể dùng trực tiếp
4. **Old token:** Expired tokens automatically invalid

**🔄 Flow hoàn chỉnh:**
```
User quên password → Nhập email → Server gửi 6-digit code qua email
→ User nhập code + new password → Server verify (hash compare + check expire)
→ Update password + delete token → User login với password mới
```

---

### 🛡️ **Tổng Quan Lợi Ích Bảo Mật**

| Kỹ thuật | Bảo vệ người dùng | Bảo vệ hệ thống | Impact nếu thiếu |
|----------|-------------------|-----------------|------------------|
| JWT Auth | Chỉ mình tôi xem data của tôi | Không lưu session trên server | Ai cũng xem được data của nhau |
| Password Hashing | Admin không thấy password | DB leak không lộ password | Hacker có DB = có tất cả passwords |
| Protected Routes | Không vào được trang không phép | Chặn unauthorized requests | Bypass frontend = full access |
| Input Validation | Không bị lỗi do dữ liệu sai | Ngăn injection attacks | XSS, NoSQL injection thành công |
| CORS | Website lạ không gọi được API | Chỉ frontend chính thức gọi API | Mọi website đều gọi được API |
| Error Handling | Thông báo lỗi dễ hiểu | Không lộ code structure | Hacker biết cấu trúc hệ thống |
| Token Expiry | Auto logout khi token cũ | Limit thời gian token bị steal | Token đánh cắp dùng mãi mãi |
| Password Reset | Lấy lại account an toàn | Ngăn account takeover | Brute force reset token |

---

> "Trong dự án finance manager, tôi đã áp dụng nhiều layers bảo mật vì đây là app xử lý dữ liệu tài chính nhạy cảm:
>
> **Layer 1 - Authentication:** JWT token-based authentication để verify user identity. Token expire sau 7 ngày để cân bằng UX và security.
>
> **Layer 2 - Password Protection:** Bcrypt hashing với 10 salt rounds. Password không bao giờ lưu plain text, kể cả khi DB bị hack thì hacker cũng không dùng được.
>
> **Layer 3 - Authorization:** Middleware protection trên mọi protected routes. Frontend có route guards, backend có middleware verify token.
>
> **Layer 4 - Input Validation:** Mongoose schema validation + regex để ngăn NoSQL injection và đảm bảo data integrity.
>
> **Layer 5 - CORS & Error Handling:** CORS chặn unauthorized domains, error handling không expose sensitive info.
>
> Mỗi layer đều có mục đích cụ thể và bổ trợ cho nhau, tạo thành defense in depth strategy."

**Câu hỏi: "Tại sao dùng JWT thay vì session?"**

> "JWT phù hợp với RESTful API vì:
> 1. **Stateless** - Server không cần lưu session, dễ scale horizontal
> 2. **Cross-platform** - Dùng được cho web, mobile, desktop
> 3. **Self-contained** - Token chứa thông tin user, giảm database queries
> 4. **Decentralized** - Microservices có thể verify token độc lập
>
> Trade-off là không thể revoke token ngay (phải đợi expire), nhưng với ứng dụng này thì benefit > drawback."

**Câu hỏi: "Làm sao ngăn chặn password breach?"**

> "Tôi áp dụng multiple layers:
> 1. **Bcrypt hashing** - One-way, cannot reverse engineer
> 2. **Salt** - Mỗi password có unique salt, rainbow table attacks không work
> 3. **select: false** - Password field không return qua API
> 4. **10 rounds** - Đủ chậm để chống brute force nhưng không ảnh hưởng UX
>
> Ngay cả khi database bị leak, attacker chỉ thấy hash. Brute force 1 password với bcrypt mất hàng năm với hardware thông thường."

---
- [x] **JWT token authentication** - Stateless authentication cho RESTful API
- [x] **Password hashing với bcrypt** - 10 salt rounds, one-way encryption
- [x] **Protected API routes** - Middleware verify token trên mọi sensitive endpoints
- [x] **Input validation (Mongoose)** - Schema-level validation, required fields
- [x] **Email format validation** - Regex pattern matching
- [x] **CORS configuration** - Whitelist specific origin only
- [x] **Error handling middleware** - Centralized, không expose sensitive info
- [x] **Token expiration handling** - Auto logout sau 7 ngày
- [x] **Password reset với token timeout** - 6-digit OTP, expire 10 phút
- [x] **Secure password storage** - select: false trong schema
- [x] **Auto logout on 401** - Client-side interceptor
- [x] **Environment variables protection** - Sensitive data trong .env

### 📈 **Security Best Practices Applied**
1. ✅ **Never store plain passwords** - Sử dụng bcrypt hashing
2. ✅ **Token-based authentication** - Stateless JWT cho scalability
3. ✅ **Middleware protection** - Protect sensitive routes ở cả frontend & backend
4. ✅ **Input sanitization** - Mongoose validation ngăn injection attacks
5. ✅ **Error messages không expose sensitive info** - Generic error messages
6. ✅ **CORS properly configured** - Chỉ allow specific origin
7. ✅ **Token stored securely** - localStorage với auto-cleanup on 401
8. ✅ **Password reset với timeout** - Ngăn chặn brute force attacks
9. ✅ **Defense in Depth** - Multiple security layers bổ trợ nhau
10. ✅ **Principle of Least Privilege** - Users chỉ access data của chính mình

---

##  Yêu cầu hệ thống

- **Node.js** >= 16.x
- **MongoDB** (Local hoặc MongoDB Atlas)
- **npm** hoặc **yarn**
- **Git** (để clone repository)

---

##  Hướng dẫn cài đặt & chạy dự án

### 1. Clone repository

```powershell
git clone https://github.com/datnqbv/finance_manager.git
cd Quan_ly_chi_tieu
```

### 2. Cài đặt Backend

```powershell
cd server
npm install

# Tạo file .env từ .env.example
cp .env.example .env

# Cấu hình file .env với thông tin của bạn:
# MONGODB_URI=mongodb://localhost:27017/finance_manager
# hoặc MongoDB Atlas: mongodb+srv://username:password@cluster.mongodb.net/finance_manager
# JWT_SECRET=your_jwt_secret_key
# PORT=5000
# CLIENT_URL=http://localhost:5173

# Chạy server
npm run dev
```

Server sẽ chạy tại: **http://localhost:5000**

### 3. Cài đặt Frontend

```powershell
cd client
npm install

# Tạo file .env (nếu cần)
cp .env.example .env

# File .env:
# VITE_API_URL=http://localhost:5000/api

# Chạy client
npm run dev
```

Client sẽ chạy tại: **http://localhost:5173**

### 4. Cấu hình MongoDB

#### Option 1: MongoDB Local
```powershell
# Cài đặt MongoDB Community Edition
# Chạy MongoDB service
mongod

# MongoDB sẽ chạy tại mongodb://localhost:27017
```

#### Option 2: MongoDB Atlas (Cloud)
1. Tạo tài khoản tại https://www.mongodb.com/cloud/atlas
2. Tạo cluster miễn phí
3. Lấy connection string và thay vào `MONGODB_URI` trong `.env`
4. Thêm IP của bạn vào whitelist

---

##  Cấu trúc dự án

## Cấu trúc dự án (có chú thích)

```
Quan_ly_chi_tieu/
├── server/                           # Backend (Node.js + Express)
│   ├── src/
│   │   ├── config/
│   │   │   └── database.js          # Kết nối và cấu hình MongoDB cho toàn bộ backend
│   │   ├── controllers/             # Xử lý logic nghiệp vụ cho từng API (gọi model, trả về response)
│   │   │   ├── auth.controller.js           # Đăng ký, đăng nhập, xác thực, profile
│   │   │   ├── transaction.controller.js   # Quản lý giao dịch thu/chi
│   │   │   ├── category.controller.js      # Quản lý danh mục giao dịch
│   │   │   ├── budget.controller.js        # Quản lý ngân sách
│   │   │   ├── goal.controller.js          # Quản lý mục tiêu tài chính
│   │   │   ├── recurring.controller.js     # Quản lý giao dịch định kỳ
│   │   │   ├── stats.controller.js         # Thống kê tổng quan, biểu đồ
│   │   │   └── notification.controller.js  # Quản lý thông báo (nếu có)
│   │   ├── models/                  # Định nghĩa cấu trúc dữ liệu MongoDB
│   │   │   ├── User.model.js                # Thông tin người dùng, avatar, mật khẩu
│   │   │   ├── Transaction.model.js         # Giao dịch thu/chi
│   │   │   ├── Category.model.js            # Danh mục giao dịch
│   │   │   ├── Budget.model.js              # Ngân sách
│   │   │   ├── Goal.model.js                # Mục tiêu tài chính
│   │   │   └── RecurringTransaction.model.js# Giao dịch định kỳ
│   │   ├── routes/                  # Định nghĩa các endpoint API, liên kết controller
│   │   │   ├── auth.routes.js               # Endpoint xác thực, profile
│   │   │   ├── transaction.routes.js        # Endpoint giao dịch
│   │   │   ├── category.routes.js           # Endpoint danh mục
│   │   │   ├── budget.routes.js             # Endpoint ngân sách
│   │   │   ├── goal.routes.js               # Endpoint mục tiêu
│   │   │   ├── recurring.routes.js          # Endpoint định kỳ
│   │   │   ├── stats.routes.js              # Endpoint thống kê
│   │   │   └── notification.routes.js       # Endpoint thông báo (nếu có)
│   │   ├── middleware/
│   │   │   ├── auth.middleware.js           # Kiểm tra JWT, bảo vệ route
│   │   │   └── error.middleware.js          # Xử lý lỗi tập trung cho API
│   │   ├── utils/
│   │   │   └── sendEmail.js                 # Gửi email (quên mật khẩu, thông báo)
│   │   └── index.js                         # Điểm khởi động server, cấu hình Express
│   ├── .env.example
│   ├── .gitignore
│   └── package.json
│
├── client/                           # Frontend (React + Vite)
│   ├── src/
│   │   ├── components/              # Các component giao diện dùng lại nhiều nơi
│   │   │   ├── Layout.jsx                   # Khung layout chung (header, sidebar, main)
│   │   │   ├── PrivateRoute.jsx             # Bảo vệ route, chỉ cho user đã đăng nhập
│   │   │   ├── DarkModeToggle.jsx           # Chuyển đổi dark/light mode
│   │   │   ├── GlobalSearch.jsx             # Tìm kiếm tổng hợp toàn bộ dự án
│   │   │   ├── Pagination.jsx               # Phân trang danh sách
│   │   │   ├── TransactionModal.jsx         # Modal thêm/sửa giao dịch
│   │   │   ├── CategoryModal.jsx            # Modal thêm/sửa danh mục
│   │   │   ├── BudgetModal.jsx              # Modal thêm/sửa ngân sách
│   │   │   ├── GoalModal.jsx                # Modal thêm/sửa mục tiêu
│   │   │   └── RecurringModal.jsx           # Modal thêm/sửa giao dịch định kỳ
│   │   ├── context/                 # Quản lý state toàn cục bằng Context API
│   │   │   ├── AuthContext.jsx              # Quản lý trạng thái đăng nhập, user
│   │   │   ├── ThemeContext.jsx             # Quản lý dark/light mode
│   │   │   ├── TransactionContext.jsx       # Quản lý danh sách giao dịch
│   │   │   ├── CategoryContext.jsx          # Quản lý danh mục
│   │   │   ├── BudgetContext.jsx            # Quản lý ngân sách
│   │   │   ├── GoalContext.jsx              # Quản lý mục tiêu
│   │   │   └── RecurringContext.jsx         # Quản lý giao dịch định kỳ
│   │   ├── pages/                   # Các trang chính của ứng dụng
│   │   │   ├── Dashboard.jsx                # Trang tổng quan, biểu đồ, thống kê
│   │   │   ├── Transactions.jsx             # Trang quản lý giao dịch
│   │   │   ├── Categories.jsx               # Trang quản lý danh mục
│   │   │   ├── Budgets.jsx                  # Trang quản lý ngân sách
│   │   │   ├── Goals.jsx                    # Trang quản lý mục tiêu
│   │   │   ├── RecurringTransactions.jsx    # Trang giao dịch định kỳ
│   │   │   ├── Statistics.jsx               # Trang thống kê chi tiết
│   │   │   ├── Profile.jsx                  # Trang cá nhân, đổi avatar
│   │   │   ├── Login.jsx                    # Trang đăng nhập
│   │   │   ├── Register.jsx                 # Trang đăng ký
│   │   │   └── ForgotPassword.jsx           # Trang quên mật khẩu
│   │   ├── services/                # Giao tiếp với API backend qua axios
│   │   │   ├── api.js                       # Tạo instance axios, cấu hình baseURL ,khởi tạo api
│   │   │   ├── auth.service.js              # Gọi API xác thực, profile
│   │   │   ├── transaction.service.js       # Gọi API giao dịch
│   │   │   ├── category.service.js          # Gọi API danh mục
│   │   │   ├── budget.service.js            # Gọi API ngân sách
│   │   │   ├── goal.service.js              # Gọi API mục tiêu
│   │   │   ├── recurring.service.js         # Gọi API định kỳ
│   │   │   ├── stats.service.js             # Gọi API thống kê
│   │   │   └── notification.service.js      # Gọi API thông báo (nếu có)
│   │   ├── utils/
│   │   │   └── exportUtils.js               # Xuất báo cáo PDF/Excel
│   │   ├── App.jsx                          # Khởi tạo router, context provider
│   │   ├── main.jsx                         # Điểm khởi động React app
│   │   └── index.css                        # File CSS gốc
│   ├── index.html
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   └── package.json
│
├── README.md                               # Tài liệu dự án, hướng dẫn sử dụng
```

---

### Mối liên hệ giữa các file/thư mục

- **server/src/controllers** gọi **models** để thao tác dữ liệu, trả về kết quả cho **routes**.
- **routes** định nghĩa các endpoint, liên kết controller với URL API.
- **middleware** bảo vệ route (auth), xử lý lỗi tập trung (error).
- **utils/sendEmail.js** được controller gọi khi cần gửi email (quên mật khẩu, thông báo).
- **client/src/components** là các khối giao diện dùng lại ở nhiều trang.
- **context** cung cấp state cho các component và page, giúp chia sẻ dữ liệu toàn app.
- **pages** là các màn hình chính, mỗi page thường dùng nhiều component và context.
- **services** là nơi gọi API backend, được các context và page sử dụng để lấy/gửi dữ liệu.
- **utils/exportUtils.js** được dùng ở các page/component để xuất báo cáo.
- **App.jsx/main.jsx** là điểm khởi động, kết nối router, context, và render layout.


-  Mật khẩu được mã hóa bằng **bcryptjs** (salt rounds: 10)
-  Xác thực bằng **JWT token** (expires: 7 days)
-  Protected routes với middleware
- Validation dữ liệu đầu vào
-  Error handling tập trung
-  CORS configuration
- Environment variables (.env)
-  Tự động logout khi token hết hạn (401)

---



### ✅ **Điểm Mạnh Kỹ Thuật**
1. **Full-stack MERN Development**
   - MongoDB + Express + React + Node.js
   - RESTful API design pattern
   - MVC architecture

2. **Security Implementation**
   - JWT authentication
   - Password hashing (bcrypt)
   - Protected routes & middleware
   - CORS configuration
   - Input validation

3. **Modern Frontend**
   - React 18 với Hooks & Context API
   - React Router v6 (Protected Routes)
   - Axios interceptors
   - Responsive UI (TailwindCSS)

4. **Professional Practices**
   - Git version control
   - Environment variables
   - Error handling patterns
   - Code organization (MVC)
   - API documentation


```


1. **"Bạn đã implement authentication như thế nào?"**
   - JWT token-based authentication
   - Bcrypt password hashing với 10 salt rounds
   - Middleware protection cho protected routes
   - Auto logout khi token expired

2. **"Làm sao bạn bảo vệ API endpoints?"**
   - Middleware `protect` verify JWT token
   - CORS configuration với origin cụ thể
   - Input validation với Mongoose schema
   - Error handling không expose sensitive info

3. **"Bạn xử lý password security ra sao?"**
   - Pre-save hook hash password với bcrypt
   - Password field có `select: false` trong schema
   - Không bao giờ trả password trong API response
   - Password reset với token timeout 10 phút

4. **"Frontend giao tiếp với Backend như thế nào?"**
   - Axios instance với baseURL configuration
   - Request interceptor tự động thêm Bearer token
   - Response interceptor handle 401 (auto logout)
   - Context API quản lý authentication state

---

##  API Endpoints

### Authentication
- `POST /api/auth/register` - Đăng ký
- `POST /api/auth/login` - Đăng nhập
- `GET /api/auth/me` - Lấy thông tin user
- `PUT /api/auth/profile` - Cập nhật profile
- `POST /api/auth/forgot-password` - Quên mật khẩu
- `POST /api/auth/reset-password` - Đặt lại mật khẩu

### Transactions
- `GET /api/transactions` - Lấy danh sách giao dịch
- `POST /api/transactions` - Thêm giao dịch
- `PUT /api/transactions/:id` - Sửa giao dịch
- `DELETE /api/transactions/:id` - Xóa giao dịch

### Categories
- `GET /api/categories` - Lấy danh sách danh mục
- `POST /api/categories` - Thêm danh mục
- `PUT /api/categories/:id` - Sửa danh mục
- `DELETE /api/categories/:id` - Xóa danh mục

### Budgets
- `GET /api/budgets` - Lấy danh sách ngân sách
- `POST /api/budgets` - Thêm ngân sách
- `PUT /api/budgets/:id` - Sửa ngân sách
- `DELETE /api/budgets/:id` - Xóa ngân sách
- `GET /api/budgets/status` - Lấy trạng thái ngân sách
- `GET /api/budgets/alerts` - Lấy cảnh báo

### Goals
- `GET /api/goals` - Lấy danh sách mục tiêu
- `POST /api/goals` - Thêm mục tiêu
- `PUT /api/goals/:id` - Sửa mục tiêu
- `DELETE /api/goals/:id` - Xóa mục tiêu
- `POST /api/goals/:id/add-amount` - Thêm tiền vào mục tiêu
- `GET /api/goals/stats` - Thống kê mục tiêu

### Recurring Transactions
- `GET /api/recurring` - Lấy danh sách giao dịch định kỳ
- `POST /api/recurring` - Thêm giao dịch định kỳ
- `PUT /api/recurring/:id` - Sửa giao dịch định kỳ
- `DELETE /api/recurring/:id` - Xóa giao dịch định kỳ
- `POST /api/recurring/:id/execute` - Thực hiện thủ công
- `GET /api/recurring/upcoming` - Lấy giao dịch sắp tới

### Statistics
- `GET /api/stats/summary` - Tổng quan tài chính
- `GET /api/stats/monthly` - Thống kê theo tháng
- `GET /api/stats/categories` - Thống kê theo danh mục

---

##  Troubleshooting

### MongoDB connection error
- Kiểm tra MongoDB đã chạy: `mongod`
- Kiểm tra connection string trong `.env`
- Nếu dùng MongoDB Atlas, kiểm tra IP whitelist

### Port already in use
- Backend: Đổi `PORT` trong `.env`
- Frontend: Đổi port trong `vite.config.js`

### CORS errors
- Kiểm tra `CLIENT_URL` trong backend `.env`
- Đảm bảo frontend đang chạy ở đúng port

### JWT token errors
- Kiểm tra `JWT_SECRET` trong `.env`
- Xóa token cũ trong localStorage và đăng nhập lại












