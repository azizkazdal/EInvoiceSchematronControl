# EInvoiceSchematronControl
E-Fatura Şematron Kontrol

Nasıl yapacağımı uzun zaman aradığım ve mantığını kafamda ilk anlarda oturmakta zorlandığım e-fatura ve diğer e-belgeler için şematron kontrolünün nasıl yapılabileceğine dair ipuçlarını ve kod örneklerini bulabilirsiniz.

Her nedense şematron kontrolünün nasıl yapılabileceğine dair bilgi paylaşımı ne yazıkki fazla yok. Özellikle NET CORE (yeni adıyla NET) ortamında geliştirme yapanlar için XML şematron kontrolü epeyce sıkıntılı olabiliyor. 

.NET XSLT versiyonu olarak sadece XSLT v1 i desteklediğini aklımızdan çıkarmamak gerek :) Meselenin en kritik noktası burası.

Bir e-fatura ya da diğer e-belgelere yapılacak şematron kontrolü, XSLT tasarım dosyası yaparak e-belge XML'nin HTML olarak ekranda gösterilmesi gibi çalışır. Elinizde e-belge XML dosyasını GIB'in belirlediği şematron kontrol kurallarını içeren bir XSLT dosyası olması gerekir.

Kritik problem ise XML kontrollerinin sağlanacağı XSLT dosyasının nasıl oluşturulacağıdır. .NET XSLT v1 desteklediğin için oluşturulacak XSLT dosyası v1 fonksiyonlarına sahip olmalı aksi halde şematron kontrolü için kullanılacak XSLT dosyası .NET ortamı (harici bir paket kullanılmadığı varsayımıyla) için işe yaramayacaktır.

Öncelikle GIB tarafından yayınlanan XML şematron kontrolleri için kuralları içeren UBL-TR_Main_Schematron, UBL-TR_Common_Schematron, UBL-TR_Codelist dosya içerikleri tek bir dosya içerisine toplanmalı. UBL-TR_Main_Schematron için de farklı e-belge tiplerine ait kuralların tamamı bulunmakta. Tavsiyem e-fatura, e-irsaliye gibi bu dosya içerisindeki kuralları ayrı ayrı XML kural dosyaları oluşturmanız. Daha sonra ilgili e-belgeleri şematron kontrolünden geçirirken daha rahat olacağını düşünüyorum.

Kuralları tek dosya içerisinde topladıktan sonra sırada bu kuralları kullanarak şematron kontrolünü yapacağımız XSLT dosyasını oluşturmamız var. XSLT formatında kural dosyasını oluşturmak için bir başka XSLT dosyasından (https://github.com/Schematron/schematron/tree/master/trunk/schematron/code adresindeki iso_schematron_skeleton_for_xslt1.xsl) yararlanacağız. Burada ki mantık iso_schematron_skeleton_for_xslt1.xsl (XSLT v1 formatında XSLT dosyasını oluşturur) XSLT dosyası kullanılarak UBL-TR_Main_Schematron, UBL-TR_Common_Schematron, UBL-TR_Codelist XML dosyalarından elde edilen ve tek dosya içerisinde toplanmış kural setlerini şematron kontrolünde kullanılabilecek hale getirmek. Evet XSLT kullanarak XML dosyayı transform edeceğiz ve elde ettiğimiz XSLT dosyayı şematron kontrolünden geçireceğimiz e-belgeler için kullacağız. :)

Transfrom işlemi için birden fazla yöntem var. Oxygen xml gibi uygulamaları kullanarak bu transform işlemi yapılabileceği gibi .NET konsol uygulaması kullanılarak şematron kontrol XSLT dosyası elde edilebilir. 

Burada ki çevrim NET 5 konsol uygulaması kullanılarak gerçekleştirilmiştir. XslCompiledTransform API kullanılarak ISO şematron dosyası ile GIB XML kuralları dosyası transform edilir.

String olarak elde edilen XSLT formatındaki metin ayrı bir dosya olarak kaydedilir. 

Ve artık işlem bir kaç ufak ayrıntıda halletiniz mi şematron kontrolü için kullanacağınız XSLT dosyanız hazır. 

XML kurallarını ISO şematron dönüşüm dosyası ile elde ettiğiniz XSLT dosyası içerisinde bir kaç .NET tarafından desteklenmeyen fonksiyon (matches, exist gibi) geliyor. Bunları XSLT dosyanız içerisinden kaldırdığınız da artık şematron kontrol dosyanız hazır.
