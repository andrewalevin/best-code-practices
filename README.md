# 👨‍💻 Best Code Practices




## Python

### Call Function name

```python

LOG_FORMAT_CALLED_FUNCTION = Template('💈 ${fname}():')

...

logger.debug(config.LOG_FORMAT_CALLED_FUNCTION.substitute(fname=inspect.currentframe().f_code.co_name))

```


### Signal

```python

def handle_suspend(signal, frame):
    """Handle the SIGTSTP signal (Ctrl+Z)."""
    logger.info("Process suspended. Exiting...")
    # No need to pause manually; the system handles the suspension
    sys.exit(0)

def handle_interrupt(signal, frame):
    """Handle the SIGINT signal (Ctrl+C)."""
    logger.info("Process interrupted by user. Exiting...")
    sys.exit(0)

signal.signal(signal.SIGTSTP, handle_suspend)
signal.signal(signal.SIGINT, handle_interrupt)
logger.info("Running ... Press Ctrl+C to stop or Ctrl+Z to suspend.")

```



### Humanize Pakacge

https://pypi.org/project/humanize/

![2024-11-21 17 24 20](https://github.com/user-attachments/assets/39953ad3-7252-4831-a34c-f54adc9aedb7)

```python
>>> import humanize
>>> import datetime as dt

>>> humanize.naturalday(dt.datetime.now())
'today'

>>> humanize.naturaldelta(dt.timedelta(seconds=1001))
'16 minutes'

>>> humanize.naturalday(dt.datetime.now() - dt.timedelta(days=1))
'yesterday'

>>> humanize.naturalday(dt.date(2007, 6, 5))
'Jun 05'

>>> humanize.naturaldate(dt.date(2007, 6, 5))
'Jun 05 2007'

>>> humanize.naturaltime(dt.datetime.now() - dt.timedelta(seconds=1))
'a second ago'

>>> humanize.naturaltime(dt.datetime.now() - dt.timedelta(seconds=3600))
'an hour ago'

```


## Handle SIGTSTP and SIGINT with asyncio 

This script demonstrates how to handle Unix signals (SIGTSTP for Ctrl+Z and SIGINT for Ctrl+C) in an asynchronous Python program using asyncio. It sets up custom handlers for these signals to manage program termination or interruption gracefully, ensuring cleanup of resources.

```python

import asyncio
import signal

# Event to signal program shutdown
shutdown_event = asyncio.Event()

# Asynchronous signal handler function
async def handle_interrupt_or_suspend(signum):
    """
    Handles received signals.
    - SIGTSTP (Ctrl+Z): Inform the user that suspending is disabled.
    - SIGINT (Ctrl+C): Signal program shutdown and perform graceful cleanup.
    """
    if signum == signal.SIGTSTP:
        print("\nReceived SIGTSTP (Ctrl+Z). Suspending is disabled.")
    elif signum == signal.SIGINT:
        print("\nReceived SIGINT (Ctrl+C). Exiting gracefully.")
        shutdown_event.set()  # Notify the main loop to terminate

# Function to set up signal handlers
def setup_signal_handlers(loop):
    """
    Registers custom signal handlers for SIGTSTP and SIGINT.
    Handlers run the asynchronous `handle_interrupt_or_suspend` function
    using the provided event loop.
    """
    def handler(signum, frame):
        asyncio.run_coroutine_threadsafe(handle_interrupt_or_suspend(signum), loop)

    # Register the custom handlers for signals
    signal.signal(signal.SIGTSTP, handler)
    signal.signal(signal.SIGINT, handler)

# Main asynchronous function
async def main():
    """
    Main program loop that runs indefinitely until a shutdown signal is received.
    """
    print("Press Ctrl+C to exit or Ctrl+Z to test SIGTSTP handling.")
    try:
        while not shutdown_event.is_set():
            print("Running... Press Ctrl+C to stop.")
            await asyncio.sleep(2)  # Simulate some ongoing work
    finally:
        print("Cleaning up...")  # Perform any cleanup before exiting

if __name__ == "__main__":
    # Create and set up a new event loop
    loop = asyncio.new_event_loop()
    asyncio.set_event_loop(loop)  # Set the event loop as the current loop
    setup_signal_handlers(loop)   # Configure signal handlers

    try:
        # Start the main asynchronous program
        loop.run_until_complete(main())
    finally:
        # Close the event loop on program termination
        loop.close()

```



