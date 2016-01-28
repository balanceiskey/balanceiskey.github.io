---
layout: post
title: You can use moment-timezone to mock timezones in Jasmine
---

I think. 

Imagine you've got a Jasmine spec that tests that a given moment-backed function returns a local hour and day for a given UTC hour and day. It looks something like this:

```javascript

// DateMath.js
import moment from 'moment';

function getDayHourForClient (utcDay, utcHour) {
    const m = moment.utc().day(utcDay).hours(utcHour).minutes(0).seconds(0);
    const localMoment = moment.unix(m.unix());

    return {
        day: localMoment.day(),
        hour: localMoment.hours(),
    };
};

export default {
    getDayHourForClient,
};

// Tests.js

describe ('A library that returns the local hour and day', () => {
    it ('should return the appropriate hour and day provided a UTC hour and day', () => {
        // Thursdays at 2 a.m. UTC
        const result = DateMath.getDayHourForClient(4, 2);

        // Wednesdays at 8 p.m. Chicago
        expect(result.day).toEqual(3);
        expect(result.hour).toEqual(20);
    });
});


```

If your computer lives in Chicago, Austin or Omaha, this test is grand. If it lives anywhere else you're sad. 

While Jasmine does provide [some utilities](http://jasmine.github.io/2.2/introduction.html#section-Jasmine_Clock) for dealing with time-dependent code it doesn't have an answer for how to run tests in a given timezone context. Enter `moment-timezone`. Consider the last test with some modifications:

```javascript

import moment from 'moment-timezone';

describe ('A library that returns the local hour and day', () => {
    it ('should return the appropriate hour and day provided a UTC hour and day', () => {
        moment.tz.setDefault('America/Chicago');

        // Thursdays at 2 a.m. UTC
        const result = DateMath.getDayHourForClient(4, 2);

        // Wednesdays at 8 p.m. Chicago
        expect(result.day).toEqual(3);
        expect(result.hour).toEqual(20);

        moment.tz.setDefault();
    });
});
```

Yay! Now we're all in Chicago! `moment-timezone` provides a way to set a default timezone, meaning that any datetime objects constructed with moment will adhere to the set timezone. 
