# 1.Git
## 1.1 Quy tắc của Git
Một số quy tắc cần lưu ý : 
* Thực hiện công việc trong một nhánh tính năng
> Tất cả các công việc được thực hiện đơn lẻ trên một chi nhánh riêng chứ không phải là chi nhánh chính. Nó cho phép bạn có thể gửi nhiều pull request mà không có sự nhầm lẫn. Bạn có thể lặp lại mà không làm ảnh hưởng đến các nhánh với code chưa ổn định, chưa hoàn thành
* Sử dụng nhánh `develop`
> Bằng cách này, bạn có thể đảm bảo rằng mã trong master hầu như luôn được thực hiện mà không có vấn đề, và hầu hết được sử dụng trực tiếp cho các bản phát hành(điều này có thể là quá mức cần thiết cho một số dự án)
* Không bao giờ được  đẩy vào nhánh `develop` hoặc `master`.Hãy tạo một Pull Request
> Nó thông báo cho các thành viên trong nhóm rằng họ đã hoàn thành một tính năng.Nó cũng cho phép ta dễ dàng kiểm tra lại code và dành ra diễn đàn thảo luận về tính năng được đề xuất
* Cập nhật nhánh `develop` ở local của bạn và thực hiện rebase trước khi pushing tính năng của bạn và thực hiện một Pull Request.
> Rebasing sẽ hợp nhất các nhánh yêu cầu (`master` hoặc `develop`) và áp dụng các commit đã thực hiện ở local trước đó mà không cần tạo một commit hợp nhất (giả sử không có xung đột).
* Giải quyết xung đột tiềm ẩn trong khi rebasing và trước khi thực hiện Pull Request
* Xóa các nhánh cục bộ và nhánh từ xa sau khi hợp nhất
> Các nhánh của bạn sẽ bị lộn xộn với các nhánh chết.Nó đảm bảo rằng chỉ hợp nhất duy nhất các nhánh trở lại (`master` hoặc `develop`) một lần.Các nhánh đặc trưng chỉ nên tồn tại trong khi công việc vẫn đang trong tiến trình.
* Trước khi thực hiện một Pull Request, hãy đảm bảo rằng các nhánh tính năng của bạn được xây dựng thành công và vượt qua tất cả các kiểm tra (bao gồm kiểm tra code style).
> Bạn sắp xếp code của bạn vào một nhánh ổn định. Nếu thử nghiệm tính năng nhánh của bạn không thành công, sẽ có cơ hội cao là việc xây dựng nhánh đích của bạn không thành công. Ngoài ra, bạn cần áp dụng kiểm tra kiểu mã trước khi thực hiện Pull Request. Nó giúp dễ đọc và làm giảm cơ hội định dạng các bản sửa lỗi được trộn lẫn với những thay đổi thực tế
* Sử dụng tệp tin `.gitignore` .
> Nó đã có danh sách các tập tin hệ thống không nên được gửi với code của bạn vào một kho lưu trữ từ xa. Ngoài ra, nó loại trừ các thư mục cài đặt và các tập tin cho hầu hết các biên tập viên được sử dụng, cũng như các thư mục phụ thuộc phổ biến nhất
* Bảo vệ nhánh `develop` và `master` của bạn.
> Nó bảo vệ các nhánh production-ready của bạn từ những thay đổi bất ngờ và không thể đảo ngược
## 1.2 Quy trình làm việc của Git
Vì hầu hết các lý do trên, chúng tôi sử dụng Feature-branch-workflow với Interactive Rebasing và một số yếu tố của Gitflow(đặt tên và có một nhánh develop).Các bước chính như sau :
* Đối với một dự án mới, khởi tạo kho chứa git trong thư mục dự án. Đối với các tính năng thay đổi tiếp theo,bước này nên được bỏ qua.
```
cd <project directory>
git init
```
* Chuyển sang một nhánh mới
```
git checkout -b <branchname>
```
* Tạo thay đổi 
```
git add
git commit -a
```
> `git commit -a` sẽ bắt đầu một trình soạn thảo cho phép bạn tách riêng chủ thể ra khỏi cơ thể.
* Đồng bộ hóa từ xa để không bỏ lỡ những thay đổi.
```
git checkout develop
git pull
```
> Điều này sẽ cho bạn cơ hội để xử lý các xung đột trên máy tính của bạn trong khi rebasing thay vì tạo một Pull Request có chứa xung đột
* Cập nhật nhánh tính năng của bạn với những thay đổi mới nhất từ develop bằng cách tương tác rebase
```
git checkout <branchname>
git rebase -i --autosquash develop
```
> Bạn có thể sử dụng --autosquash để ẩn các commit của bạn chỉ để 1 commit duy nhất.Không ai muốn nhiều người commit cho một tính năng duy nhất trong việc phát triển nhánh.
* Nếu không có xung đột,hãy bỏ qua bước này . Nếu có ,hãy giải quyết chúng là tiếp tục rebase.
```
git add <file1> <file2> ...
git rebase --continue
```
* Push nhánh của bạn. Rebase sẽ thay đổi lịch sử, do đó bạn sẽ phải sử dụng `-f` để buộc các thay đổi vào nhánh từ xa.Nếu ai đó đang làm việc tại nhánh của bạn,hãy sử dụng phương pháp gây ít tổn hại hơn `--force-with-lease`
```
git push -f
```
> Khi bạn thực hiện một rebase, bạn đang thay đổi lịch sử trên nhánh tính năng của bạn.Kết quả là, Git sẽ từ chối `git push`.Thay vào đó, bạn sẽ sử dụng cờ `-f` hoặc `--force`.
* Thực hiện một Pull Request
* Pull Request sẽ được chấp nhận , sáp nhập và đóng bởi một người đánh giá.
* Loại bỏ một nhánh tính năng cục bộ của bạn nếu bạn đã hoàn tất
```
git branch -d <branchname>
```
Loại bỏ các nhánh mà không còn ở remote
```
git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
```
## 1.3 Viết thông điệp commit tốt
Có một hướng dẫn tốt để  tạo ra cam kết và gắn bó với nó giúp làm việc với Git và cộng tác với người khác dễ dàng hơn rất nhiều.Dưới đây là một số quy tắc của ngón tay cái :
* Tách chủ thể ra khỏi cơ thể với một dòng mới giữa hai người
> Git đủ thông minh để phân biệt dòng đầu tiên của thông điệp commit của bạn dưới dạng tóm tắt của bạn. Trên thực tế, nếu bạn dùng thử git shortlog, thay vì git log, bạn sẽ thấy một danh sách dài và các thông điệp commit ,bao gồm id của commit và bản tóm tắt.
* Giới hạn dòng chủ đề tới 50 ký tự và bao bọc cơ thể ở 72 ký tự.
> Các commit nên được làm tốt , không được rườm rà.
* Viết hoa dòng chủ đề.
* Đừng kết thúc chủ đề với một khoảng thời gian
* Sử dụng [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) trong dòng chủ đề
> Thay vì viết những thông điểm nói về những gì commiter đã làm . Tốt hơn nên xem xét các thông báo này như là hướng dẫn cho những gì sẽ được thực hiện sau khi commit được áp dụng lên kho lưu trữ.
* Sử dụng phần thân của commit message để giải thích nội dung về cái gì và tại sao hơn là giải thích nó như thế nào
