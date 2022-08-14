# Forta Node Taşıma Rehberi
telegram: @eozdemirok

**Ödüller**
- **Haftalık 400.000 fort paylaştırılacaktır
- **FORT toplam arz 1.000.000.000'dır, 
- 
**Sistem Gereksinimleri**

- **4 CPU 16 RAM 100+ SSD Ubuntu 20.04(daha düşük kurmak isteyenler için en az 8 ram zorunluluğu vardır)**

**Kurulum**

- **Güncellemeler ve Docker Kurulumu**
```
apt update
```
```
apt upgrade
```
```
apt install ca-certificates curl gnupg lsb-release git htop
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
apt-get update
```
```
apt-get install docker-ce docker-ce-cli containerd.io
```
```
docker version
```
```
nano /etc/docker/daemon.json
```


- **deamon.json sayfası açıldığında aşağıdaki kodu girin**

```

{ 
   "default-address-pools": [ 
        { 
            "base":"172.17.0.0/12", 
            "size":16 
        }, 
        { 
            "base":"192.168.0.0/16" , 
            "size":20 
        }, 
        { 
            "base":"10.99.0.0/16", 
            "size":24 
        } 
    ] 
}
```
- **Kodu girdikten sonra CTRL-X Yapıp Sonra Y enter yaparak dosyayı kaydedin.**

**Yeniden başlatma**
```
systemctl restart docker
```

- **Biraz bekledikten sonra (30sn) şu kodları girin**

```
sudo curl https://dist.forta.network/pgp.public -o /usr/share/keyrings/forta-keyring.asc -s
```
```
echo 'deb [signed-by=/usr/share/keyrings/forta-keyring.asc] https://dist.forta.network/repositories/apt stable main' | sudo tee -a /etc/apt/sources.list.d/forta.list
```
```
apt-get update
```
```
apt-get install forta
```

- **En önemli yer! <your_passphrase> yerine bir şifre belirleyin ve bunu unutmayın.**
```
forta init --passphrase <your_passphrase>
```
- **Bu koddan sonra size bir cüzdan adresi verecek çıktı şöyle gözükmelidir.**

```
Scanner address: 0x43919032A43E0Ed2Dda502756D8D9D7324836920

Successfully initialized at /root/.forta

- Please make sure that all of the values in config.yml are set correctly.
- Please fund your scanner address with some MATIC.
- Please enable it for the chain ID in your config by doing 'forta register --owner-address <your_owner_wallet_address>'.
```

- **Taşıma işlemi yaptığımız için matic göndermenize gerek yoktur**

systemctl enable forta
```
```
nano /lib/systemd/system/forta.service
```
- **Dosyanın içindekileri CTRL-K ile silin <şifreniz> olan yeri değiştirin.**

```
[Unit]
Description=Forta
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
Environment="FORTA_DIR=/root/.forta/"
Environment="FORTA_PASSPHRASE=<şifreniz>"
Restart=on-failure
RestartSec=15s

ExecStart=/usr/bin/forta run

[Install]
WantedBy=multi-user.target
```
 - **Yukarıdaki kodu yapıştırdıktan sonra CTRL-X Yapıp Sonra Y enter yaparak dosyayı kaydedin.**

**Windows için winscp programı ile yada bilgisayarınıza uygun program ile sunucumuza bağlanıyoruz ve .forta klasörünü silip eski yedeklediğimiz klasörü yüklüyoruz(ctrl alt h / gizli dosyaları gösterir)**

- **tekrar terminale bağlanarak aşağıdaki kodların hepsini yapıştırın**

```
chmod -R +x .forta/*
systemctl daemon-reload
systemctl restart forta
systemctl status forta
```
- **1 dakika sonra aşağıdaki komutu girerek kontrol edin**

```
forta status
```

-Ömer Can hocamıza ve testnetrun ekibine teşekkürler

- **https://t.me/testnetrun**

- **https://www.youtube.com/c/TechTakip**

- **https://testnet.run/**

- **https://stake.testnet.run/**

- **https://twitter.com/testnetrun**



