# You Don't Know JS Yet: Get Started - 2nd Edition

#ضمیمه B : تمرین تمرین تمرین

در این ضمیمه، چند تمرین بع علاوه جواب های پیشنهادی آنها را مرور میکنیم. این تمرین‌ها _شما را در مسیر یادگیری_ مفاهیمی که در این کتاب گفته شده قرار می‌دهد.

## تمرین مقایسه‌ها

بیاید روی به تایپ مقدار‌ها و شرط‌ها در جایی که تبدیل تایپ اتفاق می‌افتد تمرین کنیم.

`scheduleMeeting(..)` باید زمان شروع را با فرمت (۲۴ ساعته به عنوان استرینگ "hh:mm") و مدت زمان جلسه را به (دقیقه عددی) بگیرد. و اگر مدت زمان جلسه داخل بازه زمانی کاری باشد (بازه زمانی با توجه به `dayStart` و `dayEnd`) مقدار `true` و اگر خارج این بازه باشد مقدار `false` را بر می‌گرداند.

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime, durationMinutes) {
    // ..TODO..
}

scheduleMeeting("7:00", 15); // false
scheduleMeeting("07:15", 30); // false
scheduleMeeting("7:30", 30); // true
scheduleMeeting("11:30", 60); // true
scheduleMeeting("17:00", 45); // true
scheduleMeeting("17:30", 30); // false
scheduleMeeting("18:00", 15); // false
```

سعی کنید اول خودتان حل کنید. استفاده برابری و عملگرهای مقایسه‌ای مرتبط را در نظر بگیرید، و اینکه چگونه تبدیل تایپ روی این کد تاثیر خواهد گذاشت. هنگامی که به کدی که کار می‌کند دست‌یافتید راه حل خودتان را با راه حل قسمت "راه حل های پیش‌نهادی" در آخر این ضمیمه _مقایسه_ کنید.

## Practicing Closure

حالا بیاید با closure تمرین کنیم. (فصل ۴ ستون ۱)

تابع `range(..)` یک عدد به عنوان اولین آرگومان خود میگیرد که بیانگر اولین عدد در بازه مورد نظر است. و دومین آرگومان هم عددیست که انتهای بازه مورد نظر را مشخص می‌کند. اگر آرگومان دوم حضور نداشته باشد در این صورت باید تابع دیگری که توقع این آرگومان را دارد برگشت داده شود.

```js
function range(start, end) {
    // ..TODO..
}

range(3, 3); // [3]
range(3, 8); // [3,4,5,6,7,8]
range(3, 0); // []

var start3 = range(3);
var start4 = range(4);

start3(3); // [3]
start3(8); // [3,4,5,6,7,8]
start3(0); // []

start4(6); // [4,5,6]
```

ابتدا سعی کنید اول خودتان آن را حل کنید.
هنگامی که به کدی که کار می‌کند دست‌یافتید راه حل خودتان را با راه حل قسمت "راه حل های پیش‌نهادی" در آخر این ضمیمه _مقایسه_ کنید.

## تمرین Prototypeها

در نهایت بیایید روی `this` و آبجکتهایی که توسط prototype به هم متصل شده‌اند کار کنیم. (فصل ۴، ستون ۲)

یک گردونه شانس با ۳ حلقه که میتواند به صورت مجزا `spin()` (تابع چرخش) بچرخد و در نهایت `display()` (تابع نمایش) محتوای همه حلقه‌ها را نمایش دهد تعریف کنید. ( از اینایی که در کازینو‌ها هست و روی هر حلقه چند تا شکل داره و اگر همه اشکال در یه خط مثل هم بیاد جایزه میبری)
در زیر ابتدایی ترین رفتار یک حلقه درون آبجکت `reel` تعریف شده است . ولی ماشین شانسی به آبجکتهای(تصویرهای) مجزا روی حلقه نیاز دارد – آبجکت‌هایی که به `reel`, دلالت می‌کند و property `position` دارد.

یک حلقه _ فقط میداند_ که چگونه مقدار کنونی خود را نمایش (`display()`) دهد، ولی ماشین شانسی مقدار همه‌ی حلقه‌ها را میداند: تصویر کنونی (`position`)، تصویر بالایی (`position - 1`) ، و تصویر پایینی (`position + 1`) . بنابراین نمایش ماشین شانسی مانند نمایش یک ماتریس 3 x 3 از تصاویر است.

```js
function randMax(max) {
    return Math.trunc(1e9 * Math.random()) % max;
}

var reel = {
    symbols: ["♠", "♥", "♦", "♣", "☺", "★", "☾", "☀"],
    spin() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        this.position =
            (this.position + 100 + randMax(100)) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        return this.symbols[this.position];
    },
};

