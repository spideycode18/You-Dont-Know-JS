# You Don't Know JS: Types & Grammar
# Chapter 1: Types

Most developers would say that a dynamic language (like JS) does not have *types*. Let's see what the ES5.1 specification (http://www.ecma-international.org/ecma-262/5.1/) has to say on the topic:

> Algorithms within this specification manipulate values each of which has an associated type. The possible value types are exactly those defined in this clause. Types are further sub classified into ECMAScript language types and specification types.
>
> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are Undefined, Null, Boolean, String, Number, and Object.

Now, if you're a fan of strongly typed (statically typed) languages, you may object to this usage of the word "type." In those languages, "type" means a whole lot *more* than it does here in JS.

Some people say JS shouldn't claim to have "types," and they should instead be called "tags" or perhaps "subtypes".

Bah! We're going to use this rough definition (the same one that seems to drive the wording of the spec): a *type* is an intrinsic, built-in set of characteristics that uniquely identifies the behavior of a particular value and distinguishes it from other values, both to the engine **and to the developer**.

In other words, if both the engine and the developer treat value `42` (the number) differently than they treat value `"42"` (the string), then those two values have different *types* -- `number` and `string`, respectively. When you use `42`, you are *intending* to do something numeric, like math. But when you use `"42"`, you are *intending* to do something string'ish, like outputting to the page, etc. **These two values have different types.**

That's by no means a perfect definition. But it's good enough for this discussion. And it's consistent with how JS describes itself.

# A Type By Any Other Name...

Beyond academic definition disagreements, why does it matter if JavaScript has *types* or not?

Having a proper understanding of each *type* and its intrinsic behavior is absolutely essential to understanding how to properly and accurately convert values to different types (see Coercion, Chapter 4). Nearly every JS program ever written will need to handle value coercion in some shape or form, so it's important you do so responsibly and with confidence.

If you have the `number` value `42`, but you want to treat it like a `string`, such as pulling out the `"2"` as a character in position `1`, you obviously must first convert (coerce) the value from `number` to `string`.

That seems simple enough.

But there are many different ways that such coercion can happen. Some of these ways are explicit, easy to reason about, and reliable. But if you're not careful, coercion can happen in very strange and surprising ways.

Coercion confusion is perhaps one of the most profound frustrations for JavaScript developers. It has often been criticized as being so *dangerous* as to be considered a flaw in the design of the language, to be shunned and avoided.

Armed with a full understanding of JavaScript types, we're aiming to illustrate why coercion's *bad reputation* is largely overhyped and somewhat undeserved -- to flip your perspective, to seeing coercion's power and usefulness. But first, we have to get a much better grip on values and types.

## Kiểu dữ liệu dựng sẵn

JavaScript định nghĩa bảy kiểu dữ liệu dựng sẵn:

* `null`
* `undefined`
* `boolean`
* `number`
* `string`
* `object`
* `symbol` -- added in ES6!

**Note:** Ngoại trừ `object`, tất cả các kiểu dữ liệu này được gọi là loại cơ bản (primitives).

Toán tử `typeof` kiểm tra kiểu dữ liệu của giá trị đã cho, và luôn luôn trả về một trong bảy chuỗi, thật ngạc nhiên, nó không trùng khớp 1-1 với bảy kiểu dữ liệu dựng sẵn mà chúng ta đã đề cập ở trên.

```js
typeof undefined     === "undefined"; // true
typeof true          === "boolean";   // true
typeof 42            === "number";    // true
typeof "42"          === "string";    // true
typeof { life: 42 }  === "object";    // true

// added in ES6!
typeof Symbol()      === "symbol";    // true
```

Sáu loại này có kiểu dữ liệu tương ứng và trả về chuỗi trùng với tên như trên. `Symbol` là kiểu dữ liệu mới trong ES6 và sẽ được đề cập đến trong chương 3.

Như bạn đã thấy, tôi đã loại `null` khỏi danh sách trên. Nó *đặc biệt* -- đặc biệt ở chỗ nó sẽ có lỗi khi chúng ra dùng nó với toán tử `typeof`:

```js
typeof null === "object"; // true
```

Nó sẽ rất tốt (và chính xác) nếu như nó trả về `"null"`, nhưng lỗi này đã tồn tại trong gần hai thập kỷ, và có vẻ sẽ không bao giờ được sửa chữa bời vì có quá nhiều nội dung web tồn tại dựa trên lỗi nàynày nên việc sữa chữa lỗi này sẽ tạo thêm nhiều lỗi khác và làm hỏng nhiều phần mềm web.

