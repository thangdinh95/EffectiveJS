# Chaper 1: Làm quen với Javascript
## Item 1: Biết loại JS nào mà mình đang sử dụng
- Cùng với những phiên bản tiêu chuẩn, thì cũng có nhưng tính năng không tiêu chuẩn mà được hỗ trợ bởi một vài JS code nhưng không phải tất cả. 
Ví dụ: nhiều JS hỗ trợ *const* như là một biến, tuy vậy theo tiêu chuẩn ECMAScript lại không hỗ trợ về syntax hay behavior nào của *const* cả.
- Vì người dùng có thể sử dụng nhiều loại browser với nhiều phiên bản khác nhau nên 1 chương trình web cũng phải được viết kĩ càng để chạy được hoàn chỉnh trên nhiều browser khác nhau.
- ES5 giới thiệu đến phiên bản *strict mode*. Tính năng này là để:
  * ngăn chặn sử dụng, throw errors
  * vô hiệu hóa tính năng có thể gây nhầm lẫn
  * Ngăn chặn 1 số từ có thể dùng làm keyword trong tương lai
- Để dùng *strict mode*, đặt câu sau trên đầu mỗi program:
```
“use strict” ;
```
- Việc sử dụng 1 đoạn string như vậy khiến nó trông kì quặc nhưng đó là backward compatibility (tương thích với cả các phiên bản cũ). Ví dụ ES3 sẽ coi đoạn đó như 1 string bình thường, tuy nhiên những phiên bản ko được hỗ trợ thì sẽ ko chạy được strict mode. 
- Có 1 vấn đề khi dùng strict mode đấy là khi sử dụng “use strict” ở đầu mỗi đoạn script hay function, thì nó sẽ gây ra sự nhạy cảm trong việc kết nối/ ghép các đoạn script lại với nhau. Để giải quyết vấn đề này thì tốt nhất là đừng bao giờ ghép strict files và nonstrict files dù nó cũng hạn chế việc kiểm soát cấu trúc của thư viện hoặc ứng dụng của bạn. Tốt hơn hết, là phải deploy 2 files này riêng biệt, 1 chứa strict files và cái còn lại chứ nonstrict files
- Để giải quyết vấn đề trên, ghép các file lại bằng cách đặt body của chúng vào *immediately invoked function expressions (IIFE)* (hàm thực thi tức thời) Ví dụ:
```
(function() {
  //file1.js
  “use strict”;
    funtion example1() {
      //do something…
    }
 })();
 
(function() {
  //file2.js
  //no strict-mode directive
  function example2() {
    var arguments = [];
    do somthing...
  }
})();
```
## Item 2: Tìm hiểu về số floating trong JS:
- trong JS, chỉ có 1 kiểu số duy nhất.
- kiểu integer trong JS chỉ là tập con của double hơn là một kiểu loại datatype riêng biệt.

## Item 3: Cảnh giác với Implicit Coercions (Ép buộc tuyệt đối):
- Nhiều ngôn ngữ sẽ coi expression như dưới là lỗi, tuy nhiên Javascript vẫn chạy bình thường và trả về kết quả:
```
3 + true; //result is 4
```
- Các