var slotMachine = {
    reels: [
        // this slot machine needs 3 separate reels
        // hint: Object.create(..)
    ],
    spin() {
        this.reels.forEach(function spinReel(reel) {
            reel.spin();
        });
    },
    display() {
        // TODO
    },
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

ابتدا سعی کنید اول خودتان آن را حل کنید.

کمک:

هنگام دسترسی نمادها به صورت دایره ای در اطراف یک قرقره، از عملگر مدول `%` برای پیچیدن «موقعیت» استفاده کنید.

از `Object.create(..)` برای ایجاد یک آبجکت استفاده کنید یک اتصال prototypeای به یک آبجکت دیگر بدهید. هنگامی که اتصال پیدا کرد، delegation به ابجکت‌ها اجازه میدهد تا عبارت `this` را هنگام فراحوانی به اشتراک بگذارند.

به جای آنکه آبجکت حلقه (reel) را مستقیم تغییر دهید تا هر سه موقعیت را نمایش دهید، می‌توانید از یک آبجکت موقت (`Object.create(..)` مجدد) که `position`, خودش را دارد استفاده کنید تا از آن تفویض بگیرید.

هنگامی که به کدی که کار می‌کند دست‌یافتید راه حل خودتان را با راه حل قسمت "راه حل های پیش‌نهادی" در آخر این ضمیمه _مقایسه_ کنید.

## راه حل های پیش‌نهادی

در ذهنتان باشد که راه حل های پیشنهادی صرفا پیشنهادی هستند. و برای حل این تمرین ها راه های مختلفی وجود دارد. راه حل خودتان را با این راه حل ها مقایسه کنید و نکات مثبت و منفی آن را در نظر بگیرید.

راه حل پیشنهادی برای تمرین "Comparisons" (ستون ۳)

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime, durationMinutes) {
    var [, meetingStartHour, meetingStartMinutes] =
        startTime.match(/^(\d{1,2}):(\d{2})$/) || [];

    durationMinutes = Number(durationMinutes);

    if (
        typeof meetingStartHour == "string" &&
        typeof meetingStartMinutes == "string"
    ) {
        let durationHours = Math.floor(durationMinutes / 60);
        durationMinutes = durationMinutes - durationHours * 60;
        let meetingEndHour = Number(meetingStartHour) + durationHours;
        let meetingEndMinutes = Number(meetingStartMinutes) + durationMinutes;

        if (meetingEndMinutes >= 60) {
            meetingEndHour = meetingEndHour + 1;
            meetingEndMinutes = meetingEndMinutes - 60;
        }

        // re-compose fully-qualified time strings
        // (to make comparison easier)
        let meetingStart = `${meetingStartHour.padStart(
            2,
            "0"
        )}:${meetingStartMinutes.padStart(2, "0")}`;
        let meetingEnd = `${String(meetingEndHour).padStart(2, "0")}:${String(
            meetingEndMinutes
        ).padStart(2, "0")}`;

        // NOTE: since expressions are all strings,
        // comparisons here are alphabetic, but it's
        // safe here since they're fully qualified
        // time strings (ie, "07:15" < "07:30")
        return meetingStart >= dayStart && meetingEnd <= dayEnd;
    }

    return false;
}

scheduleMeeting("7:00", 15); // false
scheduleMeeting("07:15", 30); // false
scheduleMeeting("7:30", 30); // true
scheduleMeeting("11:30", 60); // true
scheduleMeeting("17:00", 45); // true
scheduleMeeting("17:30", 30); // false
scheduleMeeting("18:00", 15); // false
```

---

Suggested solution for "Closure" (Pillar 1) practice:

```js
function range(start, end) {
    start = Number(start) || 0;

    if (end === undefined) {
        return function getEnd(end) {
            return getRange(start, end);
        };
    } else {
        end = Number(end) || 0;
        return getRange(start, end);
    }

    // **********************

    function getRange(start, end) {
        var ret = [];
        for (let i = start; i <= end; i++) {
            ret.push(i);
        }
        return ret;
    }
}

range(3, 3); // [3]
range(3, 8); // [3,4,5,6,7,8]
range(3, 0); // []

var start3 = range(3);
var start4 = range(4);

start3(3); // [3]
start3(8); // [3,4,5,6,7,8]
start3(0); // []

start4(6); // [4,5,6]
```

---

Suggested solution for "Prototypes" (Pillar 2) practice:

```js
function randMax(max) {
    return Math.trunc(1e9 * Math.random()) % max;
}

var reel = {
    symbols: ["♠", "♥", "♦", "♣", "☺", "★", "☾", "☀"],
    spin() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        this.position =
            (this.position + 100 + randMax(100)) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        return this.symbols[this.position];
    },
};

var slotMachine = {
    reels: [Object.create(reel), Object.create(reel), Object.create(reel)],
    spin() {
        this.reels.forEach(function spinReel(reel) {
            reel.spin();
        });
    },
    display() {
        var lines = [];

        // display all 3 lines on the slot machine
        for (let linePos = -1; linePos <= 1; linePos++) {
            let line = this.reels.map(function getSlot(reel) {
                var slot = Object.create(reel);
                slot.position =
                    (reel.symbols.length + reel.position + linePos) %
                    reel.symbols.length;
                return slot.display();
            });
            lines.push(line.join(" | "));
        }

        return lines.join("\n");
    },
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

این هم از این کتاب. ولی حالا موقع تمرین در پروژه‌های واقعی است تا این ایده‌ها را روی آن پیاده کنید. به کد زدن ادامه دهید چون بهترین راه برای یادگیری است.
