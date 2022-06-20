# Cosmos-Validator-Rehberi



<pre class="notranslate"><code>seid keys list
</code></pre>


<h2>Kurulum Sırasında Eksik Paket ve Bağımlılık Sorunları İçin Repo Dosyası </h2> <br>
<a href="https://gist.github.com/tedyun/56ee0bc8d9d954c8b707a018f0ca7992" target="_blank" > Tıklayınız </a> <br>
sudo nano /etc/apt/sources.list<br>
dosyaya girdikten sonra herşeyi silip linkteki dosyanın içeriğini yapıştırın . <br>
Ctrl + O ile kaydedin . <br>
Ctrl + X ile çıkış yapın . <br>
<pre class="notranslate"><code>sudo apt update -y<br>
sudo apt upgrade -y <br></code></pre>


<h2>Komut bulunamadı && Servis Bulunamadı Hatası İçin</h2><br>  
<pre class="notranslate"><code> source $HOME/.bash_profile<br></code></pre>

<h2>Cüzdan Oluşturma</h2><br>
Yeni cüzdan oluşturmak için aşağıdaki komutu kullanabilirsiniz. Hatırlatıcıyı(mnemonic) kaydetmeyi unutmayın.<br>
<pre class="notranslate"><code> seid keys add <cuzdan adı> <br></code></pre>

<h3>Örnek </h3> 
 <pre class="notranslate"><code> seid keys add deneme</code></pre>

(İSTEĞE BAĞLI) Cüzdanınızı özel anahtar (mnemonic) kullanarak kurtarmak için:
<pre class="notranslate"><code> seid keys add <cuzdan adı> --recover</code></pre>

<h3>Örnek </h3> 
<pre class="notranslate"><code> seid keys add deneme --recover</code></pre>

<h3>Cüzdan adresinizi öğrenmek için </h3>
<pre class="notranslate"><code> seid keys show <cuzdan adi> -a</code></pre>
  
<h3>Örnek </h3>
<pre class="notranslate"><code> seid keys show deneme -a</code></pre>

<h2>Mevcut cüzdan listesini almak için:</h2>

<pre class="notranslate"><code> seid keys list</code></pre>

<h2>Cüzdan bakiyenizi kontrol etmek için:</h2>
  
<pre class="notranslate"><code> seid query bank balances <cuzdan adresi></code></pre>
  
<h3>Örnek </h3> 
  
<pre class="notranslate"><code> seid query bank balances sei1fv4npf94ayteux82srsznyh8r6tahv7pzyrvvv</code></pre>
<br>
  
Cüzdanınızda bakiyenizi göremiyorsanız, muhtemelen  hala eşitleniyordur. Lütfen senkronizasyonun bitmesini bekleyin ve tekrar deneyin.
<br>

<h2>Validator Oluşturma Komutu </h2>
<pre class="notranslate"><code>
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
 </code></pre>
  <h4>Örnek</h4>
  <pre class="notranslate"><code>
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
</code></pre>
<h2>Validator adresi görüntüleme </h2> 
<pre class="notranslate"><code> seid keys show <cuzdan adi> --bech val -a</code></pre>
  
<h3>Örnek</h3>
<pre class="notranslate"><code> seid keys show deneme --bech val -a</code></pre>

<h1>Kullanışlı Komutlar</h1>
  

<h2>Logları Kontrol Et</h2>br>
Senkronize olup olmadığını , çalışır durumda olup olmadığını görmenize yardımcı olur . 
  
<pre class="notranslate"><code> journalctl -fu seid -o cat</code></pre>
  
  <h2>Servis Yönetimi</h2>br>
  Herhangi bir güncelleme veya özel durumda servise gerekli müdahaleyi yapmanızı sağlar . Örneğin node durdurup güncellemek için kullanılır . 
Yada olası bi sorunda restart komutu ile yeniden başlatılır . 
  
<h3>Servisi Başlat:</h3>

<pre class="notranslate"><code> systemctl start seid</code></pre>
  
<h3>Servisi Durdur:</h3>

<pre class="notranslate"><code> systemctl stop seid</code></pre>
  
<h3>Servisi Yeniden Başlat:</h3>

<pre class="notranslate"><code> systemctl restart seid</code></pre>
<h2>Node Bilgileri</h2>
<h3>Senkronizasyon Bilgisi:</h3>

<pre class="notranslate"><code> seid status 2>&1 | jq .SyncInfo</code></pre>
<h3>Validator Bilgisi:</h3>

