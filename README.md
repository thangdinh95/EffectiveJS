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
- **Namespace** là gì: namespace có thể hiểu đơn giản là vùng khai báo phạm vi. Ví dụ
```
var foo = {};
    foo.name = "Effective JS";
    foo.greeting = function() {
        return "Welcome " + foo.name;
    };
```

- Việc định nghĩa một *global variables* sẽ làm ảnh hưởng đến namespace chung mà được đặt bởi mọi người trong cùng dự án, dẫn đến việc bị xung đột tên biến,... Globals sẽ đi trái ngược lại với tiêu chí modular.  Chúng sẽ dẫn đến việc gom các components riêng rẽ một cách không cần thiết trong 1 program. 

Nó có thể thuận tiện ban đầu, 'cứ code đã, lát tổ chức lại sau' tuy nhiên 1 lập trình viên giỏi sẽ phải luôn thường trực để ý đến cấu trúc của chương trình, luôn liên tục nhóm những function liên quan lại và tách những components không liên quan ra như là một phần trong khâu xử lý program.

- Nhưng vì global là cách duy nhất để tách các components của một chương trình JS, nên việc dùng nó là không tránh khỏi. Một component hoặc thư viện phải định nghĩa một global name để các phần khác trong code có thể sử dụng nó. Nếu không, thì tốt nhất là để local variables. 

- Dĩ nhiên là có thể viết 1 chương trình mà chả cần gì ngoài glabal var, nhưng nó lại nảy sinh ra nhiều vấn đề:
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

- JS's global namespace thì cũng được biết đến là global object, cái mà có thể truy cập ở đầu mỗi chương trình như là giá trị ban đầu của từ khóa *'this'*. Global object còn có thể kể đến biến *window*. 

- Có 2 kĩ thuật để tạo ra global variable: 

Global scope: tạo bằng var
```
var str = "hello"; // global scope
```

Global object:
```
var str = {};
str.greeting = "hello";
```

Cả 2 đều có thể sử dụng được, tuy nhiên khai báo var sẽ làm rõ hơn chuyển tải ý nghĩa của một scope.

## Item 9: Luôn luôn khai báo local variable:

- Thay vì báo lỗi thì một chương trình mà gán cho một biến mà nó không liên kết chặt chẽ sẽ dẫn đến việc JS tự gán nó là một global variable mới. 
```
function plus(x,y) {
  z = 4; //global variable
  return (x + y) - 4;
}
z; //4

function plus(x,y) {
  var z = 4; //local variable
  return (x + y) - 4;
}
z; //z is not defined 
```

- Tạo ra global variable một cách cố ý là một cách làm không hay, tuy nhiên việc tạo ra global variable một cách tình cờ là một thảm họa. Chính vì điều này, nhiều lập trình viên sử dụng *lint* tool để tìm ra những style không tốt hoặc những bugs có thể gặp phải. 

## Item 10: Tránh *with*

## Item 11: Làm quen với Closures
- Một closure là một inner function (hàm khai báo bên trong một hàm khác), nó có thể truy cập tới các biến của outer function (hàm chứa inner function) - scope chain.

- Có 3 điều cần phải biết khi nhắc đến Closures:

1. JS cho phép gọi đến *variable* mà nó được định nghĩa bên ngoài function hiện tại
```
function isHandsome() {                 //outer function
  var handsome = "handsome"; 
  function person(name) {               //innder function
    return name + " is so " + handsome; //handsome là var bên ngoài function person()
  }
  return person("Thang"); 
}
isHandsome(); // "Thang is so handsome"
```

