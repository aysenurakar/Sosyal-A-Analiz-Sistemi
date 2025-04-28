# 👥 Sosyal Ağ Analiz Uygulaması 

Bu proje, bir sosyal ağdaki kullanıcılar ve arkadaşlık ilişkilerini analiz etmek için geliştirilmiş **konsol tabanlı bir C uygulamasıdır**. Kullanıcılar, bir dosyadan okunur ve aralarındaki ilişkilerle 
birlikte çeşitli analizler yapılabilir.

---

## 📂 Proje Özellikleri

✅ Kullanıcılar ve ilişkiler `veriseti.txt` dosyasından okunur  
✅ Menü tabanlı sistem ile analiz seçilebilir  
✅ DFS ile belirli mesafedeki arkadaşlar bulunabilir  
✅ Ortak arkadaşlar tespit edilir  
✅ Topluluklar (bağlı bileşenler) listelenir  
✅ Etki alanı (reachability) hesaplanır  
✅ Kırmızı-Siyah Ağaç (Red-Black Tree) ile kullanıcı arama işlemi hızlı ve dengeli yapılır

---

## 🛠 Kullanılan Yapılar

- Yapılar (struct)
- Dinamik bellek yönetimi (`malloc`)
- Derinlik Öncelikli Arama (DFS)
- Kırmızı-Siyah Ağaç (Red-Black Tree)
- Menü tabanlı kullanıcı arayüzü
- Dosya okuma (`fopen`, `fgets`)

---

## 📄 Veri Formatı (`veriseti.txt`)

```txt
USER 101
USER 102
USER 103
USER 104
USER 105

FRIEND 101 102
FRIEND 101 103
FRIEND 102 104
FRIEND 103 104
FRIEND 104 105
