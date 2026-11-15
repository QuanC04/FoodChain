# TỔNG HỢP YÊU CẦU ĐỒ ÁN TỐT NGHIỆP
# HỆ THỐNG QUẢN LÝ CHUỖI NHÀ HÀNG BẾP VIỆT

Phiên bản: Tổng hợp lại từ tài liệu yêu cầu gốc
Mục đích: Làm tài liệu nền để phân tích nghiệp vụ, xác định phạm vi, thiết kế CSDL và xây dựng chức năng.

======================================================================
1. TỔNG QUAN ĐỀ TÀI
======================================================================

Đề tài xây dựng một hệ thống quản lý chuỗi nhà hàng Bếp Việt. Hệ thống phục vụ việc vận hành nhiều chi nhánh, quản lý thực đơn, nhân sự, đơn hàng, giao hàng, khách hàng, khuyến mãi, đánh giá và báo cáo.

Hệ thống có hai phía nghiệp vụ chính:
- Phía nhà hàng: quản lý chi nhánh và các hoạt động vận hành.
- Phía khách hàng: tìm món, đặt món, thanh toán và theo dõi đơn hàng.

Lưu ý: Toàn bộ mã nguồn cũ liên quan đến Billet (sổ chi tiêu, hóa đơn thu chi, OCR...) là phần khung boilerplate và coi như chưa triển khai.

======================================================================
2. MỤC TIÊU HỆ THỐNG
======================================================================

- Quản lý tập trung hoạt động của nhiều chi nhánh nhà hàng.
- Theo dõi tình trạng hoạt động, doanh thu, đơn hàng và nhân viên theo chi nhánh.
- Quản lý danh mục, món ăn, giá và tùy chọn món.
- Quản lý nhân viên, vai trò, phân công chi nhánh, ca làm và quyền truy cập.
- Hỗ trợ khách hàng đặt món qua các kênh bán hàng.
- Quản lý quy trình xử lý đơn và giao hàng.
- Lưu trữ thông tin khách hàng, địa chỉ, lịch sử mua hàng và điểm thành viên.
- Quản lý khuyến mãi, đánh giá và cung cấp số liệu báo cáo.

======================================================================
3. CÁC NHÓM CHỨC NĂNG NGHIỆP VỤ
======================================================================

### 3.1. Quản lý chi nhánh
- Xem danh sách các chi nhánh.
- Theo dõi trạng thái đóng/mở của từng chi nhánh.
- Theo dõi doanh thu, số lượng đơn và số lượng nhân viên theo từng cơ sở.
- Thêm mới và cập nhật thông tin chi nhánh.
- Quản lý địa chỉ, vị trí trên bản đồ và bán kính giao hàng.
- Bật/tắt các kênh bán hàng:
  + Ăn tại quán (DINE_IN).
  + Đặt món trực tuyến (ONLINE).
  + Giao hàng (DELIVERY).

Thông tin chi nhánh:
Mã chi nhánh, tên, địa chỉ, hotline, người quản lý, giờ mở cửa/đóng cửa, bán kính giao hàng, vĩ độ/kinh độ, các kênh được phép hoạt động và trạng thái.

### 3.2. Quản lý thực đơn và món ăn
- Quản lý danh mục món ăn (Phở, Bún, Cơm, Đồ uống, Combo,...).
- Quản lý thông tin món: tên, mô tả, giá bán, giá vốn và hình ảnh.
- Hỗ trợ tùy chọn kích cỡ: Nhỏ, Vừa, Lớn.
- Hỗ trợ topping/đồ ăn kèm (có thể cộng hoặc trừ tiền).
- Kiểm soát trạng thái món theo từng chi nhánh:
  + AVAILABLE: Đang bán.
  + OUT_OF_STOCK: Hết hàng.
  + HIDDEN: Tạm ẩn.

### 3.3. Quản lý nhân sự, ca làm và phân quyền
- Quản lý hồ sơ nhân viên.
- Các vai trò chính:
  + BRANCH_MANAGER: Quản lý chi nhánh.
  + HEAD_CHEF/KITCHEN: Bếp trưởng / Bếp.
  + CASHIER: Thu ngân.
  + WAITER: Phục vụ.
  + SHIPPER: Nhân viên giao hàng.
- Quản lý chi nhánh mà nhân viên được phân công.
- Sắp xếp ca làm trong tuần.
- Quản lý trạng thái làm việc (WORKING, DELIVERING, ON_LEAVE).
- Phân quyền truy cập các chức năng hệ thống (Xử lý đơn, quản lý thực đơn, xem báo cáo, quản lý khuyến mãi,...).

