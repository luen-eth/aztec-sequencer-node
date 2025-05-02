<h2 align=center>Aztec Sequencer Node Rehberi</h2>

🔷 Aztec Network'e Hoş Geldiniz! 🔷

Aztec, merkeziyetsiz ve gizlilik odaklı bir ağ inşa ediyor ve sequencer node bunun önemli bir parçası. Bir sequencer çalıştırmak, normal tüketici donanımı kullanarak bloklar üretmenize ve önermenize yardımcı olur. Bu rehber, testnet üzerinde bir sequencer kurmanız için size yol gösterecek.

**Not: Herhangi bir ödül, airdrop veya teşvik için resmi bir onay bulunmamaktadır. Bu tamamen öğrenme, katkıda bulunma ve öncü bir gizlilik projesinde erken olmak içindir.**

💻 Sistem Gereksinimleri 💻

| Bileşen      | Özellikler               |
|----------------|-----------------------------|
| CPU            | 8 çekirdekli İşlemci            |
| RAM            | 16 GiB                      |
| Depolama        | 1 TB SSD                    |
| İnternet Hızı | 25 Mbps Yükleme / İndirme   |

💡 Not: Bu node'u `4 çekirdekli CPU`, `6 GB RAM` ve `25 GB depolama` ile başlatabilirsiniz. Ancak, çalışma süresi arttıkça, önerilen sistem gereksinimlerini karşılamanız önemlidir - aksi takdirde node'unuz çökebilir.

🌐 VPS Kiralamak 🌐

💡 Not: Eğer ana hedefiniz Aztec Discord'da `Çırak` rolü almak ise, VPS kiralamak zorunlu değildir. Bu node'u WSL üzerinde 30 dakika çalıştırarak bu rolü alabilirsiniz

🔹 Ziyaret edin: [PQ Hosting](https://pq.hosting/?from=622403&lang=en) (yüksek fiyat ama kripto ödemesi destekliyor) veya [contabo](https://contabo.com/en) veya [hetzner](https://www.hetzner.com/cloud) VPS kiralamak için

💡 İpucu: Eğer VPS'in ne olduğunu veya nasıl satın alınacağını bilmiyorsanız, YouTube kanalımdaki [bu videoyu](https://youtu.be/vNBlRMnHggA?si=G1huqYU3ylCGoTQE) izlemelisiniz.

⚙️ Ön Gereksinimler ⚙️

🔸 Sepolia Ethereum RPC için [Alchemy](https://dashboard.alchemy.com/apps) veya [Infura](https://developer.metamask.io/register) kullanabilirsiniz.
🔸 Consensus URL (Beacon RPC URL) için [Chainstack](https://chainstack.com/global-nodes) kullanabilirsiniz.
🔸 Validator olarak kaydolmak istiyorsanız, yeni bir evm cüzdanı oluşturun ve en az 2.5 Sepolia ETH ile fonlayın.

⚠️ ÖNEMLİ: Ücretsiz versiyonu kullanıyorsanız ve Sepolia Ethereum RPC veya Sepolia Consensus (Beacon RPC) URL'sinde maksimum istek limitine ulaşırsanız, ya premium plana geçmeniz ya da limiti her aştığınızda RPC endpoint'ini değiştirmeniz gerekecektir.

📥 Kurulum Adımları 📥

💡 İpucu: Aztec sequencer node'u nasıl kuracağınızı öğrenmek için [bu videoyu](https://youtu.be/2mBIRmMPSEM?si=TG5MRwQyZ5XqcfLI) izleyebilirsiniz.

1️⃣ curl ve wget kurulumu:
```bash
command -v curl >/dev/null 2>&1 || apt-get update && apt-get install -y curl
command -v wget >/dev/null 2>&1 || apt-get install -y wget
```

2️⃣ Aztec node kurulum scriptini indirme ve çalıştırma:
```bash
# curl ile
[ -f "aztec.sh" ] && rm aztec.sh; curl -sSL -o aztec.sh https://raw.githubusercontent.com/zunxbt/aztec-sequencer-node/main/aztec.sh && chmod +x aztec.sh && ./aztec.sh

# veya wget ile
[ -f "aztec.sh" ] && rm aztec.sh; wget -q -O aztec.sh https://raw.githubusercontent.com/zunxbt/aztec-sequencer-node/main/aztec.sh && chmod +x aztec.sh && ./aztec.sh
```

⚡ Temel Komutlar ⚡

🔸 Log kontrolü:
```bash
sudo docker logs -f --tail 100 $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

🔸 Node durdurma:
```bash
sudo docker stop $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

🎯 Discord'da "Apprentice" Rolü Almak İçin 🎯

💡 Not: Node'u çalıştırdıktan sonra, bu komutları çalıştırmadan önce en az 10-20 dakika beklemeniz gerekir

1️⃣ Block numarası alma:
```bash
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' http://localhost:8080 | jq -r '.result.proven.number'
```

2️⃣ Proof alma (block-number yerine aldığınız numarayı yazın):
    
![Screenshot 2025-05-02 120017](https://github.com/user-attachments/assets/ed5ba08e-a1a9-48bc-8518-b23211ac7588)

```bash
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["block-number","block-number"],"id":67}' http://localhost:8080 | jq -r ".result"
```

3️⃣ Discord'da /operator start komutunu kullanın ve istenen bilgileri girin

🚀 Validator Olarak Kaydolma 🚀

⚠️ UYARI: Validator olarak kaydolmaya çalışırken `ValidatorQuotaFilledUntil` gibi bir hata görebilirsiniz, bu günlük kotanın dolduğu anlamına gelir - verilen Unix zaman damgasını yerel saate dönüştürerek ne zaman tekrar Validator olarak kaydolmayı deneyebileceğinizi öğrenebilirsiniz.

Validator kaydı için komut:
```bash
aztec add-l1-validator \
  --l1-rpc-urls SEPOLIA-RPC-URL \
  --private-key YOUR-PRIVATE-KEY \
  --attester YOUR-VALIDATOR-ADDRESS \
  --proposer-eoa YOUR-VALIDATOR-ADDRESS \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```

🔗 Faydalı Linkler 🔗

📹 Kurulum Videosu: https://youtu.be/2mBIRmMPSEM
💻 VPS Rehberi: https://youtu.be/vNBlRMnHggA
💬 Discord: https://discord.com/invite/aztec
