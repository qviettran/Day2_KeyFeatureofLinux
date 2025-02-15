# Day2_KeyFeatureofLinux
Bài 1: 

1. So sánh Monolithic Kernel và Microkernel

Monolithic Kernel:

- Là một kiến trúc hệ điều hành trong đó toàn bộ các dịch vụ hệ thống như: memory management, process management, file system, device drivers, networking đều chạy trong chế độ mode cao nhất trong hệ thống - kernel mode.
- Điều này có nghĩa tất cả thành phần hệ thống đều có thể truy cập trực tiếp vào phần cứng và giao tiếp với nhau mà không cần thông qua lớp trung gian

Microkernel:

- Là kiến trúc kernel tối giản, chỉ giữ lại các chức năng cơ bản như quản lý bộ nhớ (memory management), lập lịch CPU và cơ chế giao tiếp giữa các tiến trình (IPC)
- Các dịch vụ khác như file system, device drivers, process management đều chạy trong user space, cách ly với kernel

2. So sánh ưu nhược điểm của hai mô hình này về hiệu suất, bảo trì, bảo mật.

# So sánh Monolithic Kernel và Microkernel

| **Đặc điểm**     | **Hạt nhân Vi mô**         | **Hạt nhân Đơn khối**       |
|------------------|---------------------------|-----------------------------|
| **Cấu trúc**      | Kernel lớn, tất cả các thành phần chạy trong chế độ kernel | Kernel nhỏ gọn, chỉ giữ các chức năng tối thiểu, còn lại chạy trong user space |
| **Hiệu quả**      | Nhanh hơn vì mọi thành phần chạy trong kernel mode, không có overhead từ IPC | Chậm hơn do phải giao tiếp giữa các phần module qua IPC |
| **Độ ổn định**    | Kém hơn, vì lỗi trong một module cũng có thể làm sập toàn bộ hệ thống | Cao hơn, vì được chia thành các module, lỗi ở một module sẽ là cục bộ và không ảnh hưởng các module khác |
| **Tính bảo mật**  | Dễ bị tấn công hơn do có nhiều code chạy trong kernel mode | Bảo mật hơn vì ít code chạy ở kernel mode, khó khai thác hơn |
| ***Khả năng mở rộng**| Khó mở rộng, do bất kỳ thay đổi nào cũng cần sửa kernel                    | Dễ mở rộng hơn, chỉ cần cập nhật hoặc thêm module ở user space |
| **Giao tiếp**| Các module giao tiếp trực tiếp, không có chi phí IPC | Giao tiếp qua IPC, gây overhead nhưng giúp cách ly các module |
| **Có thể mở rộng** | Dễ dàng mở rộng           | Phức tạp để mở rộng          |
| **Ứng dụng**    | Phù hợp hệ thống cần hiệu suất cao như server, desktop                    | Phù hợp với hệ thống yêu cầu độ ổn định và tính bảo mật cao                    |

3. Giải thích tại sao Linux sử dụng Monolithic Kernel nhưng vẫn có tính linh hoạt cao.

- Linux không phải một kernel nguyên khối hoàn toàn cứng nhắc. Thay vào đó nó có cấu modular, tách kernel thành các module riêng biệt (vẫn nằm trong kernel space), thay vì giữ tất cả trong kernel từ đầu, chỉ cần tải các module cần thiết vào kernel space, điều này khiến kernel nhỏ gọn hơn, hiệu quả về tài nguyên
- Mỗi module trong kernel space của Linux có thể được tải hoặc gỡ bỏ mà không làm ảnh hưởng đến các phần module khác. Có thể cập nhật hoặc thay đổi một phần của kernel mà không cần dừng toàn bộ hệ thống

Bài 2: Mô hình "Everything as a File" trong Linux

1. Giải thích mô hình "Everything as a File".

