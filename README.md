# Marmara-v1.1-Kurulumu-Komodo-setup
Marmara v1.1 Kurulumu ve komodo kurulumu

**1. kısım - Install the dependency packages**
```	sudo apt-get update
	sudo apt-get upgrade -y
	sudo apt install ufw
	sudo ufw --version
	sudo ufw enable
	sudo apt-get install ssh
	sudo ufw allow "OpenSSH"
	sudo ufw allow 56141
	sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libgtest-dev libqt4-dev libqrencode-dev libdb++-dev ntp ntpdate software-properties-common curl clang libcurl4-gnutls-dev cmake clang -y
	sudo apt-get install libsodium-dev
```
**2. kısım - Install nanomsg**
```	
        git clone https://github.com/nanomsg/nanomsg
	cd nanomsg
	cmake .
	make
	sudo make install
	sudo ldconfig
```
**3. kısım - Change swap size on to 4GB (Ubuntu)**
	
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

**4 .kısım**
```	sudo sysctl vm.swappiness=10 
	This setting will persist until the next reboot. We can set this value automatically at restart by adding the line to our /etc/sysctl.conf file:
	sudo nano /etc/sysctl.conf 
	vm.swappiness=10
	sudo ufw allow 56141
```

	
**5. kısım - Installing Komodo**	
```		cd 
	git clone https://github.com/dimxy/komodo --branch marmara-v1-1 --single-branch
	cd komodo
	./zcutil/fetch-params.sh
	./zcutil/build.sh -j$(nproc)
```

**Bu sıralamadan sonra her şey normal çalışır vaziyette olacaktır. Marmara Chain sorunsuz vaziyette kullanabilirsiniz.**
----------------------------------------------------------------------------

Wallet adresi ve Pubkey alıp - pubkey ile Staking mod da başlatma.

**chain e start verelim.**

src klasorümüze girelim.

	`cd ~/komodo/src`
  
chaine ilk startımızı verelim.

	`./komodod -ac_name=MCL0 -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000 -reindex &`

**ardından bir wallet adresi oluşturup not alınız.**

	`./komodo-cli -ac_name=MCL0 getnewaddress`

**örnek wallet adresi** 

   `  RJajZNoEcCRD5wduqt1tna5DiLqiBC23bo`

**aldığımız wallet adresini onaylatıp pubkey oluşturmak için aşağıdaki komutta belirtilen tırnaklar arasına giriniz.**

	`./komodo-cli -ac_name=MCL0 validateaddress "buraya wallet adresinizi giriniz" `



**bu şekilde çıktı alacaksınız. ve burada yazan pubkey i de not alınız.**
```
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
```

 oluşturulan pubkeyi niz : `03a3f641c4679c579b20c597435e8a32d50091bfc56e28303f5eb26fb1cb1eee72`

**Chaini  durduruyoruz.**

`./komodo-cli -ac_name=MCL0 stop`

**Sırada pubkeyimizi kullanarak chain i Mining modun da çalıştırmak.**

Aşağıki komutu kullanarak çalıştırabilirsiniz. aşağıda ki "-pubkey=pubkeyburayagirilecek"  kısma not aldığınız pubkeyi giriniz. ve komutu komple kopyalayıp terminal de "cd komodo/src" klasöründeyken yapıştırın enterlayın.
	
     ./komodod -ac_name=MCL0 -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000 -gen -genproclimit=2 -pubkey="pubkeyburayagirilecek" &

**Ve artık mining halde çalışıyor sunucumuz.** 

**mining dökümlerinize aşağıdaki kodları kullanarak ulaşabilirsiniz.**

```
./komodo-cli -ac_name=MCL0 getinfo
./komodo-cli -ac_name=MCL0 marmarainfo 0 0 0 0 //to get details
```

**Marmara Chaini farklı modlarda çalıştırma  seçenekleri.**

```
-genproclimit=-1 Şayet -1 yaparsanız tüm işlemci (CPU) günü kullanır.
-genproclimit=1  Şayet 1 yaparsanız tek işlemci kullanır 
-genproclimit=0  Şayet 0 yaparsanız Staking modunda çalışır ve Aktif coini kullanarak Staking yaparsınız.

```

----------------------------------------------------------------------------

**Not : Sunucu kapanır , reset , çökme , yenileme durumlarında yapılacaklar.**

```
cd /komodo/src
./komodo-cli -ac_name=MCL0 stop
./komodod -ac_name=MCL0 -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000 -gen -genproclimit=2 -pubkey="pubkeyburayagirilecek" &
```


