# Make a CRT File

## Create a .key File (RSA Key)

> **~/.ssh $ openssl genrsa -aes256 -out cloud9-terraform.key 2048** \
> **_Remember Pass Pharase !!!_**
```
Generating RSA private key, 2048 bit long modulus (2 primes)
....+++++
.....................+++++
e is 65537 (0x010001)
Enter pass phrase for cloud9-terraform.key:
Verifying - Enter pass phrase for cloud9-terraform.key:
```

> **~/.ssh $ ls -l**
```
-rw------- 1 ubuntu ubuntu    1766 Aug 10 15:48 cloud9-terraform.key
```

## Create a .conf File

> **~/.ssh $ vi cloud9-terraform.conf**
```
[ req ]
default_bits                    = 2048
default_md                      = sha1
default_keyfile                 = cloud9-terraform.key
distinguished_name              = req_distinguished_name
extensions                      = v3_ca
req_extensions                  = v3_ca
 
[ v3_ca ]
basicConstraints                = critical, CA:TRUE, pathlen:0
subjectKeyIdentifier            = hash
##authorityKeyIdentifier        = keyid:always, issuer:always
keyUsage                        = keyCertSign, cRLSign
nsCertType                      = sslCA, emailCA, objCA
[req_distinguished_name ]
countryName                     = Country Name (2 letter code)
countryName_default             = KR
countryName_min                 = 2
countryName_max                 = 2

# 회사명 입력
organizationName                = Organization Name (eg, company)
organizationName_default        = Organization
 
# 부서 입력
#organizationalUnitName         = Organizational Unit Name (eg, section)
#organizationalUnitName_default = Organizational Unit
 
# SSL 서비스할 domain 명 입력
commonName                      = Common Name (eg, your name or your server's hostname)
commonName_default              = Common Self Signed CA
commonName_max                  = 64 
```

> **~/.ssh $ ls -l**
```
-rw-rw-r-- 1 ubuntu ubuntu    1262 Aug 10 16:02 cloud9-terraform.conf
-rw------- 1 ubuntu ubuntu    1766 Aug 10 15:48 cloud9-terraform.key
```

## Create a .CSR File

> **~/.ssh $ openssl req -new -key cloud9-terraform.key -out cloud9-terraform.csr -config cloud9-terraform.conf** \
> **_Remember Pass Pharase !!!_**
```
Enter pass phrase for cloud9-terraform.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [KR]:
Organization Name (eg, company) [Organization]:
Common Name (eg, your name or your servers hostname) [Common Self Signed CA]:
```

> **~/.ssh $ ls -l**
```
-rw-rw-r-- 1 ubuntu ubuntu    1262 Aug 10 16:02 cloud9-terraform.conf
-rw-rw-r-- 1 ubuntu ubuntu    1090 Aug 10 16:07 cloud9-terraform.csr
-rw------- 1 ubuntu ubuntu    1766 Aug 10 15:48 cloud9-terraform.key
```

## Create a .CRT File

>  <pre>~/.ssh $ openssl x509 -req -days 3650 -extensions v3_ca -set_serial 1 \
> -in cloud9-terraform.csr \
> -signkey cloud9-terraform.key \
> -out cloud9-terraform.crt \
> -extfile cloud9-terraform.conf </pre>
> **_Remember Pass Pharase !!!_**

```
Signature ok
subject=C = KR, O = Organization, CN = Common Self Signed CA
Getting Private key
Enter pass phrase for cloud9-terraform.key:
```

> **~/.ssh $ ls -l**
```
-rw-rw-r-- 1 ubuntu ubuntu    1262 Aug 10 16:02 cloud9-terraform.conf
-rw-rw-r-- 1 ubuntu ubuntu    1220 Aug 10 16:33 cloud9-terraform.crt
-rw-rw-r-- 1 ubuntu ubuntu    1090 Aug 10 16:07 cloud9-terraform.csr
-rw------- 1 ubuntu ubuntu    1766 Aug 10 15:48 cloud9-terraform.key
```
