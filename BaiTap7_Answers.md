**Câu 1: CI khác CD chỗ nào?**
* **Trả lời:** 
  * **CI (Continuous Integration - Tích hợp liên tục):** Tập trung vào việc tự động hóa quá trình *build* và *test* mỗi khi nhà phát triển (developer) có thay đổi và push code mới lên kho lưu trữ. Mục đích là để phát hiện lỗi sớm.
  * **CD (Continuous Deployment - Triển khai liên tục):** Là bước mở rộng của CI. Sau khi code đã vượt qua quá trình CI (pass toàn bộ test), hệ thống CD sẽ tự động *deploy* (triển khai) phiên bản mới đó lên các môi trường như staging hoặc production.

**Câu 2: Runner là gì? Ai cung cấp runner?**
* **Trả lời:** 
  * **Runner** là một máy chủ (server), máy ảo hoặc container đóng vai trò trực tiếp thực thi các lệnh/công việc (jobs) được định nghĩa trong pipeline. 
  * **Người cung cấp:** Nền tảng GitLab.com cung cấp sẵn các **shared runners** hoàn toàn miễn phí cho tất cả các repository, do đó bạn không cần phải tự cài đặt hay cấu hình máy chủ riêng trừ khi có nhu cầu đặc thù.

**Câu 3: Artifacts dùng để làm gì?**
* **Trả lời:** 
  * **Artifacts** là các tệp tin (files) hoặc thư mục kết quả được tạo ra sau khi một job chạy xong (ví dụ: code đã được compile, file thực thi, file báo cáo test...).
  * **Tác dụng:** Artifacts được dùng để lưu trữ và chia sẻ dữ liệu giữa các jobs hoặc các stages khác nhau trong cùng một pipeline. (Ví dụ: Job `build` tạo ra thư mục code đã biên dịch dưới dạng artifact, sau đó job `deploy` sẽ lấy thư mục artifact đó để đưa lên server).

**Câu 4: Pipeline fail ở stage test, stage deploy có chạy không?**
* **Trả lời:** 
  * **Không.** Các stages trong GitLab CI/CD mặc định chạy theo thứ tự tuần tự. Nếu một stage trước (như `test`) bị lỗi (fail), toàn bộ pipeline sẽ dừng lại ngay lập tức và các stages phía sau (như `deploy`) sẽ không được thực thi. Điều này giúp đảm bảo code lỗi không bao giờ được đưa lên hệ thống thật.

**Câu 5: Làm sao để pipeline chỉ chạy trên branch `main`?**
* **Trả lời:** Bạn sử dụng từ khóa `rules` và gán điều kiện kiểm tra biến môi trường `$CI_COMMIT_BRANCH` trong phần cấu hình của job ở file `.gitlab-ci.yml`. Cụ thể:
  ```yaml
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
  ```
  *(Lưu ý: `$CI_DEFAULT_BRANCH` thường mặc định là nhánh `main` hoặc `master` tùy cấu hình repository).*

**Câu 6: Muốn chạy pipeline trên mọi branch?**
* **Trả lời:** Có 2 cách cơ bản:
  1. **Bỏ hẳn mục `rules`** trong khai báo cấu hình của job (mặc định job sẽ chạy trên mọi thao tác push code).
  2. Hoặc cấu hình `rules` chỉ kiểm tra xem có tồn tại branch hay không bằng cách dùng:
     ```yaml
     rules:
       - if: $CI_COMMIT_BRANCH
     ```