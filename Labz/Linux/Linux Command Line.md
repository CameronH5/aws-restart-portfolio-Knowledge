# Task: Run Familiar Commands

In this exercise, I ran several commands to gain general knowledge about the system and session that I was using.

## Command: whoami

I entered `whoami` and pressed Tab to notice that the auto-complete feature displayed the full command. I pressed Enter to display my current username.

```
[ec2-user@ip-10-0-10-105 ~]$ whoami
ec2-user
```

The `whoami` command confirmed that I was logged in as the `ec2-user`.

## Command: hostname -s

I entered `hostname -s` and pressed Enter to display a shortened version of the computer's host name.

```
[ec2-user@ip-10-0-10-105 ~]$ hostname -s
ip-10-0-10-105
```

This command displayed the shortened host name of the system I was connected to.

## Command: uptime -p

I entered `uptime -p` and pressed Enter to display the uptime of the system in an easily readable format.

```
[ec2-user@ip-10-0-10-105 ~]$ uptime -p
up 3 minutes
```

This output showed that the system had been running for 3 minutes.

## Command: who -H -a

I entered `who -H -a` and pressed Enter to display information about the users who were logged in and some additional information. This command displayed the name, line which gives information, time the event occurred, idle time of the user, Process Identifier (PID), comment and exit time.

*(Command output from instruction set - showing user login information)*

## Command: Timezone Commands

I entered `TZ=America/New_York date` and pressed Enter. Then I entered `TZ=America/Los_Angeles date` and pressed Enter. These commands identified the date and time of alternate locations in the world.

```
[ec2-user@ip-10-0-10-105 ~]$ TZ=America/New_York date
Thu Jun 18 10:43:11 EDT 2026
[ec2-user@ip-10-0-10-105 ~]$ TZ=America/Los_Angeles date
Thu Jun 18 07:43:30 PDT 2026
```

The output showed the current date and time in both New York (EDT) and Los Angeles (PDT) time zones.

## Command: cal -j

I entered `cal -j` and pressed Enter to display the calendar in Julian date format.

```
[ec2-user@ip-10-0-10-105 ~]$ cal -j
      June 2026
Sun Mon Tue Wed Thu Fri Sat
    152 153 154 155 156 157
158 159 160 161 162 163 164
165 166 167 168 169 170 171
172 173 174 175 176 177 178
179 180 181
```

The output displayed June 2026 with Julian dates, showing the consecutive day numbering for the month.

## Command: cal -s

I entered `cal -s` and pressed Enter to display the calendar with Sunday as the first day of the week.

```
[ec2-user@ip-10-0-10-105 ~]$ cal -s
      June 2026
Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30
```

This view displayed the calendar with Sunday through Saturday layout.

## Command: cal -m

I entered `cal -m` and pressed Enter to display the calendar with Monday as the first day of the week.

```
[ec2-user@ip-10-0-10-105 ~]$ cal -m
      June 2026
Mo Tu We Th Fr Sa Su
                   1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30
```

This output displayed the calendar with Monday through Sunday layout.

## Command: id ec2-user

I entered `id ec2-user` and pressed Enter to see my unique ID and group information.

```
[ec2-user@ip-10-0-10-105 ~]$ id ec2-user
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),4(adm),10(wheel),190(systemd-journal),1011(Sales),1012(HR),1013(Finance),1015(Shipping),1016(Manager),1021(CEO)
```

The output displayed my user ID (uid), group ID (gid), and all the groups that I was a member of, including Sales, HR, Finance, Shipping, Manager, and CEO groups.

---

# Task: Improve Workflow Through History and Search

In this task, I attempted to ease my overall workload by reusing commands through search techniques, manual visualization of the bash history log, and reuse of the last command.

## Viewing Command History

I started by viewing the current bash history. I entered `history` and pressed Enter. In the output, I checked if the commands that I saw were the commands that I used in Task 2.

```
[ec2-user@ip-10-0-10-105 ~]$ history
    1  whoa
    2  whoami
    3  hostname -s
    4  uptime -p
    5  who -H -a
    6  TZ=America/New_York date
    7  TZ=America/Los_Angeles date
    8  cal -j
    9  cal -s
   10  cal -m
   11  id ec2-user
   12  history
[ec2-user@ip-10-0-10-105 ~]$
```

The history command displayed all the commands I had executed during this session, confirming that all the commands from Task 2 were recorded in the history, including whoami, hostname, uptime, timezone commands, calendar commands, and the id command.

## Reverse History Search

I pressed CTRL+R to bring up reverse history search. In the reverse history search feature of the terminal, I entered `TZ` to search for the date command that I used in the previous steps. This search brought up and used the `date` command that I can edit and run by using arrow buttons to modify the command inline.

```
(reverse-i-search)`TZ': TZ=America/Los_Angeles date
```

Using the reverse history search, I was able to quickly locate previously used commands without having to scroll through the entire history. The search found the most recent timezone command I had used.

## Rerunning the Last Command

I entered `date` into the terminal and pressed Enter. Then I entered `!!` and pressed Enter. This gave me the ability to rerun the most recent command.

```
[ec2-user@ip-10-0-10-105 ~]$ date
Thu Jun 18 14:48:04 UTC 2026
```

The `date` command displayed the current system date and time. Then using `!!`, I was able to execute the last command that was entered into the keyboard without retyping it, providing an efficient way to repeat frequently used commands.

---

## Summary

Through these tasks, I gained practical experience with:
- Basic system information commands (whoami, hostname, uptime, id)
- User and login information (who)
- Timezone and date manipulation
- Calendar viewing with different formats
- Command history navigation and reuse
- Reverse history search for efficient command recall
- Using `!!` to rerun the last command without retyping