<pre class="notranslate"><code> seid status 2>&1 | jq .ValidatorInfo</code></pre>
<h3>Node Bilgisi:</h3>

<pre class="notranslate"><code> seid status 2>&1 | jq .NodeInfo</code></pre>
<h3>Node ID Göster </h3>

<pre class="notranslate"><code> seid tendermint show-node-id</code></pre>
  
<h1>Cüzdan İşlemleri</h1>
  
<h2>Cüzdan Silme</h2>

<pre class="notranslate"><code> seid keys delete <cuzdan adi> </code></pre>
  
  <h3>Örnek</h3>
 <pre class="notranslate"><code>  seid keys delete deneme</code></pre>

<h2>Cüzdandan Cüzdana Bakiye Transferi</h2>
10000000usei
<pre class="notranslate"><code> seid tx bank send <cuzdan adresiniz> <alıcı adres> <token miktarı></code></pre>
  
  <h3>Örnek</h3>
<pre class="notranslate"><code> seid tx bank send sei1fv4npf94ayteux82srsznyh8r6tahv7pzyrvvv sei1eenrsthfhs0nqd3uppalgsr34kq4sdc5rgt3lc 10000000usei</code></pre>
  
<h2>Oylama</h2>
<pre class="notranslate"><code> seid tx gov vote 1 yes --from <cuzdan adınız> --chain-id=<chain ismi></code></pre>
  <h3>Örnek</h3>
<pre class="notranslate"><code> seid tx gov vote 1 yes --from deneme --chain-id= sei-testnet-2</code></pre>
<h1>Stake, Delegasyon ve Ödüller<h1>
<h3>Delegate İşlemi:<h3>

<pre class="notranslate"><code> seid tx staking delegate $VALOPER_ADDRESS 10000000usei --from=$WALLET --chain-id=$CHAIN_ID --gas=auto</code></pre>
Payını doğrulayıcıdan başka bir doğrulayıcıya yeniden devretme:

<pre class="notranslate"><code> seid tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 10000000usei --from=$WALLET --chain-id=$CHAIN_ID --gas=auto </code></pre>
<h3>Tüm ödülleri çek:</h3>

<pre class="notranslate"><code> seid tx distribution withdraw-all-rewards --from=$WALLET --chain-id=$CHAIN_ID --gas=auto</code></pre>
<h3>Komisyon ile ödülleri geri çekin:</h3>

<pre class="notranslate"><code> seid tx distribution withdraw-rewards $VALOPER_ADDRESS --from=$WALLET --commission --chain-id=$CHAIN_ID</code></pre>

<h3>Token Göndermek için :</h3>
  
<pre class="notranslate"><code> seid tx bank send <gonderici cüzdan> <alıcı cüzdan> <token adedi> factory/<token adresi>/<token adı> --chain-id <chainid> --fees 5000usei --gas auto -y</code></pre> 
  
  
<h2>Validatör İsmini , Resmini ve Açıklamasını Değiştir</h2>
<pre class="notranslate"><code>
seid tx staking edit-validator  
--moniker="Node İsminiz"  
--website="websiteniz"   
--identity=<keybase anahtarı>  
--details="açıklama"   
--chain-id=<chain adı>
--gas="auto"  
--from=<cüzdan adı>
</code></pre>
 <h3> Örnek </h3>
 <pre class="notranslate"><code>
  seid tx staking edit-validator  
--moniker="Node İsminiz"  
--website="https://..."   
--identity=A4C5A43A85D46  
--details="açıklama"   
--chain-id=sei-testnet-2
--gas="auto"  
--from=deneme
  </code></pre>
<h2>Jail'den Kurtul(Unjail)</h2>
<pre class="notranslate"><code>
seid tx slashing unjail \
  --broadcast-mode=block \
  --from=<cüzdan adı> \
  --chain-id=<chain adı> \
  --gas=auto
  </code></pre>
  <h3>Örnek</h3>
  <pre class="notranslate"><code>
  seid tx slashing unjail \
  --broadcast-mode=block \
  --from=deneme \
  --chain-id=sei-testnet-2 \
  --gas=auto
  </code></pre>
  
<h2>Node Tamamen Silmek</h2>
<pre class="notranslate"><code>
sudo systemctl stop seid && \
sudo systemctl disable seid && \
rm /etc/systemd/system/seid.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf .sei sei && \
rm -rf $(which seid)
</code></pre>
