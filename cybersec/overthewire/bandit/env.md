- used for running programs with environment variables.
- example (setting the DEBUG=1 for running myscript.py): 

```bash
env DEBUG=1 python3 myscript.py
```

- `-S` - argument used to split the string provided by user into "command" + "arguments". It doesn't interpret the entire string as a command anymore.