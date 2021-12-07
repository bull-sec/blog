# PowerShell for Pentesters

## Potential PowerShell Locations

- 32-bit (x86) PowerShell executable: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`
- 64-bit (x64) Powershell executable: `%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe`
- 32-bit (x86) Powershell ISE executable: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell_ise.exe`
- 64-bit (x64) Powershell ISE executable: `%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell_ise.exe`

## The Very Basics

```bash
Get-ChildItem = ls or dir.

| = pipe, super powerful in PowerShell (as it is in bash), used to fork processes/functions through each other.

Get-Help = Help command, all functions

Also:

Help <function-name>
```

## Functions

```powershell
function Invoke-HelloWorld {
    $blah = "Hello World";
    Write-Output $blah;
}
```

We can invoke this in a few ways.

They all amount to the same thing, but one may be more suited to your particular situation than the other.

We can just type the above out then call `Invoke-HelloWorld`.

If we save the script to a file ("HelloWorld.ps1") we can do something like:

```powershell
. .\HelloWorld.ps1
Invoke-HelloWorld
```

And that will fire the script.

The last (the last main way at least as there are lots of others) is to just modify the script to call whatever function you want to call right at the bottom.

```powershell
function Invoke-HelloWorld {
    $blah = "Hello World";
    Write-Output $blah;
}
Invoke-HelloWorld
```

This last one is useful for things like reverse shells that you want to fire as soon as they land, and used in conjunction with an `IEX` cradle this is a great way to deploy payloads to a target machine without having to write anything to disk.

