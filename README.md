# Cosmos-Validator-Rehberi



<pre class="notranslate"><code>seid keys list
</code></pre>


<h2>Kurulum Sırasında Eksik Paket ve Bağımlılık Sorunları İçin Repo Dosyası </h2> <br>
<a href="https://gist.github.com/tedyun/56ee0bc8d9d954c8b707a018f0ca7992" target="_blank" > Tıklayınız </a> <br>
sudo nano /etc/apt/sources.list<br>
dosyaya girdikten sonra herşeyi silip yukardaki dosyanın içeriğini yapıştırın . <br>
Ctrl + O ile kaydedin . <br>
Ctrl + X ile çıkış yapın . <br>
sudo apt update -y<br>
sudo apt upgrade -y <br>


:::: Komut bulunamadı && Servis Bulunamadı Hatası İçin : : : :<br>  
source $HOME/.bash_profile<br>

Cüzdan Oluşturma<br>
Yeni cüzdan oluşturmak için aşağıdaki komutu kullanabilirsiniz. Hatırlatıcıyı(mnemonic) kaydetmeyi unutmayın.<br>
seid keys add <cuzdan adı> <br>

Örnek : 
seid keys add deneme

(İSTEĞE BAĞLI) Cüzdanınızı özel anahtar (mnemonic) kullanarak kurtarmak için:
seid keys add <cuzdan adı> --recover

Örnek : 
seid keys add deneme --recover

Cüzdan adresinizi öğrenmek için : 
seid keys show <cuzdan adi> -a
  
Örnek :
seid keys show deneme -a

Mevcut cüzdan listesini almak için:

seid keys list

Cüzdan bakiyenizi kontrol etmek için:
  
seid query bank balances <cuzdan adresi>
  
Örnek : 
  
seid query bank balances sei1fv4npf94ayteux82srsznyh8r6tahv7pzyrvvv
  
Cüzdanınızda bakiyenizi göremiyorsanız, muhtemelen  hala eşitleniyordur. Lütfen senkronizasyonun bitmesini bekleyin ve tekrar deneyin.

---------------------Validator Oluşturma Komutu ---------------------

seid tx staking create-validator \
  --amount 1000000usei \
  --from <cuzdan adı> \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(seid tendermint show-validator) \
  --moniker <validator adınız> \
  --chain-id <chain ismi>
 
  ------Örnek---
  
  seid tx staking create-validator \
  --amount 1000000usei \
  --from deneme \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(seid tendermint show-validator) \
  --moniker sinyordes \
  --chain-id sei-testnet-2

---------------------Validator adresi görüntüleme ------------------------- 
seid keys show <cuzdan adi> --bech val -a
  
Örnek:
seid keys show deneme --bech val -a

#################################Kullanışlı Komutlar###########################
  

---------------------Logları Kontrol Et---------------------
Senkronize olup olmadığını , çalışır durumda olup olmadığını görmenize yardımcı olur . 
  
journalctl -fu seid -o cat
  
  ---------------------Servis Yönetimi----------------------
  Herhangi bir güncelleme veya özel durumda servise gerekli müdahaleyi yapmanızı sağlar . Örneğin node durdurup güncellemek için kullanılır . 
Yada olası bi sorunda restart komutu ile yeniden başlatılır . 
  
Servisi Başlat:

systemctl start seid
  
Servisi Durdur:

systemctl stop seid
  
Servisi Yeniden Başlat:

systemctl restart seid
---------------------Node Bilgileri---------------------
Senkronizasyon Bilgisi:

seid status 2>&1 | jq .SyncInfo
Validator Bilgisi:

seid status 2>&1 | jq .ValidatorInfo
Node Bilgisi:

seid status 2>&1 | jq .NodeInfo
Node ID Göser:

seid tendermint show-node-id
  
##########################Cüzdan İşlemleri##################
  
-------------------------Cüzdan Silme:----------------------

seid keys delete <cuzdan adi> 
  
  ------Örnek-----
  seid keys delete deneme

------------------Cüzdandan Cüzdana Bakiye Transferi---------
10000000usei
seid tx bank send <cuzdan adresiniz> <alıcı adres> <token miktarı>
  
  -----Örnek-----
seid tx bank send sei1fv4npf94ayteux82srsznyh8r6tahv7pzyrvvv sei1eenrsthfhs0nqd3uppalgsr34kq4sdc5rgt3lc 10000000usei
  
------------------------------------Oylama---------------------
seid tx gov vote 1 yes --from <cuzdan adınız> --chain-id=<chain ismi>
  -----Örnek-----
seid tx gov vote 1 yes --from deneme --chain-id= sei-testnet-2
Stake, Delegasyon ve Ödüller
Delegate İşlemi:

seid tx staking delegate $VALOPER_ADDRESS 10000000usei --from=$WALLET --chain-id=$CHAIN_ID --gas=auto
Payını doğrulayıcıdan başka bir doğrulayıcıya yeniden devretme:

seid tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 10000000usei --from=$WALLET --chain-id=$CHAIN_ID --gas=auto
Tüm ödülleri çek:

seid tx distribution withdraw-all-rewards --from=$WALLET --chain-id=$CHAIN_ID --gas=auto
Komisyon ile ödülleri geri çekin:

seid tx distribution withdraw-rewards $VALOPER_ADDRESS --from=$WALLET --commission --chain-id=$CHAIN_ID

Token Yollamak için :
  
seid tx bank send <gonderici cüzdan> <alıcı cüzdan> <token adedi> factory/<token adresi>/<token adı> --chain-id <chainid> --fees 5000usei --gas auto -y
  
  
Validatör İsmini , Resmini ve Açıklamasını Değiştir:

kujirad tx staking edit-validator  
--moniker="Node İsminiz"  
--website="websiteniz"   
--identity=<keybase anahtarı>  
--details="Securely and non-custodial staking. Easily stake your assets with StakeRun"   
--chain-id=$CHAIN_ID   
--gas="auto"  
--from=wallet
  
  
Jail'den Kurtul(Unjail):

seid tx slashing unjail \
  --broadcast-mode=block \
  --from=$WALLET \
  --chain-id=$CHAIN_ID \
  --gas=auto
  
Node Tamamen Silmek:

sudo systemctl stop seid && \
sudo systemctl disable seid && \
rm /etc/systemd/system/seid.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf .sei sei && \
rm -rf $(which seid)
