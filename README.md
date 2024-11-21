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



