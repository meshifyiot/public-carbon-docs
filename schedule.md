# Schedule Formatting

Schedule strings are used in user subscriptions. They are used to define time ranges where notifications are allowed. A schedule is a set of days and time ranges. Schedule strings can contain multiple schedules and multiple time ranges in each schedule.

Example:
```
06|12:00 - 14:00, 15:30 - 16:00; 1|8:00 - 17:00;
```

Each schedule is delimited by `;` so we have two schedules in this string.

```
06|12:00 - 14:00, 15:30 - 16:00
1|8:00 - 17:00
```

Each schedule has a days section and a time range section delimited by `|`. This section will contain numbers `0` through `6` representing each day starting with Sunday respectivily. In the first schedule of the example the days section is defined as:

```
06
```

This indicates the time ranges to follow are effective on Sunday (`0`) and Saterday (`6`).


The time range section describes the time ranges for the schedule. The times must be in `24 hour` time, and there can be multiple time ranges delimited  In the first schedule of the example the time range section is defined as:

```
12:00 - 14:00, 15:30 - 16:00
```

This indicates that times are allowed from noon to 2 PM and 3:30 PM to 4 PM.

In summary the example schedule indicates Sunday and Saterday from noon to 2 PM, and 3:30 PM to 4:00 PM, and Monday from 8:00 AM to 5:00 PM.
