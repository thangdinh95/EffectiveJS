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
3 + true; // 4
```
- Các toan tử như -, *, /, và % sẽ chuyển hết từ argument sang số trước khi tính tóan. 
- Riêng phép + đc dùng như là phép cộng trong toan tử hoặc là kết nối chuỗi lại với nhau, phụ thuộc vào loại datatype là gì
```
2 + 3; //5
```
```
"hello" + "world"; //result is "hello world";
```
- Kết hợp số với chuỗi:
```
"2" + 3 //"23"
```
```
3 + "2" //"23"
```
```
1 + 2 + “3”; // “33”
```
```
1+ 2 + “3” + 4; // “334”
```
```
17 * “3”; //51
```
- Coercion cũng có thể ẩn đi lỗi. Một biến mà là *null* sẽ ko bị fail trong toán tử, nó sẽ tự động convert ra là 0
```
var x = null;
x + 3; //3
```
- Biến *undefined* sẽ convert ra giá trị NaN (not a number) 
```
var x;
x + 3; // NaN
```
- null vs undefined
  - null:
    - null là empty và không tồn tại giá trị nào.
    - null phải được gán 
```
var x = null;
console.log(x); // null
```
  - undefined
    - một biến đã được khai báo nhưng không được định nghĩa
```
var y;
console.log(y); // undefined
```
- Cách để check xem giá trị đó có phải là NaN hay không: Vì NaN là giá trị duy nhất của JS được xem như là không bằng chính nó, nên ta có thể test xem nó có bằng chính nó hay không. Nếu trả về true => NaN Ví dụ
```
var a = NaN;
a !== a; //true;
var b = "hello";
b !== b; //false;
var c = 4;
c !== c; //false;
```
- Objects được convert về string bằng method toString
```
"the Math object: " + Math; //"the Math object: [object Math]"
"the JSON object: " + JSON; // "the JSON object: [object JSON]"
typeof Math; //object
```
- Objects được convert về number bằng method valueOf
```
2 * { valueOf: function() { return 3; } }; // 6
```
- Khi object chứa cả toString và valueOf, nó sẽ chọn valueOf
```
var obj = {
  toString: function() {
    return "hey";
  },
  valueOf: funtion() {
    return 21;
  }
}

"object " + obj; // "object 21"
```
- Sử dụng typeof hoặc so sánh đối với *undefined*
  - DON'T
```
function (x, y) { //function này ignore mọi falsy argument trong đó có cả 0 
  if (!x) {
    x = 320;
  }
  if (!y) {
    y = 480;
  }
  return {x: x, y: y};
}

point(10, 21); // {x: 10, y: 21}
point(0, 0); // {x: 320, y: 480} 
```

        - DO
```
function (x, y) {
  if (typeof x === "undefined") {
    x = 320;
  }
  if (typeof y === "undefined") {
    y = 480;
  }
  return {x: x, y: y};
}

point(10, 21); // {x: 10, y: 21}
point(0, 0); // {x: 0, y: 0}
point(); // {x: 320, y: 480}
```
