# Advanced Timer

The `AdvancedTimer` class is a custom timer implementation that extends the functionality of the `System.Timers.Timer` class. It provides additional features for tracking and managing elapsed time intervals, making it useful for various timing and scheduling scenarios in .NET 6.0 and later.

### Namespace

```csharp
namespace Chase.CommonLib.Math;
```

### License

The `AdvancedTimer` class is part of the Chase CommonLib library, developed by LFInteractive LLC, and is licensed under the [GNU General Public License (GPL-3.0)](https://www.gnu.org/licenses/gpl-3.0.en.html#license-text).

### Constructor

#### `AdvancedTimer(double duration)`

Creates an instance of the `AdvancedTimer` class with a specified duration in milliseconds.

* **Parameters:**
  * `duration` (double): The duration of the timer in milliseconds.

Example:

```csharp
AdvancedTimer timer = new AdvancedTimer(1000); // Creates a timer with a 1-second interval.
```

### Properties

#### `Enabled`

Gets or sets a value indicating whether the `AdvancedTimer` is able to raise events at a defined interval. This property overrides the base `Enabled` property from `System.Timers.Timer`.

* **Type:** bool
* **Read-only**

#### `Interruptible`

Gets or sets whether the `Start()` method can be run while the timer is already running. When set to `true`, the timer will restart without raising the event. When set to `false`, an exception will be thrown if `Start()` is called while the timer is running.

* **Type:** bool
* **Read-write**

#### `IsRunning`

Indicates whether the `AdvancedTimer` is currently running or stopped. This property is set when using the `Start()` and `Stop()` methods.

* **Type:** bool
* **Read-only**

### Methods

#### `GetNextElapseTime()`

Gets the time when the timer will elapse next.

* **Returns:** DateTime

Example:

```csharp
DateTime nextElapseTime = timer.GetNextElapseTime();
```

#### `GetRemaining()`

Gets the number of milliseconds remaining until the next timer elapse is triggered.

* **Returns:** double

Example:

```csharp
double remainingMilliseconds = timer.GetRemaining();
```

#### `GetRemainingTime()`

Gets the time remaining until the next timer elapse is triggered as a `TimeSpan`.

* **Returns:** TimeSpan

Example:

```csharp
TimeSpan remainingTime = timer.GetRemainingTime();
```

#### `Reset()`

Restarts the `AdvancedTimer` without triggering the elapse event.

Example:

```csharp
timer.Reset();
```

#### `Start()`

Starts the `AdvancedTimer`. If called while the timer is already running and `Interruptible` is set to `false`, an exception is thrown.

* **Exceptions:** `InvalidOperationException` - Thrown when the timer is already running, and `Interruptible` is set to `false`.

Example:

```csharp
timer.Start();
```

#### `Stop()`

Stops the `AdvancedTimer`.

Example:

```csharp
timer.Stop();
```

#### `ToString()`

Overrides the `ToString()` method to provide a custom representation of the `AdvancedTimer` object. If the timer is running, it returns the remaining time as a string; otherwise, it returns a JSON representation of the object.

* **Returns:** string

Example:

```csharp
string timerInfo = timer.ToString();
```

### Example Usage

```csharp
using Chase.CommonLib.Math;

// Create an AdvancedTimer with a 2-second interval
AdvancedTimer timer = new AdvancedTimer(2000);

// Start the timer
timer.Start();

// Check if the timer is running
if (timer.IsRunning)
{
    // Get the remaining time until the next elapse
    double remainingMilliseconds = timer.GetRemaining();
    Console.WriteLine($"Remaining time: {remainingMilliseconds} milliseconds");
    
    // Stop the timer
    timer.Stop();
    
    // Reset and restart the timer
    timer.Reset();
    
    // Get the next elapse time
    DateTime nextElapseTime = timer.GetNextElapseTime();
    Console.WriteLine($"Next elapse time: {nextElapseTime}");
}
```

In this example, an `AdvancedTimer` is created with a 2-second interval. It is started, and its properties and methods are demonstrated, including checking the remaining time until the next elapse, stopping the timer, resetting it, and getting the next elapse time.
