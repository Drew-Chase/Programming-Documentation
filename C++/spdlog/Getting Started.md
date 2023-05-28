# Getting Started

1. **Include the spdlog library**

To use spdlog, include the header file in your C++ code:

```cpp
#include "spdlog/spdlog.h"
```

2. **Basic logging**

Set up basic logging with different log levels and formats:

```cpp
spdlog::set_pattern("[%H:%M:%S.%f] [%L] [thread %t] %v");
spdlog::info("Welcome to spdlog!");
spdlog::error("Some error message with arg: {}", 1);
spdlog::warn("Easy padding in numbers like {:08d}", 12);
spdlog::critical("Support for int: {0:d};  hex: {0:x};  oct: {0:o}; bin: {0:b}", 42);
spdlog::info("Support for floats {:03.2f}", 1.23456);
spdlog::info("Positional args are {1} {0}..", "too", "supported");
spdlog::info("{:<30}", "left aligned");
spdlog::set_level(spdlog::level::debug);
spdlog::debug("This message should be displayed..");
```

4. **Installing using** [**VCPKG**](../VCPKG.md)

To use spdlog with static linking, download the static libraries for your desired platform using vcpkg:

```sh
vcpkg install spdlog
```

{% content-ref url="../VCPKG.md" %}
[VCPKG.md](../VCPKG.md)
{% endcontent-ref %}

For a more in-depth understanding of spdlog features, refer to the [official documentation](https://github.com/gabime/spdlog).
