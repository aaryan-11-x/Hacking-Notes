JWT vulnerabilities arise due to flawed JWT handling within the application itself. JWT attacks involve a user sending modified JWTs to the server in order to achieve a malicious goal. Typically, this goal is to bypass authentication & access controls by *impersonating another user who has already been authenticated*.

![[Pasted image 20231001225300.png]]

- `RS256`: RSA encryption used with SHA-256
- `iat`: Initiation time
- `exp`: Expiry time
- `SIG`: Hash(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)

---
# Algorithm Confusion Attack
**Algorithm confusion attacks** (also known as key confusion attacks) occur when an attacker is able to force the server to verify the signature of a JSON web token (JWT) using a *different algorithm* than is intended by the website's developers. If this case isn't handled properly, this may enable attackers to forge valid JWTs containing arbitrary values without needing to know the server's secret signing key.

## How do Algorithm Confusion vulnerabilities arise?
```C
function verify(token, secretOrPublicKey){
    algorithm = token.getAlgHeader();
    if(algorithm == "RS256"){
        // Use the provided key as an RSA public key
    } else if (algorithm == "HS256"){
        // Use the provided key as an HMAC secret key
    }
}
```
Problems arise when website developers who subsequently use this method **assume** that it will exclusively handle JWTs signed using an asymmetric algorithm like RS256. Due to this flawed assumption, they may always pass a fixed public key to the method as follows:

```C
publicKey = <public-key-of-server>;
token = request.getCookie("session");
verify(token, publicKey);
```

In this case, if the server receives a token signed using a symmetric algorithm like HS256, the library's generic verify() method will treat the public key as an HMAC secret. This means that an attacker could sign the token using HS256 and the public key, and the server will use the same public key to verify the signature.


## Performing an algorithm confusion attack
An algorithm confusion attack generally involves the following high-level steps:
1) Obtain the server's public key
2) Convert the public key to a suitable format
3) Create a malicious JWT with a modified payload and the `alg` header set to HS256.
4) Sign the token with HS256, using the public key as the secret.

###### Step 1 - Obtain the server's public key
Servers sometimes expose their public keys as JSON Web Key (JWK) objects via a standard endpoint mapped to /jwks.json or /.well-known/jwks.json, for example. These may be stored in an array of JWKs called keys. This is known as a JWK Set.
```C
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "75d0ef47-af89-47a9-9061-7c02a610d5ab",
            "n": "o-yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw-fhvsWQ"
        },
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "d8fDFo-fS9-faS14a9-ASf99sa-7c1Ad5abA",
            "n": "fc3f-yy1wpYmffgXBxhAUJzHql79gNNQ_cb33HocCuJolwDqmk6GPM4Y_qTVX67WhsN3JvaFYw-dfg6DH-asAScw"
        }
    ]
}
```
Even if the key isn't exposed publicly, you may be able to extract it from a pair of existing JWTs.
%% The key part will start with { "kty": "RSA", "e": "AQAB", "use": "sig", "kid": ergtfio" } %%
%% Remove the trailing brackets at the end %%
###### Step 2 - Convert the public key to a suitable format
Although the server may expose their public key in `JWK` format, when verifying the signature of a token, it will use its own copy of the key from its local filesystem or database. This may be stored in a different format.

In order for the attack to work, the version of the key that you use to sign the JWT must be identical to the server's local copy. In addition to being in the same format, every single byte must match, including any non-printing characters.

For the purpose of this example, let's assume that we need the key in `X.509 PEM` format. You can convert a JWK to a PEM using the JWT Editor extension in Burp as follows:

- With the extension loaded, in Burp's main tab bar, go to the JWT Editor Keys tab.
- Click New RSA Key. In the dialog, paste the JWK that you obtained earlier.
- Select the PEM radio button and copy the resulting PEM key.
- Go to the Decoder tab and Base64-encode the PEM.
- Go back to the JWT Editor Keys tab and click New Symmetric Key.
- In the dialog, click Generate to generate a new key in JWK format.
- Replace the generated value for the **k** parameter with the Base64-encoded PEM key that you just copied.
- Save the key.

###### Step 3 - Modify your JWT
Once you have the public key in a suitable format, you can modify the JWT however you like. Just make sure that the `alg` header is set to HS256.

###### Step 4 - Sign the JWT using the public key
- Head to the request & go to JSON Web Token section
- Change the necessary parameters (Like role: user --> admin)
![[Pasted image 20231002090618.png]]
- Sign the token using the HS256 algorithm with the RSA public key as the secret.