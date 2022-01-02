# Commands details

Example:
```
FROM UBUNTU
CMD sleep 5
```

`docker run ubuntu-sleep 10`  runs the sleep container and waits for 10 seconds.   

---

Example:
```
FROM UBUNTU
ENTRYPOINT ["sleep"]
```

`docker run ubuntu-sleep 10` runs the sleep container and waits for 10 seconds.

---

Example:







`CMD` - the command that runs in the container eg. `sleep` or `nginx`



This being done in the container would be:
```
CMD sleep 5
```

Format:
```
CMD command param1			CMD sleep 5
CMD ["command", "param1"]	CMD ["sleep", "5"]  (correct way, and not!! ["sleep 5"]
```

`ENTRYPOINT ["sleep"]`  (is the command that is called on execution)  
`docker rune ubuntu-sleeper 10`  (this will append the 10 after the ENTRYPOINT)  

`CMD`  (is fully replaced by what you specify as an argument, eg. docker rune ubuntu-sleeper sleep 10)  
`ENTRYPOINT`  (has appended whatever you pass in, eg. docker run ubuntu-sleeper 10 = ENTRYPOINT 10)  


The best approach is below. This will not cause issues if you forget to pass in an argument when running the container  
```
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```

You can still override the entrypoint with (example):  
`docker run --entrypoint sleep2.0 ubuntu-sleeper 10` 