Nếu bạn muốn kiểm tra một giá trị `null` bằng cách sử dụng kiểu dữ liệu của nó, bạn cần một điều kiện tổng hợp:

```js
var a = null;

(!a && typeof a === "object"); // true
```

`null` là giá trị giá trị "primitive" duy nhất là "falsy" (còn có tên là "false-like"; xem chương 4) nhưng trả về `"object"` từ toán tử `typeof`.

Vậy chuỗi thứ bảy mà `typeof` có thể trả về là gì?

```js
typeof function a(){ /* .. */ } === "function"; // true
```

Thật dễ dàng để nghĩ rằng `function` là kiểu dữ liệu dựng sẵn trong JS, đặc biệt khi nó được trả về khi dùng toán tử `typeof`. Tuy nhiên, nếu như bạn đọc đặc tả, bạn sẽ thấy rằng nó thực tế là một "subtype" (kiểu dữ liệu phụ) của `object`. Đặc biệt, hàm còn được gọi là "callable object" (một đối tượng có thể gọi được) -- một đối tượng có thuộc tính `[[Call]]` cho phép nó được thực thi.

Sự thật thì hàm là một đối tượng rất hữu ích. Quan trọng nhất, nó có thể chứa thuộc tính. Ví dụ:

```js
function a(b,c) {
	/* .. */
}
```

Hàm có thuộc tính `length` thể hiện số lượng tham số chính thức được khai báo.

```js
a.length; // 2
```

Vì bạn đã khai báo hàm với hai tham số có tên chính thức là(`b` và `c`), nên thuộc tính "length" của hàm là `2`.

Mảng thì sao? Nó là dữ liệu cơ sở của JS, vậy nó có phải là một kiểu dữ liệu đặc biệt?

```js
typeof [1,2,3] === "object"; // true
```

Không, nó chỉ là đối tượng. Sẽ thích hợp với suy nghĩ rằng nó cũng là một "subtype" (kiểu dữ liệu phụ) của đối tượng (xem chương 3), trong trường hợp này các phần tử của nó được lập chỉ mục bằng số (trái ngược lại việc dùng khoá là chuỗi như đối tượng đơn giản) và luôn tồn tại thuộc tính `.length` được cập nhật tự động.

## Values as Types

Trong JavaScript, biến không có kiểu dữ liệu -- **Giá trị mới có kiểu dữ liệu**. Biến có thể giữ bất kỳ giá trị nào, ở bất kỳ thời điểm nào.

Một cách nghĩ khác về kiểu dữ liệu trong JS là JS không có "type enforcement" (kiểu dữ liệu bắt buộc) nghĩa là engine không bắt buộc biến luôn luôn giữ giá trị cùng kiểu dữ liệu với kiểu dữ liệu mà nó được khởi tạo. Biến trong câu lệnh gán có thể giữ giá trị có kiểu `string`, và sau đó có thể giữ giá trị có kiểu `number`, v.v...

*Giá trị* `42` có kiểu dữ liệu `number`, và *kiểu dữ liệu* của nó không thể bị thay đổi. Một giá trị khác, như `"42"` với kiểu dữ liệu `string`, có thể được tạo từ kiểu `number` của giá trị `42` thông qua cơ chế **coercion** (**ép kiểu**) (xem chương 4).

Khi bạn dùng `typeof` với một biến, nó không mang ý nghĩa "Kiểu dữ liệu của biến là gì?", Bời vì biến trong JS không có kiểu dữ liệu. Thay vào đó, nó mang ý nghĩa "Kiểu dữ liệu của giá trị mà biến đang giữ là gì?".

```js
var a = 42;
typeof a; // "number"

a = true;
typeof a; // "boolean"
```

Toán tử `typeof` luôn trả về một chuỗi. Nên:

```js
typeof typeof 42; // "string"
```

Câu lệnh `typeof 42` đầu tiên trả về`"number"`, và `typeof "number"` trả về `"string"`.

### `undefined` vs "undeclared"

Biến có thể được khai báo nhưng không mang bất kỳ giá trị nào, thì nó sẽ có giá trị `undefined`. Gọi `typeof` với biến đó thì giá trị trả về sẽ là `"undefined"`:

```js
var a;

typeof a; // "undefined"

var b = 42;
var c;

// later
b = c;

typeof b; // "undefined"
typeof c; // "undefined"
```

Có một điều hấp dẫn là hầu hết các lập trình viên khi nghĩ về từ "undefined" (không xác định) sẽ nghĩ nó đồng nghĩa với "undeclared" (không được khai báo). Tuy nhiên, trong JS, đây là hai kịch bản hoàn toàn khác nhau.