2. Function có thể gọi đến *var* được định nghĩa ở function bên ngoài (outer function) của những function bên ngoài nữa.
- Điều này có nghĩa là ta có thể trả về một function bên ngoài.
```
function isHandsome() {
  var handsome = "handsome"; 
  function person(name) {
    return name + " is so " + handsome;
  }
return person
}
var who = isHandsome();
who("Thang"); //"Thang is so handsome"
who("Bach"); //"Bach is so handsome"
who("Nam"); //"Nam is so handsome"
```
- Có thể thấy, thay vì gọi hàm person("Thang") ở bên trong outer function, thì function isHandsome tự return person. 
Vì thế mà giá trị *who* ở bên trong function *person*, và việc gọi *who* cũng sẽ gọi đến *person*. Và mặc dù function *isHandsome* đã return nhưng mà person vẫn có thể nhớ được biến *handsome*

Cũng như function ở trên, dưới đây sẽ tạo ra 2 function riêng biệt, 
```
function isHandsome(howHandsome) {
  function person(name) {
    return name + " is so " + howHandsome;
  }
return person;
}
var person1 = isHandsome("super handsome");
person1("Nam"); //Nam is so super handsome
var person2 = isHandsome("super super handsome!");
person2("Nghia"); //Nghia is so super super handsome!
```

3. Closure có thể update giá trị của outer variable (truy cập đến các biến global). 
Closure lưu trữ *references* đến những outer variables thay vì là copy nó. 
```
function box() {
   var val = undefined; 
   return {
      set: function(newVal) { val = newVal; }, 
      get: function() { return val; },
      type: function() { return typeof val; }
   }; 
}

var b = box();
b.type(); // "undefined" 
b.set(98.6);
b.get(); // 98.6 
b.type(); // "number"
```
## Item 12: Variable Hoisting
- Chúng ta nên hiểu khai báo biến trong JS theo 2 hướng: khai báo và gán. JS sẽ *"hoist"* (kéo) phần khai báo lên đầu mỗi
function bao quanh nó. Điều này có nghĩa là, Hoisting là vấn đề liên quan đến cách khai báo biến trong Javascript. Nó liên quan đến việc trong Javascript bạn có thể sử dụng một biến mà không cần phải định nghĩa trước, vì vậy để chương trình chạy chuẩn thì bạn phải khai báo biến trước khi sử dụng và đặt nó phía trên cùng phạm vi của biến.

- Lưu ý
```
function yourname() {
    return "your name is " + x;
	   var x = "Thang"; 
}
yourname(); //your name is undefined
```
Kết quả trả về sẽ là *undefined* vì JS sẽ chỉ hoist phần khai báo var x lên đầu:
```
function yourname() {
    var x
    return "your name is " + x; // vì x chưa được gán giá trị nên trả về là undefined
	   x = "Thang"; 
}
yourname(); //your name is undefined
```

## Item 13: Sử dụng Immediately Invoked Function Expressions (hàm thực thi tức thời) để tạo ra Local Scopes
- Hãy nhìn vào ví dụ dưới đây:
```
function wrapElements(a) {
   var result = [], i, n;
   for (i = 0, n = a.length; i < n; i++) {
      result[i] = function() { return a[i]; }; 
   }
   return result; }
var wrapped = wrapElements([10, 20, 30, 40, 50]); 
var f = wrapped[0];
f(); // undefined
```
Kết quả sẽ trả về là undefined.

- Để giải quyết vấn đề thì cần tạo một local scope bằng cách tạo 1 function nằm bên trong và gọi nó:
```
function wrapElements(a) {
   var result = [];
   for (var i = 0, n = a.length; i < n; i++) {
       (function(j) {
          result[i] = function() { return a[j]; };
       })(i); 
    }
    return result; 
}
```

## Item 14: Named function expressions

- Function expresstion có 2 loại đó là Named function expresstion (function expression có tên) và Anonymous function expresstion (function expression không tên)

Named function expresstion (NFE)
```
var x = function hello(name) { //function hello
   return "hello " + name;
}
```

Anonymous function expresstion (AFE)
```
var x = function(name) { //function không có tên
   return "hello " + name;
}
```

