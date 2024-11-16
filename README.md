# 👨‍💻 Best Code Practices




## Python

### Call Function name

```python

LOG_FORMAT_CALLED_FUNCTION = Template('💈 ${fname}():')

...

logger.debug(config.LOG_FORMAT_CALLED_FUNCTION.substitute(fname=inspect.currentframe().f_code.co_name))

```
