# Hacking JWT Tokens: Understanding and Exploiting Weaknesses
JSON Web Tokens (JWT) are widely used for authentication in web applications. They encode user data and are signed to ensure integrity. However, improperly implemented JWTs can be exploited by attackers. In this post, we’ll explore JWT structure, common vulnerabilities, and practical methods for ethical testing and CTF challenges.

## Table of Contents
- [JWT Structure](#jwt-structure)
- [Common Vulnerabilities](#common-vulnerabilities)
- [Tools for Testing JWTs](#tools-for-testing-jwts)
- [Practical Exploitation Examples](#practical-exploitation-examples)
- [Tips and Best Practices](#tips-and-best-practices)

## JWT Structure

A JWT has three parts separated by dots:

```

header.payload.signature

````

- **Header**: Contains the algorithm and token type.  
- **Payload**: Contains claims, e.g., user ID or roles.  
- **Signature**: Ensures the token hasn’t been tampered with.

Example decoded JWT:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
````

Payload:

```json
{
  "user": "alice",
  "role": "user"
}
```

## Common Vulnerabilities

1. **None Algorithm Vulnerability**
   Some servers accept `alg: none`, allowing unsigned tokens.

2. **Weak Secret Keys**
   Short or predictable secrets can be brute-forced.

3. **Algorithm Confusion**
   Switching algorithms (e.g., `RS256` → `HS256`) can allow attackers to forge tokens.

4. **Token Tampering**
   Modifying claims without re-signing if the server doesn’t properly validate.

## Tools for Testing JWTs

* [jwt.io](https://jwt.io) — decode and verify tokens
* [jwt-tool](https://github.com/ticarpi/jwt_tool) — brute-force or manipulate tokens
* Python scripts with `PyJWT` or `jwt` libraries for custom testing

## Practical Exploitation Examples

1. **Decoding a JWT**

```
echo "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." | jwt decode
```

2. **Testing for None Algorithm Vulnerability**

* Modify the header:

```json
{
  "alg": "none",
  "typ": "JWT"
}
```

* Remove the signature and resend the token.

3. **Brute-Forcing Weak Secrets**

```
python3 jwt_tool.py -t <token> -w wordlist.txt
```

4. **Changing Roles**

* Decode token
* Change `"role": "user"` → `"role": "admin"`
* Re-sign with the known secret (if possible)
* Resend token to gain elevated privileges

## Tips and Best Practices

* Only test JWTs on systems you are authorized to assess.
* Start by decoding and analyzing tokens before attempting modification.
* Use long, random secrets with secure algorithms (HS256 or RS256).
* Validate tokens correctly on the server side and reject unknown algorithms.
* Combine JWT testing with other authentication testing tools in CTF challenges.
