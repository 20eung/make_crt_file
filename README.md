# Make a CRT File

## Windows User: Install OpenSSL-Win64
```cmd
C:\>choco install openssl
Chocolatey v2.2.2
Installing the following packages:
openssl
By installing, you accept licenses for the packages.
Progress: Downloading openssl 3.2.0... 100%

openssl v3.2.0 [Approved]
openssl package files install completed. Performing other installation steps.
The package openssl wants to run 'chocolateyinstall.ps1'.
Note: If you don't run this script, the installation will fail.
Note: To confirm automatically next time, use '-y' or consider:
choco feature enable -n allowGlobalConfirmation
Do you want to run the script?([Y]es/[A]ll - yes to all/[N]o/[P]rint): A

Downloading openssl 64 bit
  from 'https://slproweb.com/download/Win64OpenSSL-3_2_0.exe'
Progress: 100% - Completed download of D:\Temp\chocolatey\openssl\3.2.0\Win64OpenSSL-3_2_0.exe (199.92 MB).
Download of Win64OpenSSL-3_2_0.exe (199.92 MB) completed.
Hashes match.
Installing openssl...
openssl has been installed.
WARNING: No registry key found based on  'OpenSSL-Win'
PATH environment variable does not have C:\Program Files\OpenSSL-Win64\bin in it. Adding...
WARNING: OPENSSL_CONF has been set to C:\Program Files\OpenSSL-Win64\bin\openssl.cfg
  openssl can be automatically uninstalled.
Environment Vars (like PATH) have changed. Close/reopen your shell to
 see the changes (or in powershell/cmd.exe just type `refreshenv`).
 The install of openssl was successful.
  Software installed to 'C:\Program Files\OpenSSL-Win64\'

Chocolatey installed 1/1 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).

C:\>refreshenv
Refreshing environment variables from registry for cmd.exe. Please wait...Finished..
```

## Create a .key File (RSA Key)

> **~/.ssh $ openssl genrsa -aes256 -out cloud9-terraform.key 2048** \
> **_Remember Pass Pharase !!!_** \
> **-aes256을 생략하면 패스워드없이 키파일 생성 가능**
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
default_md                      = sha256
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

stateOrProvinceName             = State or Province Name (full name)
stateOrProvinceName_default     = State

localityName                    = Locality Name (eg, city)
localityName_default            = City

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
