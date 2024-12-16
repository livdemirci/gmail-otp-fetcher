# gmail-otp-fetcher

## Giriş

**gmail-otp-fetcher**, Gmail API'sine bağlanarak en son e-postayı alan ve içeriğinden altı haneli bir kod çıkaran Java tabanlı bir yardımcı araçtır. Araç, kimlik doğrulama için OAuth 2.0 kullanır ve e-posta içeriğini hem düz metin hem de HTML formatında işleyebilir.

Bu proje, e-postalarla gönderilen tek seferlik şifreleri (OTP) doğrulamak veya e-postalarda gömülü kodları çıkarmak gibi senaryolarda faydalıdır.

---

## İçindekiler

1. [Özellikler](#özellikler)
2. [Gereksinimler](#gereksinimler)
3. [Kurulum](#kurulum)
4. [Kullanım](#kullanım)
5. [Yapılandırma](#yapılandırma)
6. [Örnek](#örnek)
7. [Bağımlılıklar](#bağımlılıklar)
8. [Sorun Giderme](#sorun-giderme)
9. [Lisans](#lisans)

---

## Özellikler

- **OAuth 2.0 Kimlik Doğrulama**: OAuth 2.0 ile Gmail'e güvenli şekilde bağlanır.
- **E-posta Çözümleme**: Gmail gelen kutusundaki en son e-postayı alır.
- **Kod Çıkartma**: Regex kullanarak altı haneli bir kodu çıkarır.
- **HTML İşleme**: `Jsoup` kullanarak e-posta içeriğini işler ve veriyi çıkarır.
- **Hata Yönetimi**: E-postaların bulunamadığı veya kodun bulunamadığı durumları ele alır.

---

## Gereksinimler

1. **Java Geliştirme Kiti (JDK)**: 17 veya üzeri bir sürüm.
2. **Google Cloud Platform (GCP)**:
   - Bir proje oluşturun ve Gmail API'sini etkinleştirin.
   - OAuth 2.0 için `credentials.json` dosyasını indirin.
3. **Maven/Gradle**: Bağımlılık yönetimi için.
4. **API Erişim Yapılandırması**: Kimlik doğrulama jetonlarını saklamak için bir dizin ayarlayın.

---

## Kurulum

### Adım 1: Projeyi Klonlayın

```bash
git clone <repository-url>
cd <repository-folder>
```

### Adım 2: Gerekli Kütüphaneleri Ekleyin

Eğer Maven kullanıyorsanız, aşağıdaki bağımlılıkları `pom.xml` dosyanıza ekleyin:

```xml
<dependencies>
    <dependency>
        <groupId>com.google.api-client</groupId>
        <artifactId>google-api-client</artifactId>
        <version>1.36.0</version>
    </dependency>
    <dependency>
        <groupId>com.google.api-client</groupId>
        <artifactId>google-api-client-gson</artifactId>
        <version>1.36.0</version>
    </dependency>
    <dependency>
        <groupId>com.google.apis</groupId>
        <artifactId>google-api-services-gmail</artifactId>
        <version>v1-rev110-1.25.0</version>
    </dependency>
    <dependency>
        <groupId>org.jsoup</groupId>
        <artifactId>jsoup</artifactId>
        <version>1.15.3</version>
    </dependency>
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-codec</artifactId>
        <version>1.15</version>
    </dependency>
</dependencies>
```

Eğer Gradle kullanıyorsanız, şu bağımlılıkları `build.gradle` dosyanıza ekleyin:

```gradle
dependencies {
    implementation 'com.google.api-client:google-api-client:1.36.0'
    implementation 'com.google.api-client:google-api-client-gson:1.36.0'
    implementation 'com.google.apis:google-api-services-gmail:v1-rev110-1.25.0'
    implementation 'org.jsoup:jsoup:1.15.3'
    implementation 'org.apache.commons:commons-codec:1.15'
}
```

### Adım 3: Yapılandırma Dosyalarını Hazırlayın

1. `credentials.json` dosyanızı bilinen bir dizine yerleştirin.
2. Aşağıdaki anahtarlarla yapılandırma dosyasını (ör. `config.properties`) güncelleyin:

```
email2=sizin-email-adresiniz@example.com
tokenDirectoryPath=/path/to/token/directory
jsonDirectoryPath=/path/to/credentials.json
```

---

## Kullanım

### Uygulamayı Çalıştırma

1. Maven veya tercih ettiğiniz yapı aracıyla projeyi oluşturun.
2. Ana sınıfı çalıştırın:

```bash
java -cp target/your-jar-file.jar bilira.utilities.GmailQuickstart
```

3. Gmail hesabınıza erişim izni vermek için kimlik doğrulama işlemini takip edin.

---

## Yapılandırma

Aşağıdaki ayarları yapılandırma dosyasından değiştirebilirsiniz:

| Anahtar                | Açıklama                                |
|------------------------|-----------------------------------------|
| `email2`               | E-postaların alınacağı Gmail adresi.    |
| `tokenDirectoryPath`   | OAuth jetonlarının saklanacağı dizin.   |
| `jsonDirectoryPath`    | `credentials.json` dosyasının yolu.    |

---

## Örnek

```java
GmailQuickstart gmailQuickstart = new GmailQuickstart(
    ConfigReader.getProperty("email2"), 
    ConfigReader.getProperty("tokenDirectoryPath"), 
    ConfigReader.getProperty("jsonDirectoryPath")
);

gmailQuickstart.fetchDigitFromGmail();
System.out.println("6 Haneli Kod: " + gmailQuickstart.getDigit());
```

---

## Bağımlılıklar

- **Google API Client**: `google-api-client`, `google-api-client-gson`, `google-api-services-gmail`.
- **Apache Commons Codec**: Base64 çözümleme için.
- **Jsoup**: HTML işleme için.
- **JavaMail API**: MIME içerik işleme için.

---

## Sorun Giderme

- **Mesaj Bulunamadı**: Gmail gelen kutunuzda e-posta olduğundan ve API'nin gerekli izinlere sahip olduğundan emin olun.
- **Geçersiz Kimlik Dosyası**: `credentials.json` dosyasının yolunu kontrol edin ve GCP projenizle eşleştiğinden emin olun.
- **Token Sorunları**: Token dizinini silip yeniden kimlik doğrulaması yapın.
- **Regex Eşleşmiyor**: Regex mantığını, beklenen kod formatına uygun olacak şekilde güncelleyin.
- **Yanlış Kod Bilgisi Geliyor**: Jsoup ile DOM Manipülasyonu Sorun Giderme Jsoup, HTML içeriklerini ayrıştırmak ve DOM manipülasyonu yapmak için güçlü bir araçtır. Ancak, bazı durumlarda sorunlarla karşılaşabilirsiniz. İşte yaygın problemler ve çözüm yolları:

**Beklenen Verinin DOM'da Bulunamaması**

Sorun:

**E-posta içeriği DOM'da beklenen <span> veya <p> etiketleri içinde değil.**
Jsoup.select() kullanılarak hedef veri seçilemiyor.

Çözüm:
HTML yapısını kontrol edin:

Gelen HTML içeriğinin doğru şekilde ayrıştırılıp ayrıştırılmadığını kontrol edin.
HTML'nin tamamını loglayarak inceleyin:

System.out.println("HTML İçeriği: " + content);

Alternatif DOM seçimleri deneyin:

Hedef içeriğin doğru bir CSS seçiciyle bulunmasını sağlayın. Örneğin:
java

for (Element element : doc.select("div, span, p, strong")) {
    System.out.println("Bulunan Element: " + element.text());
}

Farklı tag veya attribute bazlı seçiciler kullanın:

Örneğin, "<div class="code-container">" gibi bir yapı varsa:


doc.select("div.code-container");
---

## Lisans

Bu proje MIT Lisansı altında lisanslanmıştır. Daha fazla bilgi için [LICENSE](LICENSE) dosyasına bakın.