Về các hoạt động giữa NFE và AFE hoàn toàn giống nhau. Khác biệt duy nhất đó là NFE sẽ bind tên của nó như là một local varible nằm trong function
```
var f = function find(tree, key) { 
    if (!tree) {
        return null; 
    }
    if (tree.key === key) { 
        return tree.value;
    }
    return find(tree.left, key) ||
           find(tree.right, key); // local var
};
```

- Vậy sự khác nhau giữa function declaration (FD) và function expresstion (FE) là gì

Function declaration hiểu đơn giản là khai báo một function. Và chúng ta có thể gọi nó bất cứ lúc nào. Quay lại ví dụ trên thì FD sẽ được viết như sau:
```
function hello(name) {
   return "hello " + name;
}

hello("thang") // "hello thang"
```

Nhưng với một function expresstion thì ta ko thể gọi nó bên ngoài scope cha
```
var f = function hello(name) {
   return "hello " + name;
}

hello("thang") // ReferenceError: hello is undefined;
```

Tuy nhiên đối với resursion (việc sử dụng lại) thì dùng NFE chưa hẳn là hữu ích

```
var f = function find(tree, key) { 
    if (!tree) {
        return null; 
    }
    if (tree.key === key) { 
        return tree.value;
    }
    return f(tree.left, key) ||
           f(tree.right, key); // không cần phải gọi đến find() mà chỉ cần gọi đến outer scope's name
};
```

hoặc

```
function find(tree, key) { 
    if (!tree) {
        return null; 
    }
    if (tree.key === key) { 
        return tree.value;
    }
    return find(tree.left, key) ||
           find(tree.right, key); 
};

var f = find;
```

## Item 15: Cảnh giác với unport scope trong Block-local function declarations

- Không có chuẩn mực nào cho việc khai báo một function bên trong một local block. 

Nó là chuẩn khi nest một khai báo function nằm trên cùng một function khác.
```
function f() { return "global"; } 

function test(x) {
    function f() { return "local"; }
    var result = []; 
    if (x) {
        result.push(f());
    }
    result.push(f());
    return result; 
}
test(true); // ["local", "local"] 
test(false); // ["local"]
```

- Nhưng nó hoàn toàn là một câu chuyện khác nếu chúng ta chuyển *f* vào local block:

```
function f() { return "global"; }

function test(x) { 
    var result = [];
    if (x) {
        function f() { return "local"; } // block-local
        result.push(f());
    }
    result.push(f());
    return result; 

}
test(true); // ? 
test(false); // ?
```

Kết quả sẽ không trả về *["local", "global"]* và *["global"]*. 

Cách tốt nhất là cần viết portable function để tránh đặt khai báo biến vào local block như trên. 

```
function f() { return "global"; }

function test(x) {
   var g = f, result = []; 
   if (x) {
       g = function() { return "local"; } 
       result.push(g());
   } 
   result.push(g()); 
   return result;
}

test(true); // ["local", "local"] 
test(false); // ["local"]
```

## Item 16: Tránh tạo local variables với *eval*
- Xem xét ví dụ dưới đây

```
function text(x) {
   eval("var y = global;"); //dynamic binding
   return y;
}

test("hello") //"hello"
```

Khai báo var (ở ví dụ trên chính là 'var y = global;') chỉ được thực thi khi hàm *eval* được gọi.

- Đặt *eval* vào điều kiện if khiến cho các biến chạy chỉ khi điều kiện if xảy ra:

```
var y = "local";
function text(x) {
   if (x) {
       eval("var y = global;");
   }
   return y;
}

test("true") //"global"
test("false") //"local"
```

- Tuy nhiên khi đặt thuộc tính động (hàm ý nghĩa là có thể thay đổi được, không cố định) thì luôn là một ý tưởng tồi. 

```
var y = "global"; 
function test(src) {
   eval(src); // may dynamically bind
   return y; 
}

test("var y = 'local';"); // "local" 
test("var z = 'local';"); // "global"
```

