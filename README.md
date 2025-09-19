# NetPractice — Subnet Hesaplama Rehberi

Bu rehber, bir IP adresi ve maskeden **ağ başlangıcı (network address)**,  
**yayın adresi (broadcast address)** ve **host aralığını** hızlıca bulman için hazırlandı.

---

## 1) Adım Adım Çözüm Yöntemi

1. **IP & Maske'yi oku.**  
   CIDR'ı (ör: `/26`) maske değerine çevir (ör: `255.255.255.192`).

2. **Çözüm yapılacak okteti bul.**  
   Maskede 255 olmayan ilk oktettir (ör: `255.255.255.192` → 4. oktet).

3. **Blok boyutunu (block size) hesapla.**  
   **Formül:** `Blok = 256 - maskenin o oktetteki değeri`

4. **Blok katlarını yaz.**  
   `0, blok, 2*blok, 3*blok, ... 256`  
   Bu değerler alt ağların başlangıç noktalarıdır.

5. **IP'nin hangi aralıkta olduğunu bul.**  
   IP’nin o oktetini katlar tablosu ile karşılaştır.  
   Hangi iki kat arasındaysa o alt ağ seçilir.

6. **Ağ ve broadcast adreslerini bul.**  
   - **Ağ adresi:** Katın kendisi  
   - **Broadcast:** Sonraki kat − 1  

7. **Host aralığını bul.**  
   - İlk host = Ağ adresi + 1  
   - Son host = Broadcast − 1  
   - Host sayısı = `2^(host_bit_sayısı) − 2`

---

## 2) En Çok Kullanılan Maskeler ve Bloklar

| CIDR | Maske (son oktet) | Blok | Katlar |
|------|------------------|------|-------|
| /25  | 128              | 128  | 0, 128 |
| /26  | 192              | 64   | 0, 64, 128, 192 |
| /27  | 224              | 32   | 0, 32, 64, 96, 128, 160, 192, 224 |
| /28  | 240              | 16   | 0, 16, 32, 48, …, 240 |
| /29  | 248              | 8    | 0, 8, 16, 24, …, 248 |
| /30  | 252              | 4    | 0, 4, 8, 12, …, 252 |

---

## 3) Örnek Çözüm

### Örnek — 192.168.10.77/26

1. Maske: `255.255.255.192`
2. Blok: `256 - 192 = 64`
3. Katlar: `0, 64, 128, 192`
4. 77 → 64 ile 128 arası  
   → **Ağ Başlangıcı:** `192.168.10.64`
5. Broadcast = `128 − 1 = 127`  
   → **Broadcast:** `192.168.10.127`
6. Host aralığı = `65 → 126`  
   → **İlk Host:** `192.168.10.65`, **Son Host:** `192.168.10.126`

---

## 4) Hızlı Hatırlatma (Kopya Kağıdı)

- **Blok = 256 − maske değeri**
- **Ağ = en yakın küçük kat**
- **Broadcast = bir sonraki kat − 1**
- **Hostlar = ağ+1 → broadcast−1**
- **Katlar Tablosu** subneti bulmanın anahtarıdır.

