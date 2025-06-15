# Env Portal

A minimal workflow for encrypting .env files and restoring them via QR code using Dagger

## 🧰 Install Dagger CLI

[Installation | Dagger](https://docs.dagger.io/install/)

```zsh
brew install dagger/tap/dagger
```

## 🔐 Step 1: Encrypt .env and Generate Encrypted Dagger Command as QR Code

> Encrypt a .env file using dotenvx, then encrypt the Dagger command used for decryption, and generate a QR code from it

```zsh
export SECRET='super-secure-password'&&\
dagger core container from --address=oven/bun:slim\
  with-exec --args=sh,-c,'apt-get update&&apt-get install -y --no-install-recommends curl ca-certificates&&curl -s https://pkgx.sh|sh'\
  with-exec --args=pkgm,install,dotenvx,qrencode\
  with-directory --path=. --directory=.\
  with-exec --args=dotenvx,encrypt,.env\
  with-exec --args=sh,-c,'cat .env .env.keys>.env.bundle'\
  with-env-variable --name=SECRET --value=$SECRET\
  with-exec --args=sh,-c,'echo $SECRET>.secret'\
  with-exec --args=bun,-e,"eval('(async()=>{const src=Buffer.from(await Bun.file(\'.env.bundle\').text());const key_0=await crypto.subtle.importKey(\'raw\'\x2CBuffer.from(Bun.env.SECRET)\x2C{name:\'PBKDF2\'}\x2Cfalse\x2C[\'deriveKey\']);const salt=crypto.getRandomValues(new Uint8Array(16));const key_1=await crypto.subtle.deriveKey({hash:\'SHA-256\'\x2Citerations:100_000\x2Cname:\'PBKDF2\'\x2Csalt}\x2Ckey_0\x2C{length:256\x2Cname:\'AES-GCM\'}\x2Ctrue\x2C[\'encrypt\'\x2C\'decrypt\']);const iv=crypto.getRandomValues(new Uint8Array(12));const cipher=await crypto.subtle.encrypt({iv\x2Cname:\'AES-GCM\'}\x2Ckey_1\x2Csrc);const data=new Uint8Array(salt.length+iv.length+cipher.byteLength);data.set(salt\x2C0);data.set(iv\x2Csalt.length);data.set(new Uint8Array(cipher)\x2Csalt.length+iv.length);const enc=Buffer.from(data).toBase64();await Bun.write(\'.env.bundle.enc\'\x2Cenc)})()')"\
  with-exec --args=sh,-c,'curl -s -X POST --data-binary @.env.bundle.enc https://paste.rs -o .url'\
  with-exec --args=bun,-e,"eval('(async()=>{const dagger=\x60dagger core container from --address=oven/bun:slim\\\\\\\\\\\n  with-exec --args=sh\x2C-c\x2C\x27apt-get update&&apt-get install -y --no-install-recommends curl ca-certificates&&curl -s https://pkgx.sh|sh\x27\\\\\\\\\\\n  with-exec --args=pkgm\x2Cinstall\x2Cdotenvx\\\\\\\\\\\n  with-exec --args=sh\x2C-c\x2C\x27curl -s \${await Bun.file(\'.url\').text()}>.env.bundle.enc\x27\\\\\\\\\\\n  with-exec --args=bun\x2C-e\x2C\x22eval(\x27(async()=>{const src=Buffer.from(await Bun.file(\\\\\\\\\\\x27.env.bundle.enc\\\\\\\\\\\x27).text()\\\\\\\\\\\${\x22x2C\x22}\\\\\\\\\\\\x27base64\\\\\\\\\\\\x27);const [salt\\\\\\\\\\\${\x22x2C\x22}iv\\\\\\\\\\\${\x22x2C\x22}cipher]=[src.subarray(0\\\\\\\\\\\${\x22x2C\x22}16)\\\\\\\\\\\${\x22x2C\x22}src.subarray(16\\\\\\\\\\\${\x22x2C\x22}28)\\\\\\\\\\\${\x22x2C\x22}src.subarray(28)];const secret=Buffer.from(\\\\\\\\\\\\x27\${(await Bun.file(\x27.secret\x27).text()).trim()}\\\\\\\\\\\\x27);const key_0=await crypto.subtle.importKey(\\\\\\\\\\\\x27raw\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}secret\\\\\\\\\\\${\x22x2C\x22}{name:\\\\\\\\\\\\x27PBKDF2\\\\\\\\\\\\x27}\\\\\\\\\\\${\x22x2C\x22}false\\\\\\\\\\\${\x22x2C\x22}[\\\\\\\\\\\\x27deriveKey\\\\\\\\\\\\x27]);const key_1=await crypto.subtle.deriveKey({hash:\\\\\\\\\\\\x27SHA-256\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}iterations:100_000\\\\\\\\\\\${\x22x2C\x22}name:\\\\\\\\\\\\x27PBKDF2\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}salt}\\\\\\\\\\\${\x22x2C\x22}key_0\\\\\\\\\\\${\x22x2C\x22}{length:256\\\\\\\\\\\${\x22x2C\x22}name:\\\\\\\\\\\\x27AES-GCM\\\\\\\\\\\\x27}\\\\\\\\\\\${\x22x2C\x22}true\\\\\\\\\\\${\x22x2C\x22}[\\\\\\\\\\\\x27encrypt\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}\\\\\\\\\\\\x27decrypt\\\\\\\\\\\\x27]);const dec=Buffer.from(await crypto.subtle.decrypt({iv\\\\\\\\\\\${\x22x2C\x22}name:\\\\\\\\\\\\x27AES-GCM\\\\\\\\\\\\x27}\\\\\\\\\\\${\x22x2C\x22}key_1\\\\\\\\\\\${\x22x2C\x22}cipher)).toString();await Bun.write(\\\\\\\\\\\\x27.env.bundle\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}dec)})()\x27)\x22\\\\\\\\\\\n  with-exec --args=bun\x2C-e\x2C\x22eval(\x27(async()=>{const f=await Bun.file(\\\\\\\\\\\\x27.env.bundle\\\\\\\\\\\\x27).text();const m=\\\\\\\\\\\\x27#/------------------\\\\\\\\\\\\x21DOTENV_PRIVATE_KEYS\\\\\\\\\\\\x27;const i=f.indexOf(m);await Bun.write(\\\\\\\\\\\\x27.env\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}f.slice(0\\\\\\\\\\\${\x22x2C\x22}i));await Bun.write(\\\\\\\\\\\\x27.env.keys\\\\\\\\\\\\x27\\\\\\\\\\\${\x22x2C\x22}f.slice(i))})()\x27)\x22\\\\\\\\\\\n  with-exec --args=dotenvx\x2Cdecrypt\x2C.env\\\\\\\\\\\n  with-exec --args=cat\x2C.env\\\\\\\\\\\n  stdout\x60;await Bun.write(\x27.dagger\x27\x2Cdagger)})()')"\
  with-exec --args=bun,-e,"eval('(async()=>{const src=Buffer.from(await Bun.file(\'\.dagger\').text());const key_0=await crypto.subtle.importKey(\'raw\'\x2CBuffer.from(Bun.env.SECRET)\x2C{name:\'PBKDF2\'}\x2Cfalse\x2C[\'deriveKey\']);const salt=crypto.getRandomValues(new Uint8Array(16));const key_1=await crypto.subtle.deriveKey({hash:\'SHA-256\'\x2Citerations:100_000\x2Cname:\'PBKDF2\'\x2Csalt}\x2Ckey_0\x2C{length:256\x2Cname:\'AES-GCM\'}\x2Ctrue\x2C[\'encrypt\'\x2C\'decrypt\']);const iv=crypto.getRandomValues(new Uint8Array(12));const cipher=await crypto.subtle.encrypt({iv\x2Cname:\'AES-GCM\'}\x2Ckey_1\x2Csrc);const data=new Uint8Array(salt.length+iv.length+cipher.byteLength);data.set(salt\x2C0);data.set(iv\x2Csalt.length);data.set(new Uint8Array(cipher)\x2Csalt.length+iv.length);const enc=Buffer.from(data).toBase64();await Bun.write(\'.dagger.enc\'\x2Cenc)})()')"\
  with-exec --args=sh,-c,'cat .dagger.enc|qrencode -o qrcode.png'\
  file --path=/home/bun/app/qrcode.png\
  export --path=qrcode.png
```

## 🔓 Step 2: Scan QR Code and Restore Access to .env

> Decrypt the Dagger command from the QR code
> 
> Running the recovered command will give you access to the contents of the .env file

```zsh
export SECRET='super-secure-password'&&\
dagger core container from --address=oven/bun:slim\
  with-exec --args=sh,-c,'apt-get update&&apt-get install -y --no-install-recommends zbar-tools'\
  with-directory --path=. --directory=.\
  with-exec --args=sh,-c,'zbarimg qrcode.png|cut -d':' -f2->.dagger.enc'\
  with-env-variable --name=SECRET --value=$SECRET\
  with-exec --args=bun,-e,"eval('(async()=>{const src=Buffer.from(await Bun.file(\'.dagger.enc\').text()\x2C\'base64\');const [salt\x2Civ\x2Ccipher]=[src.subarray(0\x2C16)\x2Csrc.subarray(16\x2C28)\x2Csrc.subarray(28)];const secret=Buffer.from(Bun.env.SECRET);const key_0=await crypto.subtle.importKey(\'raw\'\x2Csecret\x2C{name:\'PBKDF2\'}\x2Cfalse\x2C[\'deriveKey\']);const key_1=await crypto.subtle.deriveKey({hash:\'SHA-256\'\x2Citerations:100_000\x2Cname:\'PBKDF2\'\x2Csalt}\x2Ckey_0\x2C{length:256\x2Cname:\'AES-GCM\'}\x2Ctrue\x2C[\'encrypt\'\x2C\'decrypt\']);const dec=Buffer.from(await crypto.subtle.decrypt({iv\x2Cname:\'AES-GCM\'}\x2Ckey_1\x2Ccipher)).toString();await Bun.write(\'.dagger\'\x2Cdec)})()')"\
  with-exec --args=cat,.dagger\
  stdout
```