Đó là vì code lúc này sẽ không an toàn: nó cho phép các phép gọi bên ngoài(*test("var y = 'local';");*) được quyền thay đổi scope bên trong function test.

- Cách đơn giản để đảm bảo rằng *eval* sẽ không ảnh hưởng đến outer scope đó là sử dụng IIFE - chạy eval bên trong nested scope :

```
var y = "global"; 
function test(src) {
    (function() { eval(src); })();
    return y; 
}

test("var y = 'local';"); // "global" 
test("var z = 'local';"); // "global"
```

## Item 17: indirect eval vs direct eval
- Direct eval

```
var x = "global"; 
function test() {
   var x = "local";
   return eval("x"); // direct eval 
}

test(); // "local"
```

- Indirection eval

```
var x = "global"; 
function test() {
   var x = "local";
   var f = eval;
   return f("x"); // indirect eval
}

test(); // "global"
```

# Chaper 3: Function
## Item 18: Sự khác nhau giữa Function, Method, và Constructor:

- Function:
```
function sayHi (name) {
    return "Hi, " + name;
}

sayHi(Thang); // "Hi, Thang"
```

- Method: Method là dạng function nằm trong 1 object
```
var singer = {
    name: "Beyonce",
    sayHi: function() {             //method
       return "Hi, " + singer.name;
    }
}

singer.sayHi() // "Hi, Beyonce"
```

Ngoài ra khi sử dụng method, nó có thể gọi đến một function bên ngoài và sử dụng thuộc tính trong object đó:

```
function sayHi() {
    return "Hi, " + this.name;
}

var singer = {
    name: "Beyonce",
    hello: sayHi     //method giống chức năng như function sayHi
}

var rapper = {
    name: "CL",
    hello: sayHi     //method giống chức năng như function sayHi
}

singer.hello(); // "Hi, Beyonce"
rapper.hello(); // "Hi, CL"
```

- Constructor: 

```
function User(name, password) { 
         this.name = name; 
	 this.password = password;
}
```

Gọi một User với các parameters mới thì nó được xem như *consttructor*

```
var u = new User ("Thang", "1234qwe");
u.name; //"Thang"
u.password; //"1234qwe"
```

## Item 19: Higher-Order Functions

- Higher-order functions là gì: đó là 1 function mà lấy functions khác làm arguments (còn gọi là callback function) hoặc trả về kết quả là một function.

Higher-order function có thể giúp code đơn giản hơn.

Với loop ta có thể viết
```
var names = ["Fred", "Wilma", "Pebbles"];
var upper = [];
for (var i = 0, n = names.length; i < n; i++) {
    upper[i] = names[i].toUpperCase();
}
upper; // ["FRED", "WILMA", "PEBBLES"]
```

Nhưng mình cũng có thể bỏ loop mà chỉ implement từng element:
```
var names = ["Fred" ,"Wilma" ,"Pebbles"];
var upper = names.map(function(name)){
    return name.toUpperCase();
});

upper; // ["FRED", "WILMA", "PEBBLES"]
```

- Higher-order chính là ý tưởng đã tạo nên các hàm thông dụng như apply, map, hay filter và tận dụng nó chính là một cách tuyệt vời để chúng ta áp dụng được các nguyên lý DRY - sử dụng tối đa các hàm sẵn có trong một context mới.

Ta có thể viết hàm chuyển đổi từ lower case > uppder case như sau:
```
var names = ["thang", "hai", "hung"];
var upper = [];
for (var i = 0, n = names.length; i< n; i++) {
    upper[i] = names[i].toUpperCase();
}

upper; // ["THANG", "HAI", "HUNG"]
```

Tuy nhiên ta có thể bỏ for loop thay vào đó là map() function
```
var names = ["thang", "hai", "hung"];
var upper = names.map(function(transform){
    return transform.toUpperCase;
});

upper; // ["THANG", "HAI", "HUNG"]
```

## Item 20: Sử dụng *call* để gọi đến methods với custom receiver

alskdf
