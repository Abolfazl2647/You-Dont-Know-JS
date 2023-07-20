# You Don't Know JS Yet: Get Started - 2nd Edition

# ضمیمه A: بررسی عمیق‌تر

دراین ضمیمه، میخواهیم چند عنوان از متن اصلی را اکتشاف کنیم. به محتویات این کتاب به عنوان چشم‌انداز اختیاری از جزییات ظریفی که میخواهیم در باقی این کتاب‌ها بررسی کنیم فکر کنید.

## مقادیر در مقابل رفرنس‌ها

در فصل ۲، دو مدل از أنواع اصلی مقدارها را بررسی کردیم: پریمی‌تیو و آبجکت‌ها.
ولی نکته‌ای در مورد این رو را تا کنون بررسی نکرده ایم: نحوه تخصیص و انتقال این مقدارها بین متغییر‌ها چگونه است.

در بسیاری از زبان‌ها، برنامه نویسان در تخصیص/انتقال یک متغییر به عنوان مقدار یا به عنوان رفرنس حق انتخاب دارند. گرچه در JS ، این تصمیم کاملا به نوع متغییر بستگی دارد. و این، برنامه‌نویسان سایر زبان‌های برنامه نویسی را هنگام شروع به کار با JS شگفت‌زده می‌کند.

اگر مقدار را تخصیص/انتقال دهید مقدار کپی هست. برای مثال:

```js
var myName = "Kyle";

var yourName = myName;
```

در اینجا متغیر `yourName` یک کپی جدا از متن `"Kyle"` که درون `myName` ذخیره شده است می‌باشد. زیرا این مقدار یک primitive است و مقدارهای primitive همیشه به صورت **کپی** تخصیص/انتقال داده می‌شوند.

در اینجا میتوان ثابت کرد که دو مقدار جدا نقش دارند.

```js
var myName = "Kyle";

var yourName = myName;

myName = "Frank";

console.log(myName);
// Frank

console.log(yourName);
// Kyle
```

ببیند که چگونه `yourName` با تغییر مقدار `myName` به `"Frank"` تغییر نکرد. چرا که هر متغیر کپی خودش را از مقدار دارد.

در مقابل، رفرنس‌ها به این معنی ‌هستند که دو متغییر در حال اشاره به یک مقدار می‌باشند. اینگار که تاثیر تغییر این مقدار مشترک منوت بر دسترسی آن متغییر ها است. در JS تنها با مقدار شمایل object (آرایه‌ها، objectها، توابع و غیره.) به عنوان رفرنس برخورد می‌شود.

کد زیر را در نظر بگیرید:

```js
var myAddress = {
    street: "123 JS Blvd",
    city: "Austin",
    state: "TX",
};

var yourAddress = myAddress;

// I've got to move to a new house!
myAddress.street = "456 TS Ave";

console.log(yourAddress.street);
// 456 TS Ave
```

از آنجایی که مقدار تخصیص داده شده به `myAddress` یک آبجکت است در واقع رفرنس آن نگهداری/تخصیص داده شده است، بدین ترتیب مقدار تخصیص داده شده به متغیر`yourAddress` یک کپی از رفنرس آن آنجکت است نه مقدار آن. به همین دلیل است که مقدار تخصیص داده شده به ` yourAddress.street` هنگامی که ` myAddress.street` دستخوش تغییر می‌شود، تغییر میکند. به عبارتی `myAddress` و `yourAddress` کپی‌های یک رفنرنس از آبجکت مشترک دارند، در نتیجه آپدیت یکی منجر به آپدیت هردوی آنها می‌شود.

دوباره، JS تصمیم می‌گیرد که بسته به نوع مقدار از مقدار کپی بگیرد یا از رفرنس. پریی‌میتیو‌ها با مقدار نگه‌داری می‌شوند ولی آبجکت‌ها با رفرنس. هیچ راهی وجود ندارد تا در JS این قاعده را تغییر داد.

## فرم‌های بسیار مختلف یک تابع

این قطعه کد را از فصل ۲ "توابع" آورده‌ایم:

```js
var awesomeFunction = function (coolThings) {
    // ..
    return amazingStuff;
};
```

The function expression here is referred to as an _anonymous function expression_, since it has no name identifier between the `function` keyword and the `(..)` parameter list. This point confuses many JS developers because as of ES6, JS performs a "name inference" on an anonymous function:

```js
awesomeFunction.name;
// "awesomeFunction"
```

ویژگی `name` یک تابع، یا اسم مستقیمی که به آن داده شده است را نمایان می‌کند (در صورت تعریف عادی/مستقیم) یا در صورتی که به شکل ناشناس تعریف شده باشد نام استنتاجی (ضمنی) آن را نمایان میکند.آن مقدار عموما توسط برنامه نویسان زمانی که یک تابع و یا خطایی در را پشته ردیابی بازرسی می‌کنند استفاده می‌شود.

