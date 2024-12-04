# How to check a file
## Look at it sternly and ask nicely if its the right file

`Else` use gpg and get-filehash on windows

Calculate the SHA256 of the files in question
---------------------------------------------
Get-FileHash filename.example
```powershell
npr@DESKTOP-ROME:~$ Get-FileHash C:\Users\NPR\Documents\ISOs\ubunutu.iso
Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          C2E6F4DC37AC944E2ED507F87C6188DD4D3179BF4A3F9E110D3C88D1F3294BDC       C:\Users\npr\Documents...
```

CertUtil -hashfile filename.example
```powershell
npr@DESKTOP-ROME:~$ certutil -hashfile C:\Users\NPR\Documents\ISOs\ubunutu.iso SHA256
SHA256 hash of .\ubuntu.iso:
c2e6f4dc37ac944e2ed507f87c6188dd4d3179bf4a3f9e110d3c88d1f3294bdc
CertUtil: -hashfile command completed successfully.
```

Does it match the SHA256 from the source? Ye? Great move on

gpg
===
GNU Privacy Guard (GnuPG) is a free-software replacement for Symantec's cryptographic software suite PGP.
A way to use a public key to authenticate the checksum file

##Windows
Its not likely that the key will be present.
When public key is not downloaded
```powershell
npr@DESKTOP-ROME:~$ gpg --keyid-format long --verify SHA256SUMS.gpg SHA256SUMS
gpg: Signature made Thu Apr  5 22:19:36 2018 EDT
                    using DSA key ID 46181433FBB75451
gpg: Can't check signature: No public key
gpg: Signature made Thu Apr  5 22:19:36 2018 EDT
                    using RSA key ID D94AA3F0EFE21092
gpg: Can't check signature: No public key
```
To download the public key
```powershell
npr@DESKTOP-ROME:~$ gpg --keyid-format long --keyserver hkp://keyserver.ubuntu.com --recv-keys 0x46181433FBB75451 0xD94AA3F0EFE21092
gpg: requesting key 46181433FBB75451 from hkp server keyserver.ubuntu.com
gpg: requesting key D94AA3F0EFE21092 from hkp server keyserver.ubuntu.com
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key 46181433FBB75451: public key "Ubuntu CD Image Automatic Signing Key <cdimage@ubuntu.com>" imported
gpg: key D94AA3F0EFE21092: public key "Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 2
gpg:               imported: 2  (RSA: 1)
```

When the public key is downloaded
```powershell
npr@DESKTOP-ROME:~$ gpg --verify .\SHA256SUM.gpg .\SHA256.txt
gpg: Signature made 10/10/24 06:53:08 Eastern Daylight Time
gpg:                using RSA key 84393810D22F7B37423F0EFE21092
gpg: Good signature from "Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092
```

