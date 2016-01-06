To use this repository, create a file named /etc/apt/sources.list.d/alternc-nightly.list as follow :

## Debian 8 (Jessie)

```
deb http://stable-3-3.nightly.alternc.org/ latest/
```
The repository and the packages are signed by the pgp key of AlternC nightly build user:

```
wget http://stable-3-3.nightly.alternc.org/nightly.key -O - | apt-key add -
```

## Debian 7 (Wheezy)

```
deb http://stable-3-2.nightly.alternc.org/ latest/
```

The repository and the packages are signed by the pgp key of AlternC nightly build user:

```
wget http://stable-3-2.nightly.alternc.org/nightly.key -O - | apt-key add -
```

## Debian 6 (Squeeze)

```
deb http://stable-3-1.nightly.alternc.org/ latest/
```
The repository and the packages are signed by the pgp key of AlternC nightly build user:

```
wget http://stable-3-1.nightly.alternc.org/nightly.key -O - | apt-key add -
```
## Default login/password

* login: `admin`
* password: `admin`
