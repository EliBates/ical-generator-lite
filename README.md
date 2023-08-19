`ical-generator-lite` is a fork of the ical-generator library specifically made for working on platforms like cloudflare workers. This package removes things like 'fs' and other unneeded things for edge-runtimes.

<br />
<br />

## 📦 Installation

    npm install ical-generator-lite

    # For TypeScript Users
    # (see "I use Typescript and get TS2307: Cannot find module errors" section below)
    npm i -D @types/node rrule moment-timezone moment dayjs @types/luxon

## ⚡️ Quick Start

```javascript
import ical from "ical-generator-lite";
import http from "node:http";

const calendar = ical({ name: "my first iCal" });
const startTime = new Date();
const endTime = new Date();
endTime.setHours(startTime.getHours() + 1);
calendar.createEvent({
    start: startTime,
    end: endTime,
    summary: "Example Event",
    description: "It works ;)",
    location: "my room",
    url: "http://sebbo.net/",
});

http.createServer((req, res) => calendar.serve(res)).listen(
    3000,
    "127.0.0.1",
    () => {
        console.log("Server running at http://127.0.0.1:3000/");
    }
);
```

See the [examples](./examples) folder for more examples.

## 📑 API-Reference

-   [Index](https://sebbo2002.github.io/ical-generator-lite/develop/reference/)
    -   [ICalCalendar](https://sebbo2002.github.io/ical-generator-lite/develop/reference/classes/ICalCalendar.html)
    -   [ICalEvent](https://sebbo2002.github.io/ical-generator-lite/develop/reference/classes/ICalEvent.html)
        -   [ICalAlarm](https://sebbo2002.github.io/ical-generator-lite/develop/reference/classes/ICalAlarm.html)
        -   [ICalAttendee](https://sebbo2002.github.io/ical-generator-lite/develop/reference/classes/ICalAttendee.html)
        -   [ICalCategory](https://sebbo2002.github.io/ical-generator-lite/develop/reference/classes/ICalCategory.html)

## 🕒 Date, Time & Timezones

ical-generator-lite supports [native Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date),
[Day.js](https://day.js.org/en/), [Luxon's](https://moment.github.io/luxon/) [DateTime](https://moment.github.io/luxon/docs/class/src/datetime.js~DateTime.html)
and the older [moment.js](https://momentjs.com/) and [moment-timezone](https://momentjs.com/timezone/)
objects. You can also pass a string which is then passed to javascript's `Date` internally.

It is recommended to use UTC time as far as possible. `ical-generator-lite` will output all time information as UTC time as
long as no time zone is defined. For day.js, a plugin is necessary for this, which is a prerequisite. If a time zone is
set, `ical-generator-lite` assumes that the given time matches the time zone. If a time zone is used, it is also recommended
to use a VTimezone generator. Such a function generates a VTimezone entry and returns it. For example, ical-timezones can
be used for this:

```typescript
import ical from "ical-generator-lite";
import { getVtimezoneComponent } from "@touch4it/ical-timezones";

const cal = new ICalCalendar();
cal.timezone({
    name: "FOO",
    generator: getVtimezoneComponent,
});
cal.createEvent({
    start: new Date(),
    timezone: "Europe/London",
});
```

If a `moment-timezone` object or Luxon's `setZone` method works, `ical-generator-lite` sets it according to the time zone set
in the calendar/event.

## 🚦 Tests

```
npm test
npm run coverage
npm run browser-test
```

## 🙆🏼‍♂️ Credits

Original author Sebastian Pekarek
