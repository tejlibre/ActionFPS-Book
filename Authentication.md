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
reloading : client::u pointer changes. store uid rather pointer to struct user ? (and call usermanager::find(cl::uid) everytime ?) probably better yes, *IF* usermanager::find is fast enough.
can also recompute pointers. and kick players with no pointer matching after reload.

## Authentication process

 * Use public/private key authentication, DSA method
  * user connects to server
  * server answers random challenge along with SV_SERVINFO (challenge = sha1(openssl crypto safe RAND_bytes + serverid))
  * user answers ID and signature(priv_key, challenge) along with SV_CONNECT
  * server verifies(pub_key, signature, challenge)

This does not require authentication-specific "SV" messages, which is more natural because authentication is required.
The user gets disconnected if the signature verification fails or if the corresponding user is banned.

(see https://www.openssl.org/docs/manmaster/crypto/DSA_sign.html or http://au2.php.net/manual/en/function.openssl-sign.php, better documented also https://www.openssl.org/docs/manmaster/crypto/pem.html)

REQUIRES OPENSSL >= 1.0.0e https://blog.opentrust.com/?p=174

## Log messages

[ip] -> [user:ip]

compute on SV_CONNECT'ion. client::identity = format("%s:%s", cl->u->id, cl->hostname)

## Database

config/users.gz

config/group.gz

Example + php generation script : https://gist.github.com/lucasgautheron/17464926f5ea0ddc555d
