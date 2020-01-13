[![Build Status](https://travis-ci.org/KomodoPlatform/komodo.svg?branch=master)](https://travis-ci.org/KomodoPlatform/komodo)
[![Issues](https://img.shields.io/github/issues-raw/komodoplatform/komodo)](https://github.com/KomodoPlatform/komodo/issues)
[![PRs](https://img.shields.io/github/issues-pr-closed/komodoplatform/komodo)](https://github.com/KomodoPlatform/komodo/pulls)
[![Commits](https://img.shields.io/github/commit-activity/y/komodoplatform/komodo)](https://github.com/KomodoPlatform/komodo/commits/dev)
[![Contributors](https://img.shields.io/github/contributors/komodoplatform/komodo)](https://github.com/KomodoPlatform/komodo/graphs/contributors)
[![Last Commit](https://img.shields.io/github/last-commit/komodoplatform/komodo)](https://github.com/KomodoPlatform/komodo/graphs/commit-activity)


[![gitstars](https://img.shields.io/github/stars/komodoplatform/komodo?style=social)](https://github.com/KomodoPlatform/komodo/stargazers)
[![twitter](https://img.shields.io/twitter/follow/marmarachain?style=social)](https://twitter.com/marmarachain)
[![discord](https://img.shields.io/discord/412898016371015680)](https://discord.gg/NzpnAA)


![MarmaraCreditLoop Logo](https://raw.githubusercontent.com/marmarachain/Marmara-v.1.0/master/marmara_coin_logo.png "Marmara Credit Loop Logo")


## Marmara v.1.0 Kurulumu

Kurulum sonrası Kredi Döngüsü yapabilmek ve test edebilmek için aşağıdaki dökümanı ziyaret edebilirsiniz.
https://github.com/marmarachain/Marmara-v.1.0-setup/issues/1


## Marmara v.1.0 Kurulumu ve kullanım dökümanı
----------------------------------------------------------------------------
## 1. kısım - Install the dependency packages 
```	sudo apt-get update
	sudo apt-get upgrade -y
	sudo apt install ufw
	sudo ufw --version
	sudo ufw enable
	sudo apt-get install ssh
	sudo ufw allow "OpenSSH"
	sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libgtest-dev libqt4-dev libqrencode-dev libdb++-dev ntp ntpdate software-properties-common curl clang libcurl4-gnutls-dev cmake clang -y
	sudo apt-get install libsodium-dev
```
## 2. kısım - Install nanomsg 
```	
2. kısım
	Install nanomsg
	
	cd /tmp
	wget https://github.com/nanomsg/nanomsg/archive/1.0.0.tar.gz -O nanomsg-1.0.0.tar.gz --no-check-certificate
	tar -xzvf nanomsg-1.0.0.tar.gz
	cd nanomsg-1.0.0
	mkdir build
	cd build
	cmake .. -DCMAKE_INSTALL_PREFIX=/usr
	cmake — build .
	sudo ldconfig
	
	2.1
	git clone https://github.com/nanomsg/nanomsg
	cd nanomsg
	cmake .
	make
	sudo make install
	sudo ldconfig
	
```
## 3. kısım - Change swap size on to 4GB (Ubuntu) 
	
```
	sudo swapon --show
	free -h
	df -h
	sudo fallocate -l 4G /swapfile
	ls -lh /swapfile 
	sudo chmod 600 /swapfile 
	ls -lh /swapfile 
	sudo mkswap /swapfile 
	sudo swapon /swapfile
	sudo swapon --show 
	free -h
	sudo cp /etc/fstab /etc/fstab.bak
	echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

## 4 .kısım 
```	sudo sysctl vm.swappiness=10 
	This setting will persist until the next reboot. We can set this value automatically at restart by adding the line to our /etc/sysctl.conf file:
	sudo nano /etc/sysctl.conf 
	vm.swappiness=10
	sudo ufw allow 35832
	sudo ufw allow 35833
```

	
## 5. kısım - Installing Komodo	
```
	cd 
	git clone https://github.com/marmarachain/Marmara-v.1.0 komodo --branch FSM --single-branch
	cd komodo
	./zcutil/fetch-params.sh
	./zcutil/build.sh -j$(nproc)

```

**Bu sıralamadan sonra her şey normal çalışır vaziyette olacaktır. Marmara Chain sorunsuz vaziyette kullanabilirsiniz.**
----------------------------------------------------------------------------

Wallet adresi ve Pubkey alıp - pubkey ile Staking mod da başlatma.

## chain e start verelim. 

src klasorümüze girelim.

`cd ~/komodo/src`
  
## chaine ilk startımızı verelim.

`./komodod -ac_name=MARMARATEST -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000 &`

## ardından bir wallet adresi oluşturup not alınız. 

`./komodo-cli -ac_name=MARMARATEST getnewaddress`

## örnek wallet adresi 

`RJajZNoEcCRD5wduqt1tna5DiLqiBC23bo`

## oluşturulan wallet adresini alttaki komuttaki "wallet-adresi" yazan kısma girip enter'a basıyoruz 

`./komodo-cli -ac_name=MARMARATEST validateaddress "wallet-adresi"`


## bu şekilde çıktı alacaksınız. ve burada yazan pubkey i de not alınız. 
```
{
	": true,
	"address": "RJajZNoEcCRD5wduqt1tna5DiLqiBC23bo",
	"scriptPubKey": "76a914660a7b5c8ec61c59b80cf8f2184adf8a24bccb6b88ac",
	"segid": 52,
	"ismine": true,
	"iswatchonly": false,
	"isscript": false,
	"pubkey": "03a3f641c4679c579b20c597435e8a32d50091bfc56e28303f5eb26fb1cb1eee72",
	"iscompressed": true,
	"account": ""
}
```

 oluşturulan pubkeyi niz : 03a3f641c4679c579b20c597435e8a32d50091bfc56e28303f5eb26fb1cb1eee72

## Chaini  durduruyoruz. 
```
./komodo-cli -ac_name=MARMARATEST stop
```
## Sırada pubkeyimizi kullanarak chain i Mining modun da çalıştırmak. 

Aşağıki komutu kullanarak çalıştırabilirsiniz. aşağıda ki "-pubkey=pubkeyburayagirilecek"  kısma not aldığınız pubkeyi giriniz. ve alttaki komut satırını düzenledikten sonra "cd komodo/src" klasorüne girip yapıştırın.
	
```
./komodod -ac_name=MARMARATEST -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000 -gen -genproclimit=2 -pubkey="pubkeyburayagirilecek" &
```

## Ve artık mining halde çalışıyor sunucumuz. 

## mining dökümlerinize aşağıdaki kodları kullanarak ulaşabilirsiniz. 

```
./komodo-cli -ac_name=MARMARATEST getinfo
./komodo-cli -ac_name=MARMARATEST marmarainfo 0 0 0 0 pubkey (to get details)
```

## Marmara Chaini farklı modlarda çalıştırma  seçenekleri. 

```
-genproclimit=-1 Şayet -1 yaparsanız tüm işlemci (CPU) günü kullanır.
-genproclimit=1  Şayet 1 yaparsanız tek işlemci kullanır 
-genproclimit=0  Şayet 0 yaparsanız Staking modunda çalışır ve Aktif coini kullanarak Staking yaparsınız.

```

## Not : Sunucu kapanma durumunda yapılacaklar. 

```
cd /komodo/src
./komodo-cli -ac_name=MARMARATEST stop
./komodod -ac_name=MARMARATEST -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000 -gen -genproclimit=2 -pubkey="pubkeyburayagirilecek" &
```


## İletişim (Contact) 
B. Gültekin Çetiner http://twitter.com/drcetiner & ~Paro, (c) 2019



---
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
