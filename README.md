# NetPractice — Subnet Hesaplama Rehberi

Bu rehber, bir IP adresi ve maskeden **ağ adresi (network)**,  
**broadcast adresi** ve **host aralığını** hızlıca bulman için hazırlandı.

---

## 1) Adım Adım Çözüm Yöntemi

1. **IP & Maske'yi oku**  
   CIDR (ör: `/26`) değerini maske haline çevir (ör: `255.255.255.192`).

2. **Hangi okteti çözeceğini bul**  
   Maskede 255 olmayan ilk oktettir.  
   Örnek: `255.255.255.192` → 4. oktet.

3. **Blok boyutunu (block size) hesapla**  
   **Formül:** `Blok = 256 - maskenin o oktetteki değeri`

4. **Katlar tablosunu yaz**  
   `0, blok, 2*blok, 3*blok, ... 256`  
   Bu değerler alt ağların başlangıç noktalarıdır.

5. **IP'nin hangi aralıkta olduğunu bul**  
   IP’nin o oktetini katlarla karşılaştır, hangi iki kat arasındaysa o alt ağa düşer.

6. **Ağ ve broadcast adreslerini belirle**  
   - **Ağ adresi:** Katın kendisi  
   - **Broadcast:** Sonraki kat − 1  

7. **Host aralığını çıkar**  
   - İlk host = ağ adresi + 1  
   - Son host = broadcast − 1  
   - Host sayısı = `2^(host bit sayısı) − 2`

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

## 3) Egzersiz (Adım Adım Çözüm)

### Örnek 1 — 192.168.10.77/26

**1) Block size’ı bul**  
- Maske: `/26 → 255.255.255.192`  
- Son oktet: `192`  
- **Block = 256 - 192 = 64**

**2) Katlar tablosunu çıkar**  
`0, 64, 128, 192`

**3) IP’nin hangi aralıkta olduğunu bul**  
- IP’nin son okteti: `77`  
- `77`, `64` ile `128` arasına düşer → Bu ağ seçilir.

**4) Ağ ve broadcast adreslerini hesapla**  
- **Ağ adresi:** `192.168.10.64`  
- **Broadcast:** `192.168.10.127`  

**5) Host aralığını çıkar**  
- İlk host = `192.168.10.65`  
- Son host = `192.168.10.126`

**Sonuç:**
```
Ağ     : 192.168.10.64
İlk    : 192.168.10.65
Son    : 192.168.10.126
Broad. : 192.168.10.127
```

**6) Görsel Düşünme**
```
Katlar: 0------64------128------192
              [ bizim subnetimiz ]
              64............127
```
- `64` → Ağ adresi  
- `127` → Broadcast  
- Aradaki değerler → Hostlar

---

### Örnek 2 — 10.3.200.45/20

- Maske: `255.255.240.0` → 3. oktet çözülür  
- **Block:** `256 - 240 = 16`  
- Katlar: `0, 16, 32, …, 192, 208, 224, 240`  
- 3. oktet `200` → `192` ile `208` arasına düşer  
- **Ağ:** `10.3.192.0`  
- **Broadcast:** `10.3.207.255`  
- **Host aralığı:** `10.3.192.1 – 10.3.207.254`

---

## 4) Hızlı Hatırlatma (Kopya Kağıdı)

- **Blok = 256 − maske değeri**  
- **Ağ = en yakın küçük kat**  
- **Broadcast = bir sonraki kat − 1**  
- **Hostlar = ağ+1 → broadcast−1**  
- **Katlar Tablosu** subneti bulmanın anahtarıdır.
