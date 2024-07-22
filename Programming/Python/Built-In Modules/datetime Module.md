```python
# Create a date object & replace it
from datetime import date

my_date = date(2019, 11, 4)
my_date = my_date.replace(year=1992, month=10, day=21)
print(my_date.strftime('%Y/%m/%d'))                        # Output: 1992/10/21
print(my_date)


# Today's date
today = date.today()

print("Today:", today)
print("Year:", today.year)
print("Month:", today.month)
print("Day:", today.day)


# Create date from timestamp
from datetime import date
import time

timestamp = time.time()
print("Timestamp:", timestamp)
d = date.fromtimestamp(timestamp)
print("Date:", d)


# Create date object using the ISO Format
from datetime import date
d = date.fromisoformat('2019-11-04')       # fromisoformat method available from Python 3.7
print(d)


# Get the day of the week
from datetime import date
d = date(2019, 11, 4)
print(d.weekday())           # Gives range from 0-6 (0: Monday)
print(d.isoweekday())        # Gives range from 1-7


# datetime module
from datetime import datetime
dt = datetime(2019, 11, 4, 14, 53)

print("Datetime:", dt)
print("Date:", dt.date())
print("Time:", dt.time())
print("Timestamp:", dt.timestamp())
print("today:", datetime.today())
print("now:", datetime.now())
print("utcnow:", datetime.utcnow())


# Datetime Operations
d1 = date(2020, 11, 4)
d2 = date(2019, 11, 4)

print(d1 - d2)                 # Output: 366 days, 0:00:00 (Timedelta object)


# Timedelta
from datetime import timedelta

delta = timedelta(weeks=2, days=2, hours=3)
print("Days:", delta.days)                           # Days: 16
print("Seconds:", delta.seconds)                     # Seconds: 10800
print("Microseconds:", delta.microseconds)           # Microseconds: 0


delta2 = delta * 2                                   
print(delta2)                                        # 32 days, 4:00:00

d = date(2019, 10, 4) + delta2
print(d)                                             # 2019-11-05

dt = datetime(2019, 10, 4, 14, 53) + delta2
print(dt)                                            # 2019-11-05 18:53:00
```

![[Pasted image 20240326232750.png]]

today() — returns the current local date and time with the tzinfo attribute set to None;
now() — returns the current local date and time the same as the today method, unless we pass the optional argument tz to it. The argument of this method must be an object of the tzinfo subclass;
utcnow() — returns the current UTC date and time with the tzinfo attribute set to None.

---
# Time Module
```python
time(hour, minute, second, microsecond, tzinfo, fold)
```
tzinfo: Timezone (Default none)
fold: 0 or 1 (Default 0)
The constructor of the time class accepts six arguments (hour, minute, second, microsecond, tzinfo, and fold). Each of these arguments is optional.

```python
from datetime import time
t = time(14, 53, 20, 1)

print("Time:", t)
print("Hour:", t.hour)
print("Minute:", t.minute)
print("Second:", t.second)
print("Microsecond:", t.microsecond)

# Sleep method
import time
time.sleep(5)     # Int or float


# Convert Timestamp to String
timestamp = time.time()
print(time.ctime(timestamp))       # Output: Wed Mar 27 03:47:32 2024
```

#### Time Methods
```python
time.struct_time:
    tm_year   # specifies the year
    tm_mon    # specifies the month (value from 1 to 12)
    tm_mday   # specifies the day of the month (value from 1 to 31)
    tm_hour   # specifies the hour (value from 0 to 23)
    tm_min    # specifies the minute (value from 0 to 59)
    tm_sec    # specifies the second (value from 0 to 61 )
    tm_wday   # specifies the weekday (value from 0 to 6)
    tm_yday   # specifies the year day (value from 1 to 366)
    tm_isdst  # specifies whether daylight saving time applies (1 – yes, 0 – no, -1 – it isn't known)
    tm_zone   # specifies the timezone name (value in an abbreviated form)
    tm_gmtoff # specifies the offset east of UTC (value in seconds)


import time

timestamp = time.time()
print(time.gmtime(timestamp))
print(time.localtime(timestamp))

st = time.gmtime(timestamp)
print(time.strftime("%Y/%m/%d %H:%M:%S", st))           # Output: 2019/11/04 14:53:00
print(time.asctime(st))
print(time.mktime((2019, 11, 4, 14, 53, 0, 0, 308, 0)))

# Output
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=4, tm_hour=14, tm_min=53, tm_sec=0, tm_wday=0, tm_yday=308, tm_isdst=0)
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=4, tm_hour=14, tm_min=53, tm_sec=0, tm_wday=0, tm_yday=308, tm_isdst=0)
Mon Nov  4 14:53:00 2019
1572879180.0
```

The difference between them is that the *gmtime function* returns the struct_time object in UTC, while the *localtime function* returns local time. For the gmtime function, the tm_isdst attribute is always 0.
*asctime* converts a struct_time object or a tuple to a string.
*mktime* converts a struct_time object or a tuple that expresses the local time to the number of seconds since the Unix epoch.