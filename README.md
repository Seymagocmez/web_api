# Web API & HTTP'nin Temelleri 

Uygulamalar arası veri iletişimi günümüz yazılım dünyasının temel taşlarından biridir. Bu iletişim, genellikle **Web API**'ler aracılığıyla sağlanır. 

Web API, farklı yazılım uygulamalarının internet üzerinden iletişim kurmasını sağlayan bir arayüzdür. Uygulamalar arasında veri alışverişi ve işlev çağrıları yapılmasına olanak tanır. Böylece, farklı platformlarda çalışan programlar birbirleriyle entegre olabilir ve işbirliği yapabilir.

Web API'lerin altında yatan protokol ise çoğu zaman **HTTP**’dir. HTTP sadece sayfa yüklemeleriyle sınırlı kalmaz, aynı zamanda API çağrılarında da yoğun olarak kullanılır.

## 1. HTTP Temelleri

HTTP (HyperText Transfer Protocol), web üzerindeki istemci ve sunucu arasında veri alışverişini sağlayan bir iletişim protokolüdür. Uygulama katmanında (Application Layer) çalışır ve İnternet protokolü (IP) üzerinde taşınan verilerin nasıl iletileceğini belirler. HTTP, istemcinin (genellikle web tarayıcısı veya API istemcisi) sunucuya istek göndermesini ve sunucunun da bu isteğe karşılık yanıt vermesini sağlar. Web sayfalarının, API verilerinin ve diğer kaynakların transferinde en yaygın kullanılan protokoldür.

