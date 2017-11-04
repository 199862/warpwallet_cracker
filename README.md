# warpwallet_cracker
A brute-force cracker in Go for the WarpWallet Challenge 2: https://keybase.io/warp/

# Usage

```
$ go get ./...
---
$ go build warpwallet_cracker.go
---
$ ./run.sh 
Using address "1MkupVKiCik9iyfnLrJoZLx9RH4rkF3hnA" and salt "a@b.c"
Tried 4 passphrases in 2.269448485s [last passphrase: 2zZM3L1C]
```

# Performance
This script has been optimized for speed. On a MacBook Pro this achieves ~1.1 hash/sec/core (with hyperthreading enabled). At this hashrate it is [not feasible](https://www.wolframalpha.com/input/?i=(62%5E8+%2F+1.1)+seconds+to+years) to enumerate the entire keyspace of 62^8 hashes.

For example: if you run this script for a year on a quad-core Macbook, there is a [1 in ~37M](https://www.wolframalpha.com/input/?i=62%5E8+%2F+(3600+*+365+*+1.1+*+4)) chance of your RNG gifting you 20 BTC.

Ideas for further improvements:
- Explore using SSE2 for faster scrypt
- Use all cores of the CPU
- Build a bloom filter containing past attempts (is a bloom filter lookup faster than a hash attempt?)
- Figure out a way to share the bloom filter with other crackers
- Deterministic key generation for better search space partitioning (instead of seeding rand with a unix timestamp)

# How-to
Build:

`go build warpwallet_cracker.go`

Params:

`./warpwallet_cracker [Address] [Salt - optional]`

Run (Windows):

`run.bat`

Run (*nix):

`./run.sh`

Run unit tests and benchmark:

`go test -bench=.`

Windows 10 installation

I share my installation steps for windows
windows 10 pro
-	Install Go and follow their instructions to configure it (e.g.GOBIN, GOPATH, GOROOT)
-	Install mingw64 (read their instructions)
        o	https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/mingw-w64-v5.0.3.zip

-	Install the warpwallet_cracker.go
        o	You can do it automatically with command go get but I did it manually
        o	Download zip from
        	Github.com/ctz/go-fastpbkdf2
        	Github.com/nachowski/warpwallet_cracker
        	Github.com/vsergeev/btckey
         o	Create the following directories under C:\Users\MYUSER\go
         o	 \src\github.com\ctz\go-fastpbkdf2
         o	 \src\github.com\nachowski\warpwallet_cracker
         o	 \src\github.com\vsergeev\btckey
          Unzip each, under it’s correspondent directory
From the mingw64 console run 
Cd go
go build src/github.com/nachowski/warpwallet_cracker.go

known issues:
openssl/sha.h   look for your openssl installation and copy directory \openssl containing sha.h 
paste it so it must be like this
C:\Users\MYUSER\go\src\github.com\ctz\go-fastpbkdf2\openssl

COLLECT2.EXE Delete it or rename it

-lcrypto look for libcrypto.a in your pc and paste it as follows
C:\Program Files\mingw-w64\x86_64-7.2.0-posix-seh-rt_v5-rev0\mingw64\lib\gcc\x86_64-w64-mingw32\7.2.0
