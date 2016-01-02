## User database

Update : ssh from actionfps
Format :

## Authentication process

 * Use public/private key authentication, DSA method
  * user connects to server
  * server answers random challenge
  * user answers ID and signature(priv_key, challenge + server_id)
  * server verifies(pub_key, signature, challenge + server_id)

(see https://www.openssl.org/docs/manmaster/crypto/DSA_sign.html)

## Log messages

[ip] -> [user:ip]