> Uygulama katmanıyla ilgili [bu](https://github.com/Seymagocmez/communication_protocols?tab=readme-ov-file#uygulama-katman%C4%B1-application-layer) yazıyı okuyabilirsiniz.

### HTTP Sürümleri

- **HTTP/1.1:** Her istek için ayrı TCP bağlantısı kullanır.
- **HTTP/2:** Tek bağlantı üzerinden çoklu istek gönderimi (multiplexing).
- **HTTP/3:** QUIC protokolü ile UDP tabanlı, daha hızlı.

###  HTTP Metodları

| Yöntem  | Açıklama                         |
|--------|----------------------------------|
| `GET`   | Veri çekmek için kullanılır.     |
| `POST`  | Yeni veri oluşturmak için.       |
| `PUT`   | Var olan veriyi güncellemek için.|
| `DELETE`| Veri silmek için.                |
| `PATCH` | Kısmi güncellemeler için.        |

### HTTP Durum Kodları
![http-durum-kodlari](https://github.com/user-attachments/assets/ea998a5e-92aa-46dd-a5f2-ed4a9534a1ba)

###  HTTP Header ve Çerezler

- **Headers:** `Content-Type`, `Authorization` gibi başlıklar; metadata taşır.
- **Cookies:** Tarayıcıda saklanan oturum verileri.

###  CORS ve Caching
>
- **CORS (Cross-Origin Resource Sharing):** Farklı domain'lerden gelen isteklere izin verip vermeme durumunu belirler.
- **Caching:** Yanıtların bellekte saklanması ile performans artışı.
  
## 2. Postman ile API Testi

[Postman](https://www.postman.com/) geliştiricilerin HTTP temelli API'lerin test ve dokümantasyonunu sağlayan güçlü bir araçtır.

### Özellikleri:

- Basit bir arayüzle `GET`, `POST`, `PUT`, `DELETE` gibi HTTP istekleri gönderilebilir.
- JSON yanıtları okunabilir biçimde görüntülenir.
- Header ve token yönetimi kolaydır.
- Test koleksiyonları ve otomasyon desteği sunar.

## 3. Web API Türleri ve Bazı Mimariler

![API_TYPES](https://github.com/user-attachments/assets/602e31ae-1d4e-4a66-945e-c2bfd78e1886)

Modern web sistemleri için farklı ihtiyaçlara yönelik çeşitli seçenekler bulunur. 

### REST (Representational State Transfer)

- HTTP üzerinden çalışır.
- Her kaynak bir URL ile temsil edilir.
- JSON kullanımı yaygındır.
- Fazla veri çekme riski (overfetching) vardır çünkü istemci ihtiyacından daha fazla veri talep eder; bu da gereksiz ağ trafiği ve performans kaybına yol açar. 

```http
GET /api/users/1
```

### GraphQL

- Tek bir endpoint ile çalışır.
- İstemci ihtiyacı olan alanları seçerek sorgular.
- Veri kontrolü istemcidedir (client).
- Kullanımı REST'e göre daha zordur. 

```graphql
query {
  user(id: "1") {
    name
    email
  }
}
```

### WebSocket

- Gerçek zamanlı çift yönlü iletişim sağlar.
- HTTP ile başlar, TCP üzerinden devam eder.
- Canlı verinin olduğu durumlarda ideal bir mimaridir.
- Bağlantı yönetimi REST’e göre karmaşıktır.

```text
Client ⇄ WebSocket ⇄ Server
```

### gRPC

- Google tarafından geliştirilmiştir.
- HTTP/2 ve Protocol Buffers kullanır.
- Mikroservis mimarileri için uygundur.
- JSON yerine Protobuf kullanımından ötürü hata ayıklama daha zordur.

  > Protocol Buffer: Google tarafından geliştirilen, veri serileştirme (serialization) formatıdır. Veriyi yapılandırılmış bir şekilde tanımlayıp daha sonra bunu kompakt, hızlı ve taşınabilir bir biçimde saklamak veya ağ üzerinden iletmek için kullanılır.

```proto
service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
}
```
### MQTT
![pubsub](https://github.com/user-attachments/assets/c5b6c092-71c9-4fc4-be30-1bd796f56f34)

> Publish/Subscribe (Yayınla/Abone Ol) Modeli, mesajların bir konu (topic) üzerinden yayıncılar (publishers) tarafından gönderilip, bu konulara abone olan (subscribers) alıcılara iletildiği bir iletişim desenidir. Yayıncılar ve aboneler birbirinden bağımsız çalışır, böylece sistem gevşek bağlı ve ölçeklenebilir olur. Bu model; haber servisleri, IoT uygulamaları ve mikroservisler gibi birçok alanda gerçek zamanlı ve asenkron iletişim için kullanılır.

> Broker, yayıncı ile abone arasında mesajları yönlendiren aracıdır. Yayıncıdan aldığı mesajı, ilgili abonelere ileterek iletişimi sağlar.
 
- Publish/Subscribe modeli ile çalışır.
- Genellikle IoT cihazları ve sensörler için tercih edilir.
- Broker üzerinden iletişim sağlanır.
```text
Publisher → Broker → Subscriber
```


### Serverless API

- Belirli olaylarla tetiklenen fonksiyonlar olarak çalışır.
- AWS Lambda, Google Cloud Functions gibi servisler tarafından sunulur.
- Cold Start (İlk Başlatma Gecikmesi) yaşanabilir.

```text
Client → Function → Response
```

## 📋 4. Protokollerin Karşılaştırması

| Teknoloji   | Protokol     | En İyi Kullanım Alanı               | Veri Formatı |
|-------------|--------------|-------------------------------------|--------------|
| REST        | HTTP         | Genel API kullanımı                 | JSON         |
| GraphQL     | HTTP         | Karmaşık veri sorguları             | JSON         |
| WebSocket   | TCP/HTTP     | Gerçek zamanlı uygulamalar          | Serbest      |
| gRPC        | HTTP/2       | Mikroservisler arası iletişim       | Protobuf     |
| MQTT        | TCP          | IoT cihazları, sensörler            | Serbest      |
| Serverless  | HTTP/Event   | Anlık, tetiklenen işlemler          | JSON         |

## Gerçek Uygulamalardan Bir Senaryo: 
---
Bir e-ticaret uygulamasında, kullanıcıların siparişlerini takip edebileceği bir sistem düşünelim. Bu sistemde:

- Kullanıcı mobil uygulamadan sipariş durumunu sorgular.

- Mobil uygulama, bir REST API kullanarak şu isteği gönderir:
  
```proto
GET /api/orders/12345
```
- Sunucu, application/json formatında siparişin mevcut durumunu (Hazırlanıyor, Kargoya verildi, Teslim edildi gibi) döner.

- Fakat kullanıcı, yalnızca sipariş durumu ve tahmini teslim tarihi ile ilgilenirken, API cevabında tüm ürün detayları, fatura bilgisi ve stok verisi de yer alır. Bu durumda:

- Bu gereksiz veri transferi, overfetching problemine yol açar. Hem istemcinin belleği hem de ağ kaynakları boşa harcanır.

- Alternatif olarak, GraphQL tercih edilseydi istemci sadece ihtiyacı olan alanları çekebilirdi:

```proto
query {
  order(id: "12345") {
    status
    estimatedDelivery
  }
}
```

Gerçek zamanlı teslimat durumu izlemek isteseydik, bu sistem WebSocket veya MQTT ile entegre edilerek anlık bildirimlerle daha interaktif hale getirilebilirdi.

## Kaynaklar

- [MDN Web Docs – HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [Postman API Fundamentals](https://learning.postman.com/docs/getting-started/introduction/)
- [GraphQL vs REST – Apollo Blog](https://www.apollographql.com/blog/graphql-vs-rest/)
- [6 Common Ways To Build APIs](https://blog.amigoscode.com/p/6-common-ways-to-build-apis)
- [API Development Roadmap For Developers](https://blog.amigoscode.com/p/api-development-roadmap-for-developers)
- [HTTP Durum Kodları](https://boosmart.com/http-durum-kodlari-nedir-seo-icin-neden-onemlidir/)
- [Publish/Subscribe](https://aws.amazon.com/tr/what-is/pub-sub-messaging/)
  