Một biến "undefined" là biến được khai báo trong phạm vi có thể truy cập, nhưng tại thời điểm được khai báo nó không chứa bất kỳ giá trị nào. Ngược lại, một biến "undeclared" không đươc khai báo trong phạm vi có thể truy cập.

Xét trường hợp sau:

```js
var a;

a; // undefined
b; // ReferenceError: b is not defined
```

Thông báo lỗi mà trình duyệt trả về cho điều kiện này là một nhầm lẫn khó chịu. Như bạn có thể thấy, thông báo "b is not defined" (b không được xác định) ,rất dễ gây nhầm lẫn với "b is undefined" (b không xác định). Xin nhắc lại, "undefined" (không xác định) và "is not defined" (không được xác định) là hai điều rất khác nhau. Sẽ tốt hơn và ít gây nhầm lẫn hơn nếu trình duyệt đưa ra thông báo như "b không được tìm thấy" hay "b không được khai báo".

Còn một điều đặc biệt liên quan đến toán tử `typeof` nữa vì nó liên quan đến biến không được khai báo thậm chí còn gây nhầm lẫn hơn. Xét trường hợp sau:

```js
var a;

typeof a; // "undefined"

typeof b; // "undefined"
```

Toán tử `typeof` trả về `"undefined"` cho cả biến "undeclared" (không được khai báo hoặc không được xác định). Điều đáng chú ý là không có bất kỳ thông báo lỗi nào được trả về khi chúng ta thực thi `typeof b`, dù rằng `b` là biến không được khai báo. Điều này sẽ đảm bảo an toàn khi sử dụng toán tử `typeof`.

Tương tự như trên, sẽ tốt hơn nếu khi dùng `typeof` với biến không được khai báo sẽ trả về "undeclared" thay vì kết quả trả về giống với trường hợp "undefined".

### `typeof` Undeclared

Mặc dù vậy, sự đảm bảo an toàn này là một tính năng hữu ích khi làm việc với JavaScript trên trình duyệt, nơi mà rất nhiều tệp tin có thể đưa các biến lên phạm vi toàn cục.

**Chi chú:** Rất nhiều lập trình viên tin rằng sẽ không nên có bất kỳ biến toàn cục nào, và mọi thứ nên được đóng gói trong những module và phạm vi riêng biệt. Theo lý thuyết thì điều này rất tuyệt vời nhưng lại bất khả thi trong thực tế; nhưng nó vẫn là một mục tiêu tốt để phấn đấu! May mắn thay, ES6 đã thêm first-class để hỗ trợ cho module, nó sẽ giúp điều đó trở nên thực tế hơn nhiều.

Ví dụ, tưởng tượng rằng có một "chế độ gỡ lỗi" trong chương trình của bạn mà nó được điều khiển bằng một biết toàn cục (cờ) với tên là `DEBUG`. Bạn sẽ muốn kiểm tra biến đó đã được khai báo hay chưa trước thực hiện những thao tác gỡ lỗi như xuất ra một thông báo. Một khai báo toàn cục`var DEBUG = true` sẽ chỉ được lưu trong tệp tin "debug.js", tệp tin mà bạn chỉ tải lên trình duyệt khi bạn đang trong môi trường phát triển học kiểm thử phần mềm chứ không phải với một sản phẩm hoàn thiện.

Tuy nhiên, bạn phải quan tâm tới cách kiểm tra biến toàn cục `DEBUG` trong phần còn lại của mã mà bạn lập trình, do đó bạn không thể nảo trả về một `ReferenceError`. Chế độ bảo vệ của `typeof` sẽ là phương án tuyệt vời trong trường hợp này.

```js
// oops, this would throw an error!
if (DEBUG) {
	console.log( "Debugging is starting" );
}

// this is a safe existence check
if (typeof DEBUG !== "undefined") {
	console.log( "Debugging is starting" );
}
```

Loại kiểm tra này vẫn hữu ích kể cả khi bạn không xử lý các biến do người dùng định nghĩa (như `DEBUG`). Nếu bạn đang thực hiện một tính năng kiểm tra cho một api dựng sẵn, bạn cũng có thể thấy nó hữu ích với việc không trả ra lỗi:

```js
if (typeof atob === "undefined") {
	atob = function() { /*..*/ };
}
```

