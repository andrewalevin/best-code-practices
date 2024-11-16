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
