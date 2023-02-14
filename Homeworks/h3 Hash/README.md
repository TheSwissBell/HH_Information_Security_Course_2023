# Homework H3 Hash
See instructions: https://terokarvinen.com/2023/information-security-2023/



## x) Read and summarize € Schneier 2015: Applied Cryptography: 2.3 One-Way Functions and 2.4 One-Way Hash Functions.
[Full article on O'Reilly](https://learning.oreilly.com/library/view/applied-cryptography-protocols/9781119096726/10_chap02.html#chap02-sec003 "Hypertext link")

### 2.3 ONE-WAY FUNCTIONS
+ Central notion in public-key  ryptography
+ It's a fonction that is isue to coumpute in one direction but very difficult in the reverse one
    + Posible but would take million of year to compute even with all the computers of the world
    + It's gonna change when Quantum computing.
+ Not a protocol but used in many of them in cryptographic systems.
+ From a mathematical point of view. Neither the existence of one-way functions nor the feasibility of their construction are proven
+ A metaphor is like when you brake a plate. It's fast to break it but take a lot of time to repaire it and put back togehter all the pieces in their exact place.
![broken plate example](pictures/broken_plate.jpg)

Photo by CHUTTERSNAP on [Unsplash](https://unsplash.com/photos/u3ZDnIMCfIs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

+ Often used for password storage, digital signatures, and key derivation.


+ Trapdoor one-way function exist. It's a special type. If you know _the secret_ it's easy to compute in the reverese direction. Like for example a watch. It's easy to remove and deasamble all the small pieces but very hard to put them back. Except if you have the assembly instructions (and watchmaking skills).  
![broken plate example](pictures/watch_pieces.jpg)



### 2.4 ONE-WAY HASH FUNCTIONS

+ Common cryptographic hash functions are SHA-2 and SHA-3, as well as modular exponentiation.
+ There are some attacks on one-way functions, including brute-force attacks, birthday attacks, and meet-in-the-middle attacks.
+ However, MD5 and SHA-1 are no longer considered secure for most applications due to their vulnerabilities to collision attacks.
    + SHA-2 is a family of hash functions that includes SHA-224, SHA-256, SHA-384, and SHA-512. It is currently considered secure for most applications.
+ Cryptographers are continually looking for new and more powerful one-way functions, as they are essential to the security of modern cryptography.
+ MIC: Message Integrity Check
+ HMAC (Hash-based Message Authentication Code) is a construction that combines a one-way hash function with a secret key to produce a message authentication code (MAC).
+ One-way hash functions can also be used for password storage by hashing the password and storing the hash value instead of the password itself. When a user enters a password, the system hashes it and compares the hash value to the stored hash value. This makes it difficult for an attacker who gains access to the stored hash values to determine the passwords. But as you will see in the following exercice with HashCat. If the password is not secure (findable in a dictionnary of password) the hash is crackable.


## a) Install Hashcat.

On my Debian virtual machine I installed HashCat following Tero Karvinen [tutorial](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/).

_during the lesson of February 08, 2022_

### HashCat

### Rockyou
Rockyou is a famous big dictionnary of leaked password.
The txt file is downlodable freely from [Brannon Dorsey GitHub account](https://github.com/brannondorsey/naive-hashcat/releases)

b) Crack this hash: 8eb8e307a6d649bc7fb51443a06a216f

I openned a terminal. 

First we have to identify the hash type.

```hashid -m 8eb8e307a6d649bc7fb51443a06a216f```

![hashid -m ](pictures/hashcat_1.png) 

I get a result that it's probably MD2, MD5 or MD4 hash type.

Let's try with MD5 as it is one of the most common.

It means that in the command I've to use the parameters -m **0**

```hashcat -m 0 '8eb8e307a6d649bc7fb51443a06a216f' rockyou.txt -o solved```

Working...
![hashid -m ](pictures/hashcat_2.png) 
It's cracked!
![hashid -m ](pictures/hashcat_3.png) 

I specified that I wanted the result in a file called solved

Let's open it:

`cat solved`

We can see that this hash was generated from the password _february_
![hashid -m ](pictures/hashcat_4.png) 

## c) Compile John the Ripper, Jumbo version. Karvinen 2023: Crack File Password With John.

On my Debian virtual machine I installed _John the Ripper_ following Tero Karvinen tutorial [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/).

I installed the pre-required librairies

`sudo apt-get update`
`sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip wget`

I downloaded _John the Ripper_ by clonning a GitHub repoistory

The option **_--depth=1_** is to clone only the latest version

`git clone --depth=1 https://github.com/openwall/john.git`

Then I moved into the cloned repository
`cd john/src/`

Detect the environment and create a Malefile for the next command
`./configure`
![end of configure command](pictures/john_1.png) 

`make -s clean && make -sj4`

![make](pictures/john_2.png) 

`cd ..`

`cd john`

I launched it

`./john`
![make](pictures/john_3.png) 

## d) Crack a zip file password
I downloaded the ZIP sample file on [Tero Karvinen website](https://terokarvinen.com/2023/crack-file-password-with-john/tero.zip)

`wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip`


Extract the hash in a file

`./zip2john tero.zip >tero.zip.hash`
![hash](pictures/john_4.png) 

Cracking the ZIP password

`./john tero.zip.hash`

We can see that the password is _butterfly_
![crack](pictures/john_5.png) 

Now that I have the password I can unzip it!
![unzip](pictures/john_6.png) 


n) Voluntary: create a password protected file other than ZIP. Crack the password. How many formats can you handle?

