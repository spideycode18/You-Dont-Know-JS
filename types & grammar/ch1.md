# You Don't Know JS: Types & Grammar
# Chương 1: Kiểu dữ liệu

Phần lớn các lập trình viên đều nói rằng ngôn ngữ động (dynamic language) (như JS) không có kiểu dữ liệu. Cùng xem đặc tả ES5.1 (http://www.ecma-international.org/ecma-262/5.1/) nói gì về chủ đề này:

> Mỗi giá trị được thao tác bởi một thuật toán trong đặc tả này có một kiểu dữ liệu tương ứng. The possible value types are exactly those defined in this clause. Types are further sub classified into ECMAScript language types and specification types.
>
> Kiểu dữ liệu của ngôn ngữ ECMAScript tương ứng với giá trị mà chúng được thao tác trực tiếp bởi lập trình viên ECMAScript, người sử dụng ngôn ngữ ECMAScript. Các kiểu dữ liệu trong ngôn ngữ ECMAScript là Undefined, Null, Boolean, String, Number, và Object.

Bây giờ, nếu bạn là một người hâm mộ ngôn ngữ tĩnh (strongly typed/statically typed), bạn có thể phản đối cách sử dụng từ "kiểu dữ liệu".Trong các ngôn ngữ đó, "kiểu dữ liệu" có nghĩa nhiều hơn rất nhiều so với trong JS.

Một số người nói rằng JS không nên yêu cầu phải có "kiểu dữ liệu" và thay vào đó họ nên được gọi là "thẻ" (tags) hoặc có thể là "kiểu dữ liệu con" (subtypes).

Không! Chúng ta sẽ sử dụng định nghĩa sơ bộ sau (nó có vẻ giống với cách định nghĩa trong mô tả, chỉ là cách dùng từ khác đi): một kiểu dữ liệu là một tập hợp các đặc điểm đặc thù, có sẵn, xác định hành vi riêng biệt của một giá trị cụ thể và phân biệt nó với các giá trị khác, cho cả công cụ và lập trình viên.

Nói cách khác, nếu cả công cụ và lập trình viên đều xem giá trị `42` (số) khác với giá trị `"42"` (chuỗi), thì hai giá trị đó có kiểu dữ liệu tương ứng khác nhau -- `number` và `string`. Khi bạn dùng `42`, bạn đang định làm điều gì đó với số, như toán học. Nhưng khi bạn dùng `"42"`, bạn đang dự định làm điều gì đó với chuỗi, như xuất dữ liệu ra một trang, v.v... **Hai giá trị này có kiểu dữ liệu khác nhau.**

Đó không phải là một định nghĩa hoàn hảo. Nhưng nó đủ tốt cho thảo luận này. Và nó phù hợp với cách mà JS đã mô tả chính nó.

# Ý nghĩa của kiểu dữ liệu...

Ngoài những bất đồng về định nghĩa, tại sao JavaScript có kiểu dữ liệu hay không lại quan trọng?

Có một sự hiểu biết đúng đắn về từng kiểu dữ liệu và hành vi đặc thù của nó là vô cùng cần thiết để hiểu đúng và chính xác cách chuyển đổi giá trị giữa các kiểu dữ liệu khác nhau (xem Ép kiểu (Coercion), Chương 4). Hầu như mỗi chương trình JS được viết ra sẽ cần xử lý việc ép kiểu cho giá trị trong một vài trường hợp, vì vậy điều quan trọng là bạn phải thực hiện một cách có trách nhiệm và tự tin.

Nếu bạn có một `number` với giá trị `42`, nhưng bạn muốn xử lý nó như một `string`, chẳng hạn lấy `"2"` ra như là một ký tự ở vị trí `1`, Bạn rõ ràng phải chuyển đổi (ép kiểu) giá trị từ `number` sang `string`.

Điều đó có vẻ đơn giản.

Nhưng có rất nhiều cách khác nhau để việc ép kiểu này xảy ra. Một vài cách rõ ràng, dễ dàng để giải thích và đáng tin cậy. Nhưng nếu bạn không cẩn thận, việc ép kiểu sẽ diễn ra một cách rất lạ lùng và đáng ngạc nhiên.

Sự nhầm lẫn của ép kiểu có lẽ là một trong những điều gây chán nản nhất của những lập trình viên JavaScript. Nó thường bị chỉ trích là nguy hiểm đến mức bị coi là một lỗ hổng trong thiết kế của ngôn ngữ, bị xa lánh và tránh né.

Được trang bị đầy đủ kiến thức về kiểu dữ liệu của JavaScript, chúng tôi hướng tới mục tiêu minh hoạ tại sao tiếng xấu của ép kiểu phần lớn được kiểm soát và phần nào không được quan tâm -- để lật lại quan điểm của bạn, để thấy được sức mạnh và sự hữu ích của ép kiểu. Nhưng trước tiên, chúng ta phải nắm rõ giá trị và kiểu dữ liệu.

## Kiểu dữ liệu dựng sẵn

JavaScript định nghĩa bảy kiểu dữ liệu dựng sẵn:

* `null`
* `undefined`
* `boolean`
* `number`
* `string`
* `object`
* `symbol` -- được thêm trong ES6!

**Note:** Ngoại trừ `object`, tất cả các kiểu dữ liệu này được gọi là loại nguyên thuỷ (primitives).

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

Sáu loại này có giá trị của kiểu dữ liệu tương ứng và trả về chuỗi trùng với tên như trên. `Symbol` là kiểu dữ liệu mới trong ES6 và sẽ được đề cập đến trong chương 3.

Như bạn đã thấy, tôi đã loại `null` khỏi danh sách trên. Nó *đặc biệt* -- đặc biệt ở chỗ nó sẽ có lỗi khi chúng ra dùng nó với toán tử `typeof`:

```js
typeof null === "object"; // true
```

Sẽ rất tốt (và chính xác) nếu như nó trả về `"null"`, nhưng lỗi này đã tồn tại trong gần hai thập kỷ, và có vẻ sẽ không bao giờ được sửa chữa bời vì có quá nhiều nội dung web tồn tại dựa trên lỗi này nên việc sữa chữa lỗi này sẽ tạo thêm nhiều lỗi khác và làm hỏng nhiều phần mềm web.

Nếu bạn muốn kiểm tra một giá trị `null` bằng cách sử dụng kiểu dữ liệu của nó, bạn cần một điều kiện tổng hợp:

```js
var a = null;

(!a && typeof a === "object"); // true
```

`null` là giá trị nguyên thuỷ (primitive) duy nhất là "sai" (falsy) (còn có tên là "false-like"; xem chương 4) nhưng trả về `"object"` từ toán tử `typeof`.

Vậy chuỗi thứ bảy mà `typeof` có thể trả về là gì?

```js
typeof function a(){ /* .. */ } === "function"; // true
```

Chúng ta ễ dàng để nghĩ rằng `function` là kiểu dữ liệu dựng sẵn trong JS, đặc biệt khi nó được trả về khi dùng toán tử `typeof`. Tuy nhiên, nếu như bạn đọc đặc tả, bạn sẽ thấy rằng nó thực tế là một kiểu dữ liệu con (subtype) của `object`. Đặc biệt, hàm còn được gọi là "đối tượng có thể gọi được" (callable object) -- một đối tượng có thuộc tính `[[Call]]` cho phép nó được thực thi.

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

Vì bạn đã khai báo hàm với hai tham số có tên chính thức là (`b` và `c`), nên thuộc tính `length` của hàm là `2`.

Mảng thì sao? Nó là dữ liệu cơ sở của JS, vậy nó có phải là một kiểu dữ liệu đặc biệt?

```js
typeof [1,2,3] === "object"; // true
```

Không, nó chỉ là đối tượng. Sẽ thích hợp với suy nghĩ rằng nó cũng là một "kiểu dữ liệu con" (subtype) của đối tượng (xem chương 3), trong trường hợp này các phần tử của nó được lập chỉ mục bằng số (trái ngược lại việc dùng khoá là chuỗi như đối tượng đơn giản) và luôn tồn tại thuộc tính `.length` được cập nhật tự động.

## Giá trị như kiểu dữ liệu

Trong JavaScript, biến không có kiểu dữ liệu -- **Giá trị mới có kiểu dữ liệu**. Biến có thể giữ bất kỳ giá trị nào, ở bất kỳ thời điểm nào.

Một cách nghĩ khác về kiểu dữ liệu trong JS là JS không có "kiểu dữ liệu bắt buộc" (type enforcement) nghĩa là công cụ không bắt buộc biến luôn luôn giữ giá trị cùng kiểu dữ liệu với kiểu dữ liệu mà nó được khởi tạo. Biến trong câu lệnh gán có thể giữ giá trị có kiểu `string`, và sau đó có thể giữ giá trị có kiểu `number`, v.v...

*Giá trị* `42` có kiểu dữ liệu `number`, và *kiểu dữ liệu* của nó không thể bị thay đổi. Một giá trị khác, như `"42"` với kiểu dữ liệu `string`, có thể được tạo từ kiểu `number` của giá trị `42` thông qua cơ chế **ép kiểu** (coercion) (xem chương 4).

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

Thông báo lỗi mà trình duyệt trả về cho điều kiện này là một nhầm lẫn khó chịu. Như bạn có thể thấy, thông báo "b is not defined" (b không được xác định) ,rất dễ gây nhầm lẫn với "b is undefined" (b không xác định). Xin nhắc lại, "undefined" (không xác định) và "is not defined" (không được xác định) là hai điều rất khác nhau. Sẽ tốt hơn và ít gây nhầm lẫn hơn nếu trình duyệt đưa ra thông báo như "b is not found" (b không được tìm thấy) hay "b is not declared" (b không được khai báo).

Còn một điều đặc biệt liên quan đến toán tử `typeof` nữa vì nó liên quan đến biến không được khai báo, thậm chí còn gây nhầm lẫn hơn. Xét trường hợp sau:

```js
var a;

typeof a; // "undefined"

typeof b; // "undefined"
```

Toán tử `typeof` trả về `"undefined"` cho cả biến "undeclared" (không được khai báo hoặc không được xác định). Điều đáng chú ý là không có bất kỳ thông báo lỗi nào được trả về khi chúng ta thực thi `typeof b`, dù rằng `b` là biến không được khai báo. Điều này sẽ đảm bảo an toàn khi sử dụng toán tử `typeof`.

Tương tự như trên, sẽ tốt hơn nếu khi dùng `typeof` với biến không được khai báo sẽ trả về "undeclared" thay vì kết quả trả về giống với trường hợp "undefined".

### `typeof` Undeclared

Mặc dù vậy, sự đảm bảo an toàn này là một tính năng hữu ích khi làm việc với JavaScript trên trình duyệt, nơi mà rất nhiều tệp tin có thể đưa các biến lên phạm vi toàn cục.

**Chi chú:** Rất nhiều lập trình viên tin rằng sẽ không nên có bất kỳ biến toàn cục nào, và mọi thứ nên được đóng gói trong những mô đun và phạm vi riêng biệt. Theo lý thuyết thì điều này rất tuyệt vời nhưng lại bất khả thi trong thực tế; nhưng nó vẫn là một mục tiêu tốt để phấn đấu! May mắn thay, ES6 đã thêm first-class để hỗ trợ cho mô đun, nó sẽ giúp điều đó trở nên thực tế hơn nhiều.

Ví dụ, tưởng tượng rằng có một "chế độ gỡ lỗi" trong chương trình của bạn mà nó được điều khiển bằng một biết toàn cục (cờ) với tên là `DEBUG`. Bạn sẽ muốn kiểm tra biến đó đã được khai báo hay chưa trước khi thực hiện những thao tác gỡ lỗi như xuất ra một thông báo. Một khai báo toàn cục`var DEBUG = true` sẽ chỉ được lưu trong tệp tin "debug.js", tệp tin mà bạn chỉ tải lên trình duyệt khi bạn đang trong môi trường phát triển hoặc kiểm thử phần mềm chứ không phải với một sản phẩm hoàn thiện.

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

Một cách khác để thực hiện các kiểm tra này đối với các biến toàn cục nhưng không có tính năng bảo vệ an toàn của typeof là xem rằng tất cả các biến toàn cục cũng là thuộc tính của đối tượng toàn cục, trong trình duyệt là đối tượng `window`. Vì vậy các kiểm tra trên được thực hiện (khá an toàn) như:

```js
if (window.DEBUG) {
	// ..
}

if (!window.atob) {
	// ..
}
```

Không giống việc tham chiếu đến các biến không được khai báo, lỗi `ReferenceError` sẽ không bị trả về nếu bạn truy cập đến một thuộc tính của đối tượng (kể cả dối tượng toàn cục `window`) mà nó không tồn tại.

Mặt khác, tham chiếu thủ công đến biến toàn cục với tham chiếu `window` là điều mà một số lập trình viên muốn tránh, đặc biệt là nếu mã của bạn cần chạy trong nhiều môi trường JS (ví dụ, không chỉ trình duyệt, mà là node.js phía máy chủ), ở đó đối tượng toàn cầu có thể không được gọi `window`.

Về mặt kỹ thuật, chức năng bảo vệ an toàn này của `typeof` hữu dụng kể cả khi bạn không dùng cho những biến toàn cục, mặc dù những trường hợp này ít phổ biến hơn, và một số lập trình viên ít mong muốn dùng cách tiếp cận này hơn. Thử tưởng tượng bạn có một hàm tiện ích mà bạn muốn người khác sap chéo và dán vào chương trình hoặc mô đun của họ, trong đó bạn muốn kiểm tra xem chương trình đã khai báo một biến nào đó (biến mà bạn định dùng) chưa:

```js
function doSomethingCool() {
	var helper =
		(typeof FeatureXYZ !== "undefined") ?
		FeatureXYZ :
		function() { /*.. tính năng mặc định ..*/ };

	var val = helper();
	// ..
}
```

`doSomethingCool()` kiểm tra biến `FeatureXYZ`, nếu nó đã được định nghĩa thì dùng nó, nếu chưa chúng ta sẽ dùng hàm mặc định mà chúng ta định nghĩa cho nó. Bây giờ nếu ai đó nhập tiện ích này vào chương trình/ mô đun của họ, sẽ an toàn nếu kiểm tra xem họ đã định nghĩa `FeatureXYZ` hay chưa:

```js
// IIFE (xem "Immediately Invoked Function Expressions"
// thảo luận ở cuốn *Scope & Closures* của phát hành này)
(function(){
	function FeatureXYZ() { /*.. tính năng XYZ của chúng ta ..*/ }

	// truyền `doSomethingCool(..)`
	function doSomethingCool() {
		var helper =
			(typeof FeatureXYZ !== "undefined") ?
			FeatureXYZ :
			function() { /*.. tính năng mặc định ..*/ };

		var val = helper();
		// ..
	}

	doSomethingCool();
})();
```

Ở đây, `FeatureXYZ` không phải là biến toàn cục, nhưng chúng ta vẫn có thể dùng chức năng bảo vệ an toàn của `typeof` để giúp nó an toàn cho việc kiểm tra. Quan trọng hơn, ở đây chúng ta không có đối tượng nào để sử dụng (giống như cách chúng ta thực hiện với biến toàn cục như `window.___`) cho việc thực hiện kiểm tra, nên `typeof` khá hữu dụng.

Nhiều lập trình viên khác sẽ thích mẫu thiết kế "Dependency Injection" hơn, ở đó thay vì `doSomethingCool()` inspecting implicitly for `FeatureXYZ` to be defined outside/around it, it would need to have the dependency explicitly passed in, like:

```js
function doSomethingCool(FeatureXYZ) {
	var helper = FeatureXYZ ||
		function() { /*.. default feature ..*/ };

	var val = helper();
	// ..
}
```

Có rất nhiều lựa chọn khi thiết kế một hàm như vậy. Không thiết kế nào ở đây là đúng hay sai -- có những sự đánh đổi khác nhau giữa các cách tiếp cận. Nhưng nhìn chung, chức năng bảo vệ an toàn khỏi việc không được khai báo của `typeof` cho chúng ta thêm nhiều lựa chọn hơn.

## Đánh giá

JavaScript có bảy kiểu dữ liệu dựng sẵn: `null`, `undefined`,  `boolean`, `number`, `string`, `object`, `symbol`. Nó được nhận diện bời toán tử `typeof`.

Biến không có kiểu dữ liệu, nhưng giá trị bên trong chúng có. Những kiểu dữ liệu này xác định những hành vi có sẵn của những giá trị đó.

Nhiều lập trình viên sẽ giả định "undefined" và "undeclared" tương đối giống nhau, nhưng trong JavaScript, chúng rất khác nhau. `undefined` là giá trị mà một biến được khai báo có thể giữ. "Undeclared" nghĩa là biến đó chưa được khai báo.

Thật không may JavaScript lại kết hợp hai thuật ngữ này lại làm một, không chỉ trong việc thông báo lỗi "ReferenceError: a is not defined") mà còn trong việc trả về giá trị khi dùng `typeof`, chúng đều là `"undefined"` cho cả hai trường hợp.

Tuy nhiên, chức năng bảo vệ (ngăn ngừa lỗi) của `typeof` khi nó được dùng với biến không được khai báo có thể hữu ích trong một số trường hợp.
