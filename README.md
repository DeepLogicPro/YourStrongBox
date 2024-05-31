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

 - **key_size** (int: 1024/2048/3072/7680) - Key size (default=2048).

**Response:**

```
{
  "private_key": "-----BEGIN PRIVATE KEY-----...",
  "public_key": "-----BEGIN PUBLIC KEY-----..."
}
```

