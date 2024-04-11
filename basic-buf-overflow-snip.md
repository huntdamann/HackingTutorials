# Buffer Overflow Snippet

``` console
foo@bar:~$ python -c 'import sys; sys.stdout.buffer.write(b"a" * 112 + b"\n")'
```

- The amount of a's written into buffer is arbitrary
