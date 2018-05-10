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

## Item 3: Cảnh giác với Implicit Coercions (Ép buộc ngầm):
- Coercion có nghĩa là gì: là quá trình chuyển đổi từ datatype này sang datatype khác. Ví dụ từ string > number, object > boolean,... 
- Những loại nào có thể là đối tượng của Coercion: 
  - Primitive (number, string, boolean, null, undefined)
  - object 
- Tại sao lại là Implicit Coercions ?

Coercions có 2 loại: Explicit và Implicit 

Explicit: viết rõ hẳn: Number(3) // 3

Implicit: xảy ra khi apply giá trị với datatype khác loại. 

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

**DON'T**
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
**DO**
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

## Item 4: Sử dụng Primative value thay vì Object Wrappers
- JS có 5 primative values:
  - number
  - string
  - boolean
  - null
  - undefined

- Chúng ta có thể tạo ra một String object wrapper bằng cách:
```
var s = new String("hello");
```
Có thể nối lại với một string khác:
```
s + " world" // "hello world"
```
Lấy kí tự dựa trên index:
```
s[4]; // "o"
```
- Điều khác biệt đó là khi tạo ra một string object wrapper thì đó là một object thực sự
```
typeof "hello" //string
typeof s; //object
```
Chính vì vậy mà ta **không thể so sánh nội dung trong 2 object với nhau** nên thay vì vậy hãy dùng primative string như bình thường


## Item 5: Tránh sử dụng == với Mixed Types
- Khi 2 tham số cùng kiểu data, thì dùng == hay === cũng không khác biệt. Nhưng sử dụng strict equality (===) là cách tốt để người đọc rõ hơn là sẽ ko có sự chuyển đổi nào nữa liên quan đến việc so sánh (từ string sang number,...)
- Coercion rules cho == operator khi tham số là khác loại
  - null == undefined > không có Coercions vì nó luôn luôn là true
  - null or undefined == mọi tham số khác null or undefined > không có Coercions vì nó luôn luôn là false
  - primitive string, number, boolean == date object > primitive sẽ là dạng number, Date object sẽ là dạng object (convert bằng toString sau đó là valueOf)
  - primitive string, number, boolean == non-date object > primitive sẽ là dạng number, Date object sẽ là dạng object (convert bằng valueOf sau đó là toString)
  - Primitive string, number, or boolean == Primitive string, number, or boolean > Primitive sẽ là dạng number 
```
  var d = new Date()
  var str = d.toString(); // "Wed May 09 2018 10:26:52 GMT+0700 (+07)"
  var num = d.valueOf(); // 1525836412668
  
  console.log(d == str) //true vì d được convert sang string 
  console.log(d == num) //false vì d không được convert sang number bằng valueOf
```

## Item 6: Luật dấu chẩm phẩy:
1. Dấu chấm phẩy được chèn vào trước }, sau 1 hoặc nhiều dòng, hoặc cuối mỗi program
2. Không đặt dấu chẩm phẩy trong *for loop*
3. Ta buộc phải dùng (;) giữa các lệnh gắn (bao gồm cả tính toán), lệnh gọi hàm trên cùng một dòng
4. Khi ghép file, phải chèn (;) giữa các script

## Item 7: Nghĩ đến Strings như là một dãy 16 Bit code

# Chap 2: Variable Scope
## Item 8: Giảm việc sử dụng Global Object
- Việc định nghĩa một *global variables* sẽ làm ảnh hưởng đến namespace chung mà được đặt bởi mọi người trong cùng dự án, dẫn đến việc bị xung đột tên biến,... Globals sẽ đi trái ngược lại với tiêu chí modular.  Chúng sẽ dẫn đến việc gom các components riêng rẽ một cách không cần thiết trong 1 program. 

Nó có thể thuận tiện ban đầu để 'cứ code đã, lát tổ chức lại sau' tuy nhiên 1 lập trình viên giỏi sẽ phải luôn thường trực để ý đến cấu trúc của chương trình, luôn liên tục nhóm những function liên quan lại và tách những components không liên quan ra như là một phần trong khâu xử lý program.

- Nhưng vì global là cách duy nhất để tách các components của một chương trình JS, nên việc dùng nó là không tránh khỏi. Một component hoặc thư viện phải định nghĩa một global name để các phần khác trong code có thể sử dụng nó. Nếu không, thì tốt nhất là để local variables. 

- Nó dĩ nhiên là có thể viết 1 chương trình mà chả cần gì ngoài glabal var, nhưng nó lại nảy sinh ra nhiều vấn đề:
```
var i, n, sum; //globals
function averageScore(players) {
sum = 0;
for (i = 0, n = players.length; i < n; i++) {
        sum += score(players[i]);
    }
return sum / n; }

```

averageScore sẽ không làm việc nếu function score có các biến khác với averageScore
```
var i, n, sum; //same globals as averageScore
function averageScore(players) {
    sum = 0;
    for (i = 0, n = players.length; i < n; i++) {
            sum += score(players[i]);
    }
    return sum / n; 
 }
```
Để giải quyết vấn đề này thì ta nên đặt biến này bên trong function cần nó:
```
function averageScore(players) { 
   var i, n, sum;
   sum = 0;
   for (i = 0, n = players.length; i < n; i++) {
        sum += score(players[i]);
   }
   return sum / n; 
}

function score(player) { 
   var i, n, sum;
   sum = 0;
   for (i = 0, n = player.levels.length; i < n; i++) {
       sum += player.levels[i].score;
   }
   return sum; 
}
```

- JS's global namespace thì cũng được biết đến là global object, cái mà có thể truy cập
