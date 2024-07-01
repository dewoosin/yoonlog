---
name: Yoon
gender: male
birthday: 1986-08-01
email: 10000007@qq.com
phone: 1000000007
tags:
  - family
  - closerfamily
---

## Profile

Name：`=(this.name)`
Age：`$= let date = moment(dv.current().birthday.toString(),"yyyy-MM-DD"); let today = moment().startOf('day'); today.diff(date,"years")` years old
E-mail：`=(this.email)`
Phone：`=(this.phone)`

## Habit Tracker

```dataview
TABLE WITHOUT ID
	link(file.name) as "Date",
	Reading AS "📖",
	Workout AS "🏃‍♂️",
	Writing AS "📝",
	Drink AS "🍺",
	Summary
	FROM "00 - DailyNotes/DailyNote" 
	SORT file.name DESC
	LIMIT 14
```

## Tasks This Week
```tasks
due in this week
```

```tasks
scheduled in this week
```
## Tasks Last Week
```tasks
due before in this week
not done
```
