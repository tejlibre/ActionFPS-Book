1. Auth is now automatic, runs in /register-play/ and /play/ endpoints.
2. User key-pair is generated and server in the above pages, for every user.
3. A file is generated with the key pairs for every user.
4. On file update, the file is sent to woop.ac and a combiner script runs
5. Combiner script takes in admins from the Management panel (one column pls only) and merges it with Clans JSON, Players JSON and the public keys of users.