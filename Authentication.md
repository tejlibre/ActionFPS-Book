## User database

Update : ssh from actionfps
Format : 


User {

    * id
    * nicknames
    * groups
    * admins
    * pubkey (2048,256)

}

### In Memory
Vector for now? http://www.cplusplus.com/reference/unordered_map/unordered_map/ ? c++11 necessary? Or just go for http://www.cplusplus.com/reference/map/map/

### Storage

users.tsvgz, i.e. gzipped tab separated values

## Authentication process

 * Use public/private key authentication, DSA method
  * user connects to server
  * server answers random challenge
  * user answers ID and signature(priv_key, challenge + server_id)
  * server verifies(pub_key, signature, challenge + server_id)

(see https://www.openssl.org/docs/manmaster/crypto/DSA_sign.html)

## Log messages

[ip] -> [user:ip]