بنابراین شاید حتی یک تابع ناشناس هم نامی بگیرد. گرچه این نام استنتاجی (ضمنی) در موارد محدودی تخصیص پیدا می‌کند مثل زمانی که تابع ضمنی توسط `=` اختصاص داده شود. اگر یک تابع ضمنی را به عنوان یک آرگومان به تابعی که فراخوانی می‌شود بدهید، اینگونه که هیچ نامی حتی به صورت استنتاجی هم تاثیر نداشته باشد; ویژگی `name` یک متن(استرینگ) خالی خواهد بود، و console معمولا "(anonymous function)" گزارش میکند.

حتی اگر نام به صورت استنتاجی باشد ** بازم هم یک تابع ناشناس خواهد بود**. چرا؟ چون نام استنتاجی آن یک متن metadata خواهد بود. نه یک identifier فعال که به تابع ارجاع می‌شود.و تابع ناشناس نامی ندارد که با آن بتواند خودش را درون خویش فراخوانی کند.- برای کارهای بازگشتی و رویداد های unbinding و غیره.

توابع بی‌نامرا با کد زیر مقایسه کنید:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function someName(coolThings) {
    // ..
    return amazingStuff;
};

awesomeFunction.name;
// "someName"
```

این تابع ضمنی را _تابع ضمنی نام‌دار_ میخوانند،چراکه `someName` در زمان کامپایل به طور مستقیم با تابع ضمنی در ارتباط است. مشارکت نام `awesomeFunction` تا زمان جرای آن خط از کد اتفاق نخواهد افتاد. این دو نام مجبور نیستند که یکی باشند. گاهی أوقات منظقی است که دو نام مختلف داشته باشند و برخی أوقات بهتر است تا نام یکسان داشته باشند.

توجه کنید که برای تابع نام دهی مستقیم `someName` در هنگام تخصیص _نام_ به ویژگی `name` تقدم دارد.

آیا توابع ضمنی باید نام‌دار یا بی نام باشند؟ نظرات در این مسله متفاوت است. بیشتر برنامه نویسان اهمیتی نمی‌دهند که از توابع بی‌نام استفاده کنند. آنها کوتاه‌تر، و در حوزه گسترده‌ JS رایج تر هستند.

به نظر من اگر تابعی در برنامه شما وجود دارد ، هدفی دارد که اگر نداشت حذفش می‌کردید. و اگر هدفی داشته باشد طبیعتا یک نام برای توصیف آن هدف باید داشته باشد.

آیا توابع ضمنی باید نام‌دار یا بی نام باشند؟ نظرات در این مسله متفاوت است. بیشتر برنامه نویسان اهمیتی نمی‌دهند که از توابع بی‌نام استفاده کنند. آنها کوتاه‌تر، و در حوزه گسترده‌ JS رایج تر هستند.
به نظر من اگر تابعی در برنامه شما وجود دارد ، هدفی دارد که اگر نداشت حذفش می‌کردید. و اگر هدفی داشته باشد طبیعتا یک نام برای توصیف آن هدف باید داشته باشد.
اگر تابعی نامی دارد، شما به عنوان کسی که کد را نوشته باید آن نام را در کد وارد کنید که برای فهمیدن نام آن تابع خواننده مجبور نباشد تابع را به‌طور ذهنی ایجاد کند. حتی تابع ناچیز `x * 2` بحتما باید خوانده شود تا نام "double" یا "multBy2"از آن استنتاج شود. این مقدار کوچک از کار ذهنی را هم میتوان با صرف یک ثانیه زمان و دادن نام"double" یا "multBy2"به تابع برای همیشه حذف کرد. تا کاربر را از شر فکر کردن برای هر بار خواندن کد در آینده نجات داد.

متاسفانه از اوایل سال ۲۰۲۰ فرم‌های مختلفی از تعریف تابع وجود دارد.( شاید در آینده بیشتر هم بشود!)

اینجا هم چند نمونه از أنواع تعریف تابع:

```js
// generator function declaration
function *two() { .. }

// async function declaration
async function three() { .. }

// async generator function declaration
async function *four() { .. }

// named function export declaration (ES6 modules)
export function five() { .. }
```

و این هم دسته‌ای از (تعداد زیاد) از أنواع توابع ضمنی:

```js
// IIFE
(function(){ .. })();
(function namedIIFE(){ .. })();

// asynchronous IIFE
(async function(){ .. })();
(async function namedAIIFE(){ .. })();

