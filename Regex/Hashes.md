# 🧩 **1. Hash Algorithm Names**

**Keywords:**
`md5`, `sha1`, `sha224`, `sha256`, `sha384`, `sha512`, `sha3`,
`bcrypt`, `scrypt`, `argon2`, `pbkdf2`,
`ripemd160`, `whirlpool`,
`hmac`,
`blake2` / `blake2b` / `blake2s`,
`crc32`, `crc64`,
`tiger`, `gost`, `skein`

**Regex:**

```regex
(?i)\b(md5|sha1|sha224|sha256|sha384|sha512|sha3|bcrypt|scrypt|argon2|pbkdf2|ripemd160|whirlpool|hmac|blake2|blake2b|blake2s|crc32|crc64|tiger|gost|skein)\b
```

---

## 🔑 Common Hex Hashes

* **MD5 (32 hex)**

```regex
\b[a-fA-F0-9]{32}\b
```

* **SHA1 / RIPEMD160 (40 hex)**

```regex
\b[a-fA-F0-9]{40}\b
```

* **SHA224 (56 hex)**

```regex
\b[a-fA-F0-9]{56}\b
```

* **SHA256 / GOST / Skein-256 (64 hex)**

```regex
\b[a-fA-F0-9]{64}\b
```

* **SHA384 (96 hex)**

```regex
\b[a-fA-F0-9]{96}\b
```

* **SHA512 / Whirlpool / Skein-512 / BLAKE2b (128 hex)**

```regex
\b[a-fA-F0-9]{128}\b
```

* **Skein-1024 (256 hex)**

```regex
\b[a-fA-F0-9]{256}\b
```

---

## 🔎 CRCs

* **CRC32 (8 hex)**

```regex
\b[a-fA-F0-9]{8}\b
```

* **CRC64 (16 hex)**

```regex
\b[a-fA-F0-9]{16}\b
```

---

## 🌀 Less Common Hex Hashes

* **Tiger (48 hex)**

```regex
\b[a-fA-F0-9]{48}\b
```

---

## 🔐 Password Hashing Schemes

* **bcrypt**

```regex
\$2[aby]?\$\d{2}\$[./A-Za-z0-9]{53}
```

* **scrypt (MCF)**

```regex
\$scrypt\$[^:\n]+:[^:\n]+:[^:\n]+\$[A-Za-z0-9+/=]+
```

* **argon2 (i/d/id)**

```regex
\$argon2(?:id|i|d)\$v=\d+\$m=\d+,t=\d+,p=\d+\$[A-Za-z0-9+/=]+\$[A-Za-z0-9+/=]+
```

* **PBKDF2 (MCF)**

```regex
\$pbkdf2-(?:sha1|sha256|sha512)\$\d+\$[A-Za-z0-9+/=]+\$[A-Za-z0-9+/=]+
```

---

# 🗂️ **2. Hash Variables / Fields**

**Keywords:**
`hash`, `hashed`,
`password_hash`, `passwd_hash`,
`token_hash`, `auth_hash`, `secret_hash`,
`digest`, `message_digest`, `data_digest`,
`checksum`, `file_checksum`, `crc`,
`signature`, `digital_signature`,
`hash_value`, `hash_key`,
`md5sum`, `sha1sum`, `sha256sum`,
`file_hash`, `blob_hash`,
`object_hash`, `content_hash`

**Regex:**

```regex
(?i)\b(hash|hashed|password_hash|passwd_hash|token_hash|auth_hash|secret_hash|digest|message_digest|data_digest|checksum|file_checksum|crc|signature|digital_signature|hash_value|hash_key|md5sum|sha1sum|sha256sum|file_hash|blob_hash|object_hash|content_hash)\b
```

---

# 🔏 **3. Password / Authentication Hashes**

**Keywords:**
`user_hash`, `login_hash`,
`password_digest`, `auth_digest`,
`salted_hash`, `peppered_hash`,
`hash_salt`, `hash_pepper`,
`pwd_hash`,
`user_digest`,
`password_fingerprint`

**Regex:**

```regex
(?i)\b(user_hash|login_hash|password_digest|auth_digest|salted_hash|peppered_hash|hash_salt|hash_pepper|pwd_hash|user_digest|password_fingerprint)\b
```

---

# 🔒 **4. API / Token / Security Hashes**

**Keywords:**
`api_hash`, `token_digest`, `token_hash`,
`auth_digest`, `auth_hash`,
`signature_hash`, `message_signature`, `digital_signature`,
`hmac_key`, `hmac_hash`,
`integrity_hash`,
`security_hash`,
`request_signature`,
`jwt_signature`

**Regex:**

```regex
(?i)\b(api_hash|token_digest|token_hash|auth_digest|auth_hash|signature_hash|message_signature|digital_signature|hmac_key|hmac_hash|integrity_hash|security_hash|request_signature|jwt_signature)\b
```
