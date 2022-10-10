# When to use the different log levels

## There are different ways to log messages, in order of fatality:

1. FATAL
2. ERROR
3. WARN
4. INFO
5. DEBUG
6. TRACE

> How do I decide when to use which?<br>
> What's a good heuristic to use?



![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/10/09/1.png)

- **Trace** - Only when I would be "tracing" the code and trying to find one **part** of a function specifically.<br>
- **Debug** - Information that is diagnostically helpful to people more than just developers (IT, sysadmins, etc.).<br>
- **Info** - Generally useful information to log (service start/stop, configuration assumptions, etc). Info I want to always have available but usually don't care about under normal circumstances. This is my out-of-the-box config level.<br>
- **Warn** - Anything that can potentially cause application oddities, but for which I am automatically recovering. (Such as switching from a primary to backup server, retrying an operation, missing secondary data, etc.)<br>
- **Error** - Any error which is fatal to the **operation**, but not the service or application (can't open a required file, missing data, etc.). These errors will force user (administrator, or direct user) intervention. These are usually reserved (in my apps) for incorrect connection strings, missing services, etc.<br>
- **Fatal** - Any error that is forcing a shutdown of the service or application to prevent data loss (or further data loss). I reserve these only for the most heinous errors and situations where there is guaranteed to have been data corruption or loss.<br>