- Mô hình "Everything as a File" là một nguyên tắc thiết kế quan trọng trong Unix và Linux. Theo mô hình này, hầu hết mọi thành phần trong hệ thống - bao gồm cả phần cứng, tiến trình và giao tiếp hệ thống đều được biểu diễn dưới dạng tệp
- Vì mọi thứ là tệp, các chương trình có thể dùng cùng một tập hợp hàm (open, read, write) để thao tác với tập tin, thiết bị và dữ liệu hệ thống
- Quản lí thiết bị dễ dàng hơn khi thay vì có các API phức tạp cho mỗi loại thiết bị, chỉ cần đọc/ghi dữ liệu trong /dev/, thay vì sử dụng công cụ đặc biệt để thay đổi cấu hình CPU, chỉ cần sửa một tệp trong /sys/

2. Nêu các đối tượng trong Linux hoạt động như file (ví dụ: thiết bị, tiến trình).

- Linux biểu diễn thiết bị phần cứng dưới dạng các tệp đặc biệt nằm trong thư mục /dev/.
- Thư mục /proc/ chứa các tệp đặc biệt đại diện cho tiến trình đang chạy và thông tin hệ thống.
- Thư mục /sys/ chứa các tệp cấu hình hệ thống, giúp quản lý phần cứng và driver mà không cần khởi động lại.

Bài 3: Cách Linux thực hiện Preemptive Multitasking

1. Giải thích Preemptive Multitasking là gì

- Preemptive Multitasking (đa nhiệm ưu tiên) là một kỹ thuật lập lịch CPU trong đó hệ điều hành có quyền tạm dừng (preempt) một tiến trình đang chạy để nhường CPU cho một tiến trình khác có độ ưu tiên cao hơn hoặc để đảm bảo phân bổ tài nguyên công bằng giữa các tiến trình.
- Ưu điểm: Tránh tiến trình chiếm CPU quá lâu, giúp hệ thống ổn định hơn, hỗ trợ chạy đa nhiệm hiệu quả - mở nhiều ứng dụng mà không treo, phù hợp cho hệ thống thời gian thực
- Nhược điểm: Chi phí chuyển đổi cao hơn vì cần lưu trữ và khôi phục trạng thái tiến trình trước đó, cần có cơ chế đồng bộ hóa tốt để tránh lỗi khi nhiều tiến trình cùng truy cập một tài nguyên

2. Mô tả vai trò của Linux Scheduler trong việc quản lý tiến trình.

- Linux Scheduler (Bộ lập lịch của Linux) là thành phần chịu trách nhiệm quyết định tiến trình nào sẽ chạy tiếp theo trên CPU. Nó đảm bảo rằng tài nguyên CPU được sử dụng hiệu quả, giúp hệ thống vận hành mượt mà, công bằng và đáp ứng nhanh.

# So sánh Preemptive Multitasking và Cooperative Multitasking

| **Tiêu chí**     | **Preemptive Multitasking**         | **Cooperative Multitasking**       |
|------------------|---------------------------|-----------------------------|
| **Quyền kiểm soát CPU**      | Hệ điều hành kiểm soát tiến trình | Tiến trình tự nhường CPU |
| **Cơ chế chuyển đổi**      | Sử dụng timer interrupt để tạm dừng tiến trình | Tiến trình tự gọi yield() hoặc sleep() để nhường CPU |
| **Độ ổn định**    | Ổn định hơn, tránh tiến trình chiếm CPU quá lâu | Có thể gặp lỗi nếu một tiến trình không chịu nhường CPU |

# Mô tả thuật toán Completely Fair Scheduler (CFS) và các yếu tố quyết định scheduling.

- CFS (Completely Fair Scheduler - lập lịch mặc định của Linux) đảm bảo rằng mọi tiến trình được cấp CPU một cách công bằng dựa trên vruntime (thời gian CPU mà tiến trình đã sử dụng).
- Sử dụng thuật toán CFS cho hầu hết các tiến trình, đồng thời hỗ trợ lập lịch thời gian thực khi cần. Có thể điều chỉnh thuật toán lập lịch để tối ưu hóa hiệu suất theo từng trường hợp cụ thể.
# Các yếu tố quyết định scheduling: 
+ Tiến trình nào sử dụng ít CPU hơn sẽ được ưu tiên chạy trước.
+ Mỗi tiến trình có một giá trị vruntime. Scheduler chọn tiến trình có vruntime thấp nhất để chạy.
+ Khi tiến trình chạy, vruntime của nó tăng lên, đến khi một tiến trình khác có vruntime thấp hơn thì sẽ được cấp CPU.