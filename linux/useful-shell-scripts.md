# Useful Shell Scripts

## Delete files once it is more than 3

```bash
#!/bin/sh
cd /Users/jjiang153/testshell/files
fileNum=$(ls -l | wc -l)
fileNum=$((fileNum-1))
echo "${fileNum}"

if [ $[fileNum] -gt 3 ]
then
   deleteNum=$((fileNum-3))
   echo "${deleteNum}"
   rm -r $(ls -rt | head -n${deleteNum})
fi
```



