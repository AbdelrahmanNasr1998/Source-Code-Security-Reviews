# 🛡️ **Authentication / Tokens**

**Keywords:**
`secret`, `key`, `token`, `api_key` / `apikey` / `api-key`, `client_secret`, `access_token`, `auth_token`, `refresh_token` `jwt`, `bearer`, `token_secret`, `app_secret`, `master_key`, `root_key`, `system_key`, `super_secret`, `vault_token`

**Regex:**

```regex
(?i)\b(secret|key|token|api[_-]?key|client_secret|access_token|auth_token|refresh_token|jwt|bearer|token_secret|app_secret|master_key|root_key|system_key|super_secret|vault_token)\b
```

---

# 🔑 **Passwords**

**Keywords:**
`password`, `pwd`, `passphrase`, `passwd`, `db_password` / `database_password`, `root_password`, `sql_password`, `mysql_password`, `postgres_password`, `redis_password`, `smtp_password`, `smtp_secret`, `keystore_password`, `truststore_password`, `vpn_password`, `vpn_psk`

**Regex:**

```regex
(?i)\b(password|pwd|passphrase|passwd|db_password|database_password|root_password|sql_password|mysql_password|postgres_password|redis_password|smtp_password|smtp_secret|keystore_password|truststore_password|vpn_password|vpn_psk)\b
```

---

# 🔒 **Security / Cryptography**

**Keywords:**
`private_key`, `public_key`, `signing_key`, `encryption_key`, `hmac`, `salt`, `credential`, `credentials`, `certificate`, `cert`, `pem`, `pfx`, `openvpn_key`, `wireguard_private_key`

**Regex:**

```regex
(?i)\b(private_key|public_key|signing_key|encryption_key|hmac|salt|credential|credentials|certificate|cert|pem|pfx|openvpn_key|wireguard_private_key)\b
```

---

# 🔐 **Tokens / API / Auth (Generic)**

**Keywords:**
`auth`, `authorization`, `api_secret`, `secret_key`, `registry_token`, `registry_password`, `docker_token`, `docker_password`

**Regex:**

```regex
(?i)\b(auth|authorization|api_secret|secret_key|registry_token|registry_password|docker_token|docker_password)\b
```

---

# ☁️ **AWS**

**Keywords:**
`aws_access_key_id`, `aws_secret_access_key`, `aws_session_token`, `aws_key`, `aws_secret_key`, `aws_account_id`, `ses_secret_key`

**Regex:**

```regex
(?i)\b(aws_access_key_id|aws_secret_access_key|aws_session_token|aws_key|aws_secret_key|aws_account_id|ses_secret_key)\b
```

---

# 🌐 **Google Cloud (GCP)**

**Keywords:**
`gcp_key`, `gcp_service_account`, `gcp_client_email`, `gcp_private_key_id`, `google_api_key`, `google_client_secret`

**Regex:**

```regex
(?i)\b(gcp_key|gcp_service_account|gcp_client_email|gcp_private_key_id|google_api_key|google_client_secret)\b
```

---

# 🔷 **Azure**

**Keywords:**
`azure_client_id`, `azure_client_secret`, `azure_tenant_id`, `azure_subscription_id`, `azure_storage_account_key`, `azure_storage_connection_string`, `azure_devops_token`

**Regex:**

```regex
(?i)\b(azure_client_id|azure_client_secret|azure_tenant_id|azure_subscription_id|azure_storage_account_key|azure_storage_connection_string|azure_devops_token)\b
```

---

# 🗄️ **Database Credentials**

**Keywords:**
`db_user` / `db_username`, `db_pass` / `db_password`, `sql_password`, `mongo_uri` / `mongodb_uri`, `postgres_password`, `mysql_password`, `redis_password`

**Regex:**

```regex
(?i)\b(db_user|db_username|db_pass|db_password|sql_password|mongo_uri|mongodb_uri|postgres_password|mysql_password|redis_password)\b
```

---

# ⚙️ **CI/CD & DevOps**

**Keywords:**
`github_token`, `gitlab_token`, `bitbucket_token`, `circleci_token`, `travis_token`, `jenkins_token`, `teamcity_token`

**Regex:**

```regex
(?i)\b(github_token|gitlab_token|bitbucket_token|circleci_token|travis_token|jenkins_token|teamcity_token)\b
```

---

# ☸️ **Kubernetes / Containers**

**Keywords:**
`kubeconfig`, `kubernetes_token`, `k8s_token`

**Regex:**

```regex
(?i)\b(kubeconfig|kubernetes_token|k8s_token)\b
```

---

# 🌍 **Cloudflare / Networking**

**Keywords:**
`cloudflare_api_key`, `cloudflare_api_token`, `cf_token`, `cf_api_key`

**Regex:**

```regex
(?i)\b(cloudflare_api_key|cloudflare_api_token|cf_token|cf_api_key)\b
```

---

# 📧 **Email / Messaging Services**

**Keywords:**
`sendgrid_api_key`, `mailgun_api_key`, `postmark_api_token`, `gmail_client_secret`

**Regex:**

```regex
(?i)\b(sendgrid_api_key|mailgun_api_key|postmark_api_token|gmail_client_secret)\b
```

---

# 📊 **Monitoring / Logging / Error Tracking**

**Keywords:**
`datadog_api_key`, `datadog_app_key`, `sentry_dsn`, `newrelic_license_key`, `rollbar_access_token`, `bugsnag_api_key`

**Regex:**

```regex
(?i)\b(datadog_api_key|datadog_app_key|sentry_dsn|newrelic_license_key|rollbar_access_token|bugsnag_api_key)\b
```

---

# 📱 **Social Media / Third-Party APIs**

**Keywords:**
`facebook_app_secret`, `twitter_api_key`, `twitter_api_secret`, `twitter_bearer_token`, `linkedin_client_secret`, `instagram_access_token`

**Regex:**

```regex
(?i)\b(facebook_app_secret|twitter_api_key|twitter_api_secret|twitter_bearer_token|linkedin_client_secret|instagram_access_token)\b
```

---

# 📲 **Mobile Apps (iOS/Android)**

**Keywords:**
`firebase_api_key`, `onesignal_api_key`, `expo_access_token`, `mapbox_access_token`, `google_maps_api_key`, `apple_signin_client_secret`

**Regex:**

```regex
(?i)\b(firebase_api_key|onesignal_api_key|expo_access_token|mapbox_access_token|google_maps_api_key|apple_signin_client_secret)\b
```

---

# 💳 **Payment / Financial**

**Keywords:**
`stripe_secret_key`, `stripe_api_key`, `paypal_client_id`, `paypal_client_secret`, `braintree_key`, `square_access_token`, `square_client_secret`, `adyen_api_key`, `checkout_com_secret_key`, `worldpay_api_key`

**Regex:**

```regex
(?i)\b(stripe_secret_key|stripe_api_key|paypal_client_id|paypal_client_secret|braintree_key|square_access_token|square_client_secret|adyen_api_key|checkout_com_secret_key|worldpay_api_key)\b
```

---

# 📦 **Misc / Other Services**

**Keywords:**
`slack_token`, `discord_token`, `telegram_bot_token`, `twilio_account_sid`, `twilio_auth_token`, `npm_token`, `dockerhub_password`, `heroku_api_key`, `firebase_api_key`, `pypi_token`

**Regex:**

```regex
(?i)\b(slack_token|discord_token|telegram_bot_token|twilio_account_sid|twilio_auth_token|npm_token|dockerhub_password|heroku_api_key|firebase_api_key|pypi_token)\b
```