// arrow function expressions
var f;
f = () => 42;
f = x => x * 2;
f = (x) => x * 2;
f = (x,y) => x * y;
f = x => ({ x: x * 2 });
f = x => { return x * 2; };
f = async x => {
    var y = await doSomethingAsync(x);
    return y * 2;
};
someOperation( x => x * 2 );
// ..
```

به خاطر داشته باشید که تابع کمانی شکل ** به صورت نوشتاری بی‌نام ** هستند، به این معنی که نوشتار آنها اینگونه است که نمی‌توان به آنها نامی اختصاص داد. شاید توابع ضمنی نامی استنتاجی دریافت کنند، ولی زمانی که در یکی از حالت‌های اختصاص دهی باشند، نه در حالتی که (خیلی هم مرسوم است) که به عنوان یک آرگومان در فراخوانی تابعی قرار گرفتند.

از آنجایی که فکر نمی‌کنم استفاده زیاد از توابع بی نام در برنامه ایده خوبی باشد، پس علاقه چندانی به استفاده از فرم توابع کمانی `=>` ندارم.
این نوع تابع در حقیقت فقط یک هدف دارد (که کلمه کلیدی `this` را lexically بکار برد)، و این به این معنی نیست که ما باید از آن در هر تابعی که می‌نویسیم استفاده کنیم. برای هر کاری از ابزار مناسب آن استفاده کنید.

همچنین توابع را میتوان در کلاس‌ها و تعاریف آبجکت‌ها هم تایین کرد. وقتی در این فرم قرار می‌گیرند به عنوان "methods" از آنها یاد می‌شود.
اگر چه در JS این عبارت تفاوت چندان قابل مشاهده‌ای روی تابع ندارد:

```js
class SomethingKindaGreat {
    // class methods
    coolMethod() { .. }   // no commas!
    boringMethod() { .. }
}

var EntirelyDifferent = {
    // object methods
    coolMethod() { .. },   // commas!
    boringMethod() { .. },

    // (anonymous) function expression property
    oldSchool: function() { .. }
};
```

ای بابا! چقدر راه مختلف برای تعریف تابع وجود داره.

هیچ راه میانبری وجود ندارد; شما مجبور هستید با فرم های مختلف تعریف تابع آشنا شویدتا بتوانید آنها را تشخیص دهید و در جای مناسبه کد از آنها استفاده کنید. آنها را مطالعه و تمرین کنید.

## Coercive Conditional Comparison

Yes, that section name is quite a mouthful. But what are we talking about? We're talking about conditional expressions needing to perform coercion-oriented comparisons to make their decisions.

`if` and `? :`-ternary statements, as well as the test clauses in `while` and `for` loops, all perform an implicit value comparison. But what sort? Is it "strict" or "coercive"? Both, actually.

Consider:

```js
var x = 1;

if (x) {
    // will run!
}

while (x) {
    // will run, once!
    x = false;
}
```

You might think of these `(x)` conditional expressions like this:

```js
var x = 1;

if (x == true) {
    // will run!
}

while (x == true) {
    // will run, once!
    x = false;
}
```

In this specific case -- the value of `x` being `1` -- that mental model works, but it's not accurate more broadly. Consider:

```js
var x = "hello";

if (x) {
    // will run!
}

if (x == true) {
    // won't run :(
}
```

Oops. So what is the `if` statement actually doing? This is the more accurate mental model:

```js
var x = "hello";

if (Boolean(x) == true) {
    // will run
}

// which is the same as:

if (Boolean(x) === true) {
    // will run
}
```

Since the `Boolean(..)` function always returns a value of type boolean, the `==` vs `===` in this snippet is irrelevant; they'll both do the same thing. But the important part is to see that before the comparison, a coercion occurs, from whatever type `x` currently is, to boolean.

You just can't get away from coercions in JS comparisons. Buckle down and learn them.

## Prototypal "Classes"

In Chapter 3, we introduced prototypes and showed how we can link objects through a prototype chain.

Another way of wiring up such prototype linkages served as the (honestly, ugly) predecessor to the elegance of the ES6 `class` system (see Chapter 2, "Classes"), and is referred to as prototypal classes.

| TIP:                                                                                                                                       |
| :----------------------------------------------------------------------------------------------------------------------------------------- |
| While this style of code is quite uncommon in JS these days, it's still perplexingly rather common to be asked about it in job interviews! |

Let's first recall the `Object.create(..)` style of coding:

```js
var Classroom = {
    welcome() {
        console.log("Welcome, students!");
    },
};

var mathClass = Object.create(Classroom);

mathClass.welcome();
// Welcome, students!
```

Here, a `mathClass` object is linked via its prototype to a `Classroom` object. Through this linkage, the function call `mathClass.welcome()` is delegated to the method defined on `Classroom`.

The prototypal class pattern would have labeled this delegation behavior "inheritance," and alternatively have defined it (with the same behavior) as:

```js
function Classroom() {
    // ..
}

Classroom.prototype.welcome = function hello() {
    console.log("Welcome, students!");
};

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

All functions by default reference an empty object at a property named `prototype`. Despite the confusing naming, this is **not** the function's _prototype_ (where the function is prototype linked to), but rather the prototype object to _link to_ when other objects are created by calling the function with `new`.

We add a `welcome` property on that empty object (called `Classroom.prototype`), pointing at the `hello()` function.

Then `new Classroom()` creates a new object (assigned to `mathClass`), and prototype links it to the existing `Classroom.prototype` object.

Though `mathClass` does not have a `welcome()` property/function, it successfully delegates to the function `Classroom.prototype.welcome()`.

This "prototypal class" pattern is now strongly discouraged, in favor of using ES6's `class` mechanism:

```js
class Classroom {
    constructor() {
        // ..
    }

    welcome() {
        console.log("Welcome, students!");
    }
}

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Under the covers, the same prototype linkage is wired up, but this `class` syntax fits the class-oriented design pattern much more cleanly than "prototypal classes".
