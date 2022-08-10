# make_crt_file

## Create .CSR file

**~/.ssh $ ls -l**
```
-rw-------  1 ubuntu ubuntu 3243 Aug  5 17:01 id_rsa
```

**~/.ssh $ openssl req -new -key id_rsa -out request.csr**
```
Can't load /home/ubuntu/.rnd into RNG
139711512363456:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/home/ubuntu/.rnd
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

**~/.ssh $ ls -l**
```
-rw-------  1 ubuntu ubuntu 3243 Aug  5 17:01 id_rsa
-rw-rw-r--  1 ubuntu ubuntu 1651 Aug 10 10:39 request.csr
```

## Create a .CRT File

**~/.ssh $ openssl x509 -req -days 365 -in request.csr -signkey id_rsa -out certificate.crt**
```
Signature ok
subject=C = AU, ST = Some-State, O = Internet Widgits Pty Ltd
openssl req -new -key id_rsa -out request.csr
Getting Private key
```

**~/.ssh $ ls -l**
```
-rw-rw-r--  1 ubuntu ubuntu 1818 Aug 10 10:39 certificate.crt
-rw-------  1 ubuntu ubuntu 3243 Aug  5 17:01 id_rsa
-rw-rw-r--  1 ubuntu ubuntu 1651 Aug 10 10:39 request.csr
```