### 3.4. Đặt món, xử lý đơn hàng và giao hàng
**Phía khách hàng:**
- Tìm và xem các món ăn.
- Thêm món vào giỏ hàng.
- Áp dụng mã khuyến mãi.
- Chọn phương thức thanh toán.
- Theo dõi tiến trình giao hàng theo thời gian thực.

**Phía nhà hàng:**
- Tiếp nhận và quản lý luồng đơn hàng.
- Cập nhật trạng thái đơn.
- Gán shipper phụ trách đơn giao hàng.

**Các kênh bán hàng:**
- DINE_IN: Ăn tại quán.
- ONLINE: Đặt món trực tuyến.
- DELIVERY: Giao hàng.

**Vòng đời trạng thái đơn hàng:**
`NEW` -> `CONFIRMED` -> `PREPARING` -> `READY` -> `DELIVERING` -> `COMPLETED` (hoặc `CANCELLED`).

**Thông tin đơn hàng:**
Khách hàng, chi nhánh, shipper (nếu có), khuyến mãi (nếu có), kênh bán, địa chỉ giao hàng, thời gian giao dự kiến, tiền món, phí giao hàng, tiền giảm giá, tổng thanh toán, phương thức/trạng thái thanh toán, trạng thái đơn, thời điểm tạo đơn. Lưu snapshot giá và tên món tại thời điểm đặt.

### 3.5. Quản lý khách hàng (CRM) và thành viên
- Lưu hồ sơ khách hàng, các địa chỉ giao hàng.
- Lịch sử đặt món, tổng số đơn, tổng chi tiêu và điểm tích lũy.
- Phân hạng thành viên: NEW, BRONZE, SILVER, GOLD.

### 3.6. Khuyến mãi, đánh giá và báo cáo
**Khuyến mãi:**
- Tạo và quản lý mã khuyến mãi.
- Loại giảm: giảm theo %, giảm số tiền cố định, miễn phí giao hàng.
- Điều kiện: giá trị đơn tối thiểu, giới hạn số lượt sử dụng, giới hạn đơn hàng đầu tiên, thời gian hiệu lực, phạm vi chi nhánh áp dụng.

**Đánh giá (Review):**
- Đánh giá theo đơn hàng (1 đến 5 sao), nhận xét chi tiết.
- Nhà hàng / quản lý phản hồi đánh giá.

**Báo cáo:**
- Doanh thu, lợi nhuận gộp, giá trị đơn hàng trung bình (AOV), tỷ trọng kênh bán, top món bán chạy.

======================================================================
4. MÔ HÌNH THỰC THỂ CỐT LÕI (17 THỰC THỂ NỀN TẢNG)
======================================================================

1. Branch: Chi nhánh.
2. Employee: Nhân viên.
3. EmployeePermission: Quyền của nhân viên.
4. WorkShift: Ca làm việc và lịch tuần.
5. Category: Danh mục món ăn.
6. Dish: Món ăn.
7. DishOption: Tùy chọn kích cỡ/topping.
8. BranchDish: Trạng thái món tại từng chi nhánh.
9. Customer: Khách hàng.
10. CustomerAddress: Địa chỉ khách hàng.
11. Order: Đơn hàng.
12. OrderItem: Chi tiết món trong đơn.
13. OrderItemOption: Tùy chọn của món đã đặt.
14. OrderStatusLog: Lịch sử trạng thái đơn.
15. Promotion: Chương trình khuyến mãi.
16. PromotionBranch: Chi nhánh áp dụng khuyến mãi.
17. Review: Đánh giá của khách hàng.

======================================================================
5. CÁC NGUYÊN TẮC THIẾT KẾ CẦN TUÂN THỦ (MỤC 8)
======================================================================

1. Tách tài khoản đăng nhập (Auth Account: email, phone, password_hash, role) khỏi hồ sơ Nhân viên (Employee) và Khách hàng (Customer).
2. Phân công chi nhánh linh hoạt (một nhân viên có thể gắn với chi nhánh cụ thể hoặc điều chuyển).
3. Ca làm việc (WorkShift) phân biệt rõ giữa "khung ca mẫu" và "lịch phân ca thực tế theo ngày".
4. Giữ nguyên vẹn lịch sử đơn hàng (Snapshot giá, tên món, tùy chọn lúc đặt món vào OrderItem, không phụ thuộc vào Dish sau này sửa đổi).
5. Quản lý trạng thái món theo từng chi nhánh độc lập (BranchDish).
6. Khuyến mãi có ràng buộc thời gian, ngân sách, số lượt dùng, chi nhánh áp dụng và điều kiện đơn hàng.
