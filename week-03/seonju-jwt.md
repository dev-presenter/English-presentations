# JWT

## What is JSON Web Token?

- JWT is a compact and self-contained way for securely transmitting information in a JSON object
- JWT is digitally signed
- JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

## When should you use JSON Web Tokens?

- Authorization
    - Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token.
- Information Exchange
    - JWTs can verify who the sender of the data is.
    - you can also verify that the content hasn’t been tampered with by calculating the signature based on the header and the payload.

## Structure

In its compact form, JSON Web Tokens consist of three parts separated by dots (`.`), which are:

- Header
- Payload
- Signature

Therefore, a JWT typically looks like the following:

```
xxxxx.yyyyy.zzzzz
```

### Header

For example:

```json
{
  "typ": "JWT", 
  "alg": "HS256"
}
```

- typ : the type of the token
- alg : the signing algorithm such as HMAC SHA256 or RSA

### Payload

For example:

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

The payload contains the claims that make statements about an entity and additional data.

- Registered claims: These are a set of predefined claims that provide a set of useful and interoperable claims.
    - Some of them are: **iss**(issuer), **exp**(expiration time), **sub**(subject), **aud**(audience), and others.
- Public claims: These can be defined to avoid collisions in the IANA JSON Web Token Registry or a URI that contains a collision resistant namespace.
- Private claims: These are the custom claims created to share information and neither registered or public claims.

### Signature

To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For example, If you want to use the HMAC SHA256 algorithm, the signature will be created in the following way:

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

### Output

- This is consist of three Base64-URL strings separated by dots

  → It can be easily passed in HTML and HTTP environments

  → It is more compact than XML-based standards


## Precautions

- When you use JSON for authentication, there are precautions to be taken.
    - You should not keep tokens longer than required because tokens are credentials.
    - Whenever the user wants to access a protected route or resource, the user agent should send the JWT typically in the **Authorization** header using the **Bearer** schema.

        ```json
        Authorization: Bearer <token>
        ```

    - Because servers don't accept more than 8 KB in headers, you should try to prevent them from getting too big.

## Benefits

Let's talk about the benefits of JSON Web Tokens (JWT) when compared to Simple Web Tokens (SWT) and Security Assertion Markup Language Tokens (SAML).

- Compact:
    - JSON is less verbose than XML, when it is encoded its size is also smaller, making JWT more compact than SAML.
- Security:
    - SWT can only be symmetrically signed by a shared secret using the HMAC algorithm.
    - JWT and SAML tokens can use a public/private key pair in the form.
- Easy:
    - JSON parsers map directly to objects, but XML doesn't have a natural document-to-object mapping.