
# Kill on keyword search in grep qemu

```
ps aux | grep qemu | awk '{print $2}' | xargs kill -9
```
