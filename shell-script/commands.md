## Search files
```
# list files/dirs in descending order(recursive)
du -ah . | sort -rh | head -5

# list files in descending order(recursive)
du -ah . | grep -v "/$" | sort -rh | head -5
```