**Note:** Nếu bạn đang định nghĩa một "polyfill" cho một tính năng mà không biết nó đã tồn tại hay chưa, có lẽ bạn nên tránh dùng `var` thể tạo khai báo `atob`. nếu bạn khai báo `var atob` trong câu lệnh `if`, khai báo này sẽ bị kéo lên (xem cuốn *Scope & Closures* của đợt phát hành này) trên cùng của phạm vi, kể cả khi điều kiện `if` không được thông qua ( bởi vì biến toàn cục `atob` đã tồn tại trước đó!). Trong một vài trình duyệt và một vài kiểu dữ liệu đặc biệt của biến toàn cục dựng sẵn (thường được gọi là những đối tượng chủ (host objects)), những khai báo trùng lắp này có thể trả ra lỗi. Việc bỏ qua `var` ngăn chặn khai báo này.

Another way of doing these checks against global variables but without the safety guard feature of `typeof` is to observe that all global variables are also properties of the global object, which in the browser is basically the `window` object. So, the above checks could have been done (quite safely) as:

```js
if (window.DEBUG) {
	// ..
}

if (!window.atob) {
	// ..
}
```

Unlike referencing undeclared variables, there is no `ReferenceError` thrown if you try to access an object property (even on the global `window` object) that doesn't exist.

On the other hand, manually referencing the global variable with a `window` reference is something some developers prefer to avoid, especially if your code needs to run in multiple JS environments (not just browsers, but server-side node.js, for instance), where the global object may not always be called `window`.

Technically, this safety guard on `typeof` is useful even if you're not using global variables, though these circumstances are less common, and some developers may find this design approach less desirable. Imagine a utility function that you want others to copy-and-paste into their programs or modules, in which you want to check to see if the including program has defined a certain variable (so that you can use it) or not:

```js
function doSomethingCool() {
	var helper =
		(typeof FeatureXYZ !== "undefined") ?
		FeatureXYZ :
		function() { /*.. default feature ..*/ };

	var val = helper();
	// ..
}
```

`doSomethingCool()` tests for a variable called `FeatureXYZ`, and if found, uses it, but if not, uses its own. Now, if someone includes this utility in their module/program, it safely checks if they've defined `FeatureXYZ` or not:

```js
// an IIFE (see "Immediately Invoked Function Expressions"
// discussion in the *Scope & Closures* title of this series)
(function(){
	function FeatureXYZ() { /*.. my XYZ feature ..*/ }

	// include `doSomethingCool(..)`
	function doSomethingCool() {
		var helper =
			(typeof FeatureXYZ !== "undefined") ?
			FeatureXYZ :
			function() { /*.. default feature ..*/ };

		var val = helper();
		// ..
	}

	doSomethingCool();
})();
```

Here, `FeatureXYZ` is not at all a global variable, but we're still using the safety guard of `typeof` to make it safe to check for. And importantly, here there is *no* object we can use (like we did for global variables with `window.___`) to make the check, so `typeof` is quite helpful.

Other developers would prefer a design pattern called "dependency injection," where instead of `doSomethingCool()` inspecting implicitly for `FeatureXYZ` to be defined outside/around it, it would need to have the dependency explicitly passed in, like:

```js
function doSomethingCool(FeatureXYZ) {
	var helper = FeatureXYZ ||
		function() { /*.. default feature ..*/ };

	var val = helper();
	// ..
}
```

There are lots of options when designing such functionality. No one pattern here is "correct" or "wrong" -- there are various tradeoffs to each approach. But overall, it's nice that the `typeof` undeclared safety guard gives us more options.

## Review

JavaScript có bảy kiểu dữ liệu dựng sẵn: `null`, `undefined`,  `boolean`, `number`, `string`, `object`, `symbol`. Nó được nhận diện bời toán tử `typeof`.

Biến không có kiểu dữ liệu, nhưng giá trị bên trong chúng có. Những kiểu dữ liệu này xác định những hành vi có sẵn của những giá trị đó.

Nhiều lập trình viên sẽ giả định "undefined" và "undeclared" tương đối giống nhau, nhưng trong JavaScript, chúng rất khác nhau. `undefined` là giá trị mà một biến được khai báo có thể giữ. "Undeclared" nghĩa là biến đó chưa được khai báo.

Thật không may JavaScript lại kết hợp hai thuật ngữ này lại làm một, không chỉ trong việc thông báo lỗi "ReferenceError: a is not defined") mà còn trong việc trả về giá trị khi dùng `typeof`, chúng đều là `"undefined"` cho cả hai trường hợp.

Tuy nhiên, chức năng bảo vệ (ngăn ngừa lỗi) của `typeof` khi nó được dùng với biến không được khai báo có thể hữu ích trong một số trường hợp.
