# Breach Parse
https://academy.tcm-sec.com/courses/1152300/lectures/24747381

We can use Breach-Parse to pull targeted information from the database.
```sh
breach-parse @tesla.com tesla.txt
```

Breach-Parse will then create three separate files. We are then able to read the contents of the master file to read both breached email addresses and plain text passwords.