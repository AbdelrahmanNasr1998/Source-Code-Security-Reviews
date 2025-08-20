# 🖨️ **1. Generic Print / Debug Statements**

**Keywords:**
`print`, `printf`, `println`,
`System.out.println` (Java),
`Console.WriteLine`, `Debug.WriteLine` (.NET),
`echo` (PHP, Bash),
`write`, `writeln`,
`puts`, `p` (Ruby),
`trace`, `tracef`,
`format`,
`die`, `var_dump`, `dump`, `dd` (Laravel),
`alert` (JS),
`document.write` (JS),
`debug`, `debugf`

**Regex:**

```regex
(?i)\b(print|printf|println|System\.out\.println|Console\.WriteLine|Debug\.WriteLine|echo|write|writeln|puts|trace|tracef|format|die|var_dump|dump|dd|alert|document\.write|debug|debugf|p)\b
```

---

# 📜 **2. Logging Functions / Libraries**

**Keywords:**
`log`,
`logger`, `logging`, `Log`,
`log.debug`, `log.trace`,
`log.info`, `log.notice`,
`log.warn`, `log.warning`,
`log.error`, `log.critical`, `log.fatal`,
`console.log`, `console.debug`, `console.info`, `console.warn`, `console.error`,
`syslog`,
`tracing`,
`traceback`,
`NSLog` (Objective-C),
`android.util.Log` (`Log.d`, `Log.i`, `Log.w`, `Log.e`)
`SLF4J`, `Log4j`, `logback` (Java),
`Serilog`, `NLog` (.NET),
`Winston`, `Bunyan`, `Pino` (Node.js)

**Regex:**

```regex
(?i)\b(log|logger|logging|log\.debug|log\.trace|log\.info|log\.notice|log\.warn|log\.warning|log\.error|log\.critical|log\.fatal|console\.log|console\.debug|console\.info|console\.warn|console\.error|syslog|tracing|traceback|NSLog|Log\.d|Log\.i|Log\.w|Log\.e|SLF4J|Log4j|logback|Serilog|NLog|Winston|Bunyan|Pino)\b
```

---

# 🔑 **3. Sensitive Logging Keywords**

Developers often leak **secrets in logs**. Look for these words near logging/print statements:

**Keywords:**
`password`, `pwd`, `passwd`, `passphrase`,
`secret`,
`token`, `auth_token`, `access_token`, `refresh_token`,
`api_key`, `apikey`, `client_secret`,
`key`, `private_key`, `public_key`,
`hash`, `digest`, `checksum`,
`credentials`, `auth`, `authorization`,
`jwt`, `bearer`,
`session`, `cookie`,
`ssn`, `credit_card`, `card_number`,
`pin`, `cvv`

**Regex:**

```regex
(?i)\b(password|pwd|passwd|passphrase|secret|token|auth_token|access_token|refresh_token|api[_-]?key|apikey|client_secret|key|private_key|public_key|hash|digest|checksum|credentials|auth|authorization|jwt|bearer|session|cookie|ssn|credit[_-]?card|card_number|pin|cvv)\b
```
