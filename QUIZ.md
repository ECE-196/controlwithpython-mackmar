### How does the DevBoard handle received serial messages? How does this differ from the naïve approach?

The DevBoard handles received serial messages by reading data from the serial port using the `read` method of the `LockedSerial` class. This method is overridden to ensure thread-safe access to the serial port by acquiring a lock (`_lock`) before reading data. This differs from the naive approach where serial communication could lead to race conditions or data corruption if multiple threads attempt to read from the serial port simultaneously without synchronization.

### What does `detached_callback` do? What would happen if it wasn't used?

The `detached_callback` decorator is used to execute functions in separate threads. Without it, functions like `connect`, `update_led`, `disconnect`, and `send_invalid` would execute in the main thread, potentially blocking the GUI and making it unresponsive during tasks like serial communication. By using `detached_callback`, these functions are executed asynchronously in separate threads, allowing the GUI to remain responsive.

### What does `LockedSerial` do? Why is it _necessary_?

The `LockedSerial` class extends the `Serial` class from the `serial` module and adds thread-safe behavior to serial communication. It achieves this by using a lock (`_lock`) to synchronize access to the serial port. Methods like `read`, `write`, and `close` are overridden to acquire the lock before performing any operation on the serial port. This is necessary because serial communication is typically not thread-safe, and accessing the serial port from multiple threads concurrently can lead to data corruption or undefined behavior. By using `LockedSerial`, access to the serial port is serialized, ensuring that only one thread can access it at a time and preventing race conditions.
