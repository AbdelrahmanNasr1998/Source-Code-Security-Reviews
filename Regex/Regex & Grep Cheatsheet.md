# 📘 Regular Expressions

---

## 🔹 Regular Expression Basics

* `.` → Any character except newline
* `a` → The character `a`
* `ab` → The string `"ab"`
* `a|b` → `a` or `b`
* `a*` → Zero or more `"a"`
* `\` → Escape special character

---

## 🔹 Quantifiers

* `*` → 0 or more
* `+` → 1 or more
* `?` → 0 or 1
* `{2}` → Exactly 2
* `{2,5}` → Between 2 and 5
* `{2,}` → 2 or more
* `{,5}` → Up to 5
* **Greedy by default** → Add `?` for reluctant match

---

## 🔹 Groups

* `( ... )` → Capturing group
* `(?P<Y> ... )` → Named capturing group `Y`
* `(?: ... )` → Non-capturing group
* `\Y` → Match Y-th captured group
* `(?P=Y)` → Match named group `Y`
* `(?# ... )` → Comment

---

## 🔹 Character Classes

* `[ab-d]` → One of: `a`, `b`, `c`, `d`
* `[^ab-d]` → Anything except `a`, `b`, `c`, `d`
* `[\b]` → Backspace character
* `\d` → Digit (0–9)
* `\D` → Non-digit
* `\s` → Whitespace
* `\S` → Non-whitespace
* `\w` → Word character (letters, digits, `_`)
* `\W` → Non-word character

---

## 🔹 Assertions

* `^` → Start of string
* `\A` → Start of string (ignores `m` flag)
* `$` → End of string
* `\Z` → End of string (ignores `m` flag)
* `\b` → Word boundary
* `\B` → Non-word boundary
* `(?=...)` → Positive lookahead
* `(?!...)` → Negative lookahead
* `(?<=...)` → Positive lookbehind
* `(?<!...)` → Negative lookbehind
* `(?()|)` → Conditional

---

## 🔹 Flags

* `i` → Ignore case
* `m` → `^` and `$` match start/end of line
* `s` → `.` matches newline as well
* `x` → Allow spaces & comments
* `L` → Locale character classes
* `u` → Unicode character classes
* `(?iLmsux)` → Inline flags

---

## 🔹 Special Characters

* `\n` → Newline
* `\r` → Carriage return
* `\t` → Tab
* `\YYY` → Octal character
* `\xYY` → Hexadecimal character

---

## 🔹 Replacement

* `\g<0>` → Entire match
* `\g<Y>` → Match group Y (name or number)
* `\Y` → Insert group number Y

---

# 📘 `grep` with Regex

---

## 🔹 Useful Options

* `--color=auto` → Highlight matches in color
* `-r` → Recursive (search in all files inside folder)
* `-i` → Case-insensitive search
* `-n` → Show line numbers
* `-E` → Extended regex (for `|`, `+`, `{}` etc.)
* `--exclude-dir=NAME` → Skip directory

👉 Example:

```bash
grep -ri --color=auto "pattern" folder/
```

---

## 🔹 Examples

### 1. Search "error" in logs folder

```bash
grep -rin --color=auto "error" logs/
```

### 2. Search "error" OR "fail"

```bash
grep -rin --color=auto -E "error|fail" logs/
```

### 3. Search lines starting with "INFO"

```bash
grep -rin --color=auto "^INFO" logs/
```

### 4. Find emails in text

```bash
grep -rin --color=auto "[a-zA-Z0-9._%+-]\+@[a-zA-Z0-9.-]\+\.[a-z]\{2,4\}" docs/
```

### 5. Find numbers

```bash
grep -rin --color=auto "[0-9]\+" data/
```

### 6. Show line numbers too

```bash
grep -rin --color=auto "warning" logs/
```

### 7. Search excluding `.git` folder

```bash
grep -rin --color=auto --exclude-dir=".git" "password" project/
```
