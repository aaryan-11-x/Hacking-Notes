
| Day of the Week | Integer Value | Constant           |
| --------------- | ------------- | ------------------ |
| Monday          | 0             | calendar.MONDAY    |
| Tuesday         | 1             | calendar.TUESDAY   |
| Wednesday       | 2             | calendar.WEDNESDAY |
| Thursday        | 3             | calendar.THURSDAY  |
| Friday          | 4             | calendar.FRIDAY    |
| Saturday        | 5             | calendar.SATURDAY  |
| Sunday                | 6              | calendar.SUNDAY                   |

```python
# Print calendar of a year
import calendar

print(calendar.calendar(2020))
## OR
calendar.prcal(2020)
# w – date column width (default 2)
# l – number of lines per week (default 1)
# c – number of spaces between month columns (default 6)
# m – number of columns (default 3)


# Calendar of a specific month
print(calendar.month(2024, 11))
## OR
calendra.prmonth(2024, 11)


# Change the first day of a week (Default: Monday)
calendar.setfirstweekday(calendar.SUNDAY) or calendar.setfirstweekday(6)


# Get weekday (day of the week) as an int value
print(calendar.weekday(2020, 12, 24))


# Check for leap years
print(calendar.isleap(2020))
print(calendar.leapdays(2010, 2021))  # Up to but not including 2021


# Iterate through dates in a month
c = calendar.Calendar()

for date in c.itermonthdates(2019, 11):
    print(date, end=" ")
```