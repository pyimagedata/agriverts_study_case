# agriverts_study_case
Bu çalışma, yapay zeka algoritmaları kullanılarak marul bitkilerinin tespit edilmesi ve boyutlarının belirlenmesi üzerine odaklanmaktadır. Model, bir web sunucusuna yüklenerek kullanıcı arayüzü ve API ile erişilebilir hale getirilmesı amaçlanmıştır. Bu hedefler doğrulturundan aşağıdaki adımlar gerçekleştirilmiştir.

## 1. Veri Seti Hazırlama
Gönderilen proje dosyasında gerekli veri seti hazır olarak bukunmaktadır. Bundan dolayı yeni bir veri seti hazırlanmamıştır. Veri seti içerisinde gerekli görseller ve bu görsellere ait etiketler yer almaktadır. Veri setinde toplam 747 adet görsel ve 747 adet etiket dolasyası bulunmaktadır. Bu görsellerden 518(%70) tanesi eğitim, 112(%15) tanesi doğrulama ve kalan 112(%15) tanesi ise test için ayrılmıştır.

## 2. Modelin Eğitilmesi
Yapay zeka modelinin eğitimi için YOLO algoritması seçilmiştir. Bu algoritma diğerlerine göre daha yüksek doğruluğa ve daha hızlı tespki süresine sahiptir. Bundan dolayı bu çalışma için en uygum algoritmadır. Eğitim işlemi toplamda 500 iterasyon kullanılarak tamamlanmıştır. Bu eğitim sonucunda aşağıdaki veriler elde edilmiştir.
![P_curve](https://github.com/pyimagedata/agriverts_study_case/assets/67453649/592dbc0f-e6b0-4ff1-932c-cd9806c88639)   ![R_curve](https://github.com/pyimagedata/agriverts_study_case/assets/67453649/8f217ddf-90aa-46af-b5eb-96f626610c5e)  ![F1_curve](https://github.com/pyimagedata/agriverts_study_case/assets/67453649/f6520247-291b-424a-9eb0-ca85d41a79ad)





Bitki tanıma için 2015 yılından beri geliştirilen ve açık kaynak kodlu YOLO algoritmasının v8 versiyonu kullanılmıştır. Bu YOLO olgoritması pythorch kütüphaesi ile geliştirilmektedir. Bu algoritmanın seçilmesinin sebebi uzun yıllardır geliştiriliyor olması diğer algoritmalardan farklı bir yapıya sahip. Bundan dolayı daha iyi daha hızlı çalışmaktadır. Algoritma birçok yapay zeka problemin çözüöünde kullanılmaktadır. Bu çalışmada ise nesne tanıma işlemi için kullanılmıştır. 

Proje dosyasında gerekli veri seti etiketlenmiş olarak gönderilmiş olup etiketleme işlemi çok uzun süreceği için tekrardan etiketleme işlemi yapılmadı. Veri seti ile gelen etiketler kullanılarak eğitim işlemi tamamlandı. Bu eğitim esnasında train ve val klasörlerindeki görseller kullanılarak eğitim işlemi tamamlandı. Test klasöründeki görsellerle ise test işlemi gerçekleştirildi. Oluşturulan model 640x480 boyutunda görseller üzerinde eğitilmiştir. 
