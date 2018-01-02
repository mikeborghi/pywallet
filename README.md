## Pywallet 2.3 - wallet.dat management and key recovery tool forked by Mike Borghi 
###### This documentation is a work in progress and I plan to finish it within a couple days
___
###### Other helpful programs:
* [Bitcoin-tool (cli)](https://github.com/matja/bitcoin-tool) for all things key related (useful for converting hexprivkey to different coins)
* [Wallet-key-tool (GUI)](https://github.com/prof7bit/wallet-key-tool/releases) GUI for  importing almost every different wallet file and export to various standard formats 

----
### Main features:
* ##### Dump wallet private keys 
* ##### Scan drives for any potential privkey's and store in new .dat
* Will Add More
##### Based off of [jackjack's](https://github.com/jackjack-jj) fork of Joric's pywallet  with some minor bug fixes. Please submit issues and I will attempt to fix them
###### If you are looking to recover a wallet file that will not open in their respective programs, click [to be finished soon] for a easy to follow guide to recovering your funds 
###### Requirements:
* BSDDB 
* Python 2.x

------
#### Notes:
* This version is incompatible with python3
* BSDDB is required 	--- [Installation instructions using pip](https://stackoverflow.com/questions/16003224/installing-bsddb-package-python)
* The primary usage of this tool at this point in time would be to dump old wallet.dat files because SPV wallets have become the norm.  
* The original repo did not have instructions for the recover function despite having the code for doing so

##### Known bugs:
* Using othercoinversion still uses the bitcoin WIF prefix
* Dumpwithbalance is barely functional because of blockchain.info's rate limiter that was introduced after the creation of this version of pywallet

#### To Do:
* Finish this documentation with more example commands 
*  ``` --dumpwithbalance ``` uses blockchain.info api which implemented a very strict rate limiter so add multiple data sources 
* Change french storage size prefixes to English (e.g. Gio to Gb) because Murica
* Add command line options for different popular coin types (like the currently implmented --testnet & --namecoin arguments)
* Add missing features to the web client (web client has limited functionality)


###### While there are many versions of pywallet around, this version has more functionality such as:
* a web interface 
* recover function which allows for scanning entire volumes or individual files for anything that resembles a privatekey and creates a .dat with the results.  



-------
#### Examples:
* #### **Show arguments** 
	###### Simply navigate to the folder with pywallet.py in it and type the following to view the help text
	 ```` python pywallet-py````
* #### Dump a wallet.dat located in the current directory
	``` python pywallet.dat --dumpwallet --datadir=./ --wallet=wallet.dat  ```
    * The output of each address in the wallet has the following fields
    *        
            "addr": " ", 
            "balance": "x", [if --dumpwithbalance is an argument)
            "compressed": [true,false], 
            "hexsec": "[hex priv key]", 
            "private": "[raw ecdsa privkey]", 
            "pubkey": " [addr pubkey] ", 
            "reserve": 1, 
            "sec": "[base58check privkey]", 
 
* ### To recover all private keys on a disk 
	1. *  **OSX** - open terminal and type
	   	``` diskutil list 	```
        *  **Unix** - open terminal and type 
	   	``` fdisk -l 	```   
	2. * Find the identifier for the volume you would like to recover keys from
	3. * Enter the following into terminal to scan ``` disk12 ``` the second partition ``` s2 ``` that has a size of 291Mb and output the recovered keys into the current directory  
	    ``` python pywallet.py --recover --recov_device=/dev/disk12s2  --recov_size=291Mio --recov_outputdir=. ```
        
		* ###### I believe the size acronyms are french.  
    		* Gio = 1024Mb
    		* Go = 1000Mb
    		* Mio = 1024Kb
    		* Mo = 1000Kb
    		* Etc. See line 94 - 101
-----
 
   ### Command Line Arguments
```   --version             show program's version number and exit
  -h, --help            show this help message and exit
  --passphrase=PASSPHRASE
                        passphrase for the encrypted wallet
  --dumpwallet          dump wallet in json format
  --dumpwithbalance     includes balance of each address in the json dump,
                        takes about 2 minutes per 100 addresses
  --importprivkey=KEY   import private key from vanitygen
  --importhex           KEY is in hexadecimal format
  --datadir=DATADIR     wallet directory (defaults to bitcoin default)
  --wallet=WALLETFILE   wallet filename (defaults to wallet.dat)
  --label=LABEL         label shown in the adress book (defaults to '')
  --testnet             use testnet subdirectory and address type
  --namecoin            use namecoin address type
  --otherversion=OTHERVERSION
                        use other network address type, whose version is
                        OTHERVERSION
  --info                display pubkey, privkey (both depending on the
                        network) and hexkey
  --reserve             import as a reserve key, i.e. it won't show in the
                        adress book
  --multidelete=MULTIDELETE
                        deletes data in your wallet, according to the file
                        provided
  --balance=KEY_BALANCE
                        prints balance of KEY_BALANCE
  --web                 run pywallet web interface
  --port=PORT           port of web interface (defaults to 8989)
  --recover             recover your deleted keys, use with recov_size and
                        recov_device
  --recov_device=RECOV_DEVICE
                        device to read (e.g. /dev/sda1 or E: or a file)
  --recov_size=RECOV_SIZE
                        number of bytes to read (e.g. 20Mo or 50Gio)
  --recov_outputdir=RECOV_OUTPUTDIR
                        output directory where the recovered wallet will be
                        put
  --clone_watchonly_from=CLONE_WATCHONLY_FROM
                        path of the original wallet
  --clone_watchonly_to=CLONE_WATCHONLY_TO
                        path of the resulting watch-only wallet
  --dont_check_walletversion
                        don't check if wallet version > 81000 before running
                        (WARNING: this may break your wallet, be sure you know
                        what you do)
  --wait=NSECONDS       wait NSECONDS seconds before launch```

```
------

### Original README Taken from [Jackjack's original repo](https://github.com/jackjack-jj/pywallet) 

```
Requirements: Python 2.x, with bsddb and twisted packages

Dependencies:

Debian-based Linux:
 aptitude install build-essential python-dev python-twisted python-bsddb3

Mac OS X:
 1. Install MacPorts from http://www.macports.org/
 2. sudo port install python27 py27-twisted py27-pip py-bsddb python_select
 3. sudo port select --set python python27
 4. sudo easy_install ecdsa

Windows: 
 1. Install Python 2.7
 2. Install Twisted 11.0.0 for Py2.7, then Zope.Interface (a .egg file) for Py2.7: http://twistedmatrix.com/trac/wiki/Downloads

 3. Untested, proposed by TeaRex: install Zope.Interface from http://www.lfd.uci.edu/~gohlke/pythonlibs

 If this doesn't work, you will have to install the egg file:

 3(32bit). http://pypi.python.org/pypi/setuptools#downloads to install setuptools
 3(64bit). http://pypi.python.org/pypi/setuptools#windows to download, then run ez_setup.py
 
 4. Go to C:\Python27\Scripts
 5. Run easy_install.exe zope.interface-3.6.4-py2.7-win-amd64.egg 
```
-------

