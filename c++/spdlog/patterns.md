---
description: this page describes all the different patterns in spdlog
---

# Patterns

To set a custom logging pattern with spdlog in C++, you can use the set\_pattern() function. The pattern string can contain various placeholders that will be replaced by corresponding values when logging. The following are some of the placeholders that can be used in the pattern string:

* %v: the log message
* %n: the logger name
* %^: start color
* %$: end color
* %t: thread id
* %T: time
* %P: process id
* %F: source file name
* %l: log level
* %L: short log level name

For example, to set a pattern that includes the log level, time, logger name, and log message, use the following code:

```cpp
spdlog::set_pattern("[%L] [%T] [%n] %v");
```

This will produce logs in the following format:

```log
[info] [12:34:56] [my_logger] This is a log message
```

For more information on the available placeholders and their usage, check out the [spdlog documentation](https://github.com/gabime/spdlog#pattern-flags).
