# YourStrongBox API Documentation

[YourStrongBox](https://yourstrongbox.com/) is a personal secure password manager.

#### Public API methods
- **[Login](#login)**
- **[Generate password](#gen-pass)**

#### Private API methods
- **[Generate RSA keys](#gen-rsa-keys)**

## Public API methods

### <a id="login"></a>Login

POST:

```
https://yourstrongbox.com/api/login
```
**Headers:**

```
{
  "Content-Type": "application/json"
}
```
**Body:**

```
{
  "email": "some@email.com",
  "password": "AAAAAGKoKtf34WDVSMs4looEpOc1S5..."
}
```
**Fields:**

- **email** - user email.
- **password** - API password previously generated in the account settings on the API tab.

![img](imgs/settings-api-pass.png)

**Response:**

```
{
  "token_type": "Bearer",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9..."
}
```

### <a id="gen-pass"></a>Generate password
GET:

```
https://yourstrongbox.com/api/password/generate?length=16&spec=n
```
**Headers:**

```
{
  "Content-Type": "application/json"
}
```

**Query params:**

- **length** (int) - password length (default=8).
- **upper** (t/f) - whether or not to use upper characters to generate a password (default=t).
- **lower** (t/f) - whether or not to use lower characters to generate a password (default=t).
- **digits** (t/f) - whether or not to use digits to generate a password (default=t).
- **spec** (t/f) - whether or not to use special characters to generate a password (default=t).

**Response:**

```
{
  "password": "KAgTdFtUB1ZSMVLG",
  "entropy": 95
}
```

## Private API methods

### <a id="gen-rsa-keys"></a>Generate RSA keys
POST:

```
https://yourstrongbox.com/api/rsa/generate/keys
```

**Headers:**

```
{
  "Content-Type": "application/json",
  "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9..."
}
```
**Body:**

```
{
  "key_size": 3072
}
```

**Fields:**

 - **key_size** (int: 1024/2048/3072/7680) - key size (default=2048).

**Response:**

```
{
  "private_key": "-----BEGIN PRIVATE KEY-----...",
  "public_key": "-----BEGIN PUBLIC KEY-----..."
}
```
### <a id="password-check"></a>Check Password
Checking password strength

POST:

```
https://yourstrongbox.com/api/password/check
```

**Headers:**

```
{
  "Content-Type": "application/json",
  "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9..."
}
```
**Body:**

```
{
  "password": "cXdlcnR5"
}
```

**Fields:**

 - **password** - password must be encoded in base64 format.

**Response:**

```
{
  "leaks": {
    "value": 10713794,
    "value_display": "10 713 794"
  },
  "complexity": {
    "value": 28,
    "description": "This password is not secure and can be easily cracked using brute force"
  },
  "strength": {
    "score": 0,
    "password": "Very weak",
    "guesses": 5,
    "guesses_log10": 0.6989700043360187,
    "crack_times_seconds": {
      "online_throttling_per_hour": 180.0,
      "online_no_throttling_per_second": 0.5,
      "offline_slow_hashing_per_second": 0.0005,
      "offline_fast_hashing_per_second": 5e-10
    },
    "crack_times_display": {
      "online_throttling_per_hour": "3 minutes",
      "online_no_throttling_per_second": "less than a second",
      "offline_slow_hashing_per_second": "less than a second",
      "offline_fast_hashing_per_second": "less than a second"
    },
    "feedback": {
      "warning": "This is a top-10 common password.",
      "suggestions": [
        "Add another word or two. Uncommon words are better."
      ]
    }
  }
}
```
**Description**

- `leaks` - shows the number of times the password has been found in public access (or the Darknet) due to data breaches, server attacks, phishing attacks, or other methods of compromising security systems.
	- `value` - actual value.
	- `value_display` - user-friendly value.
- `complexity` - password complexity.
	- `value` - actual value (< 40 - Poor; 40 <= 50 - Mediocre; 50 <= 80 - Strong; 80 < - Very strong).
	- `description` - description of the value for the user.
- `strength` - password strength assessment.
	- `score` - an integer from 0 to 4 representing the overall strength of the password (0 - Very weak; 1 - Weak; 2 - Mediocre; 3 - Strong; 4 - Very strong).
	- `password` - user-friendly value.
	- `guesses` - an estimate of how many guesses an attacker would need to crack the password. This value is calculated based on the patterns detected in the password.
	- `guesses_log10` - the base-10 logarithm of the guesses value. This can be useful for comparisons.
	- `crack_times_seconds` - the estimated time (in seconds) required to crack the password under various attack scenarios.
		- `online_throttling_per_hour` - a scenario with online attacks where rate limiting is enforced.
		- `online_no_throttling_per_second` - a scenario with online attacks where there's no rate limiting.
		- `offline_slow_hashing_per_second` - a scenario with slower, more secure hashing algorithms.
		- `offline_fast_hashing_per_second` - a scenario with fast, optimized attacks using powerful hardware.
	- `crack_times_display` - the same as crack\_times\_seconds, but presented in a more user-friendly format.
	- `feedback` - an object providing suggestions and warnings to improve the password.
		- `warning` - a string with a general warning about the password's weaknesses.
		- `suggestions` - a list of suggestions to make the password stronger.