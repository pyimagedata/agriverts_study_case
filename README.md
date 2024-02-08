# agriverts_study_case
Bu çalışma, yapay zeka algoritmaları kullanılarak marul bitkilerinin tespit edilmesi ve boyutlarının belirlenmesi üzerine odaklanmaktadır. Model, bir web sunucusuna yüklenerek kullanıcı arayüzü ve API ile erişilebilir hale getirilmesı amaçlanmıştır. Bu hedefler doğrulturundan aşağıdaki adımlar gerçekleştirilmiştir.

## 1. Veri Seti Hazırlama
Gönderilen proje dosyasında gerekli veri seti hazır olarak bukunmaktadır. Bundan dolayı yeni bir veri seti hazırlanmamıştır. Veri seti içerisinde gerekli görseller ve bu görsellere ait etiketler yer almaktadır. Veri setinde toplam 747 adet görsel ve 747 adet etiket dolasyası bulunmaktadır. Bu görsellerden 518(%70) tanesi eğitim, 112(%15) tanesi doğrulama ve kalan 112(%15) tanesi ise test için ayrılmıştır.

## 2. Modelin Eğitilmesi
Yapay zeka modelinin eğitimi için YOLO algoritması seçilmiştir. Bu algoritma diğerlerine göre daha yüksek doğruluğa ve daha hızlı tespki süresine sahiptir. Bundan dolayı bu çalışma için en uygum algoritmadır. Eğitim işlemi toplamda 500 iterasyon kullanılarak tamamlanmıştır. Bu eğitim sonucunda aşağıdaki veriler elde edilmiştir.

<img src="https://github.com/pyimagedata/agriverts_study_case/assets/67453649/592dbc0f-e6b0-4ff1-932c-cd9806c88639" width="500" />
<img src="https://github.com/pyimagedata/agriverts_study_case/assets/67453649/8f217ddf-90aa-46af-b5eb-96f626610c5e" width="500" />
<img src="https://github.com/pyimagedata/agriverts_study_case/assets/67453649/f6520247-291b-424a-9eb0-ca85d41a79ad" width="600" />

<img src="https://github.com/pyimagedata/agriverts_study_case/assets/67453649/1efbd1e1-724a-43ae-a3a9-0cc027d79070" width="1200" />
<img src="https://github.com/pyimagedata/agriverts_study_case/assets/67453649/8d575821-fa07-4fad-8a24-42efa69c03e5" width="1200" />

## Web ve API Uygulaması
Bu yapay zeka modelinin bir web sunucusuna yüklenmesi ve bir kullanıcı arayüzü ve api özelliğini olması istenmiştir. Bu amaç doğrultusunda Python programlama dili ile geliştirilmiş olan Django framework ile geliştirme işlemi tamamlanmıştır. Django framework hem gelişmiş web uygulamalrı hemde API geliştiemk için en iyi seçeneklerden birisidir. İlk olarak localde bir django uygulaması geliştirilmiş ve yapay zaka modeli bu uygualamaya entegre edilmiştir. Bu web uygulamasında kullanıcı dosya konumundan herhangi bir görseli uyfulamaya yükledikten sonra tespit işlemini gerçekleştirip veriyi sunucuya gönderir. Sunucuda tespit işlemi yapılarak görsel ek bilgilerle istek cevaplanır. Arayüz kısmında direkt görsel gönderilebilmektedir. 

API kısmında ise direkt görsel göndermek yerine belirlenen görsel ilk olarak bytes formatına dönüştürülerek sunucuya gönderilir. Tespit işlemi yapılıp ek bilgilerle tekrardan görsel bytes formatına dönüştürülerek kullanıya verileri döndürür. Bu API uygulaması şuan için temel seviyede oluşturulmuştur. 

Localde geliştirilen bu web uygulaması pythonanywhere.com sunucusu üzerinde deploy edilmiş ve online olarak herkesin erişimine açık hale getirilmiştir. https://pyimagedata.pythonanywhere.com/ bu web adresi üzerinden uygulamaya online olarak erişilebilmektedir.

![image](https://github.com/pyimagedata/agriverts_study_case/assets/67453649/5b2e3656-7723-4da7-be47-85c1d39882ab)

Aşağıdaki kod ile API tarafına görsel gönderilip tespit işlemi yapılarak json formastında veriler alınmaktadır.

<pre><code>
import requests
import base64
from PIL import Image
from io import BytesIO
import json
import io


# Path to the image file
image_path = '2.jpg'

# Open the image file
with open(image_path, 'rb') as image_file:
    # Convert the image to base64
    image_base64 = base64.b64encode(image_file.read()).decode('utf-8')

# API endpoint URL
url = 'http://pyimagedata.pythonanywhere.com/api/object-recognition/'

# Data to be sent in the request
data = {'image': image_base64}

# Send a POST request to the API endpoint
response = requests.post(url, data=data)

if response.status_code == 200:
    try:
        # Try to parse the JSON response
        result_json = response.json()

        image_data = base64.b64decode(result_json["image"])

        image_buffer = BytesIO(image_data)

        # Open the image using PIL
        im = Image.open(image_buffer)
        im.show()  # Show the image (you can remove or modify this line)
    except requests.exceptions.JSONDecodeError:
        print("Empty or invalid JSON response.")
else:
    # Print the raw response content for debugging
    print(f"Request failed with status code {response.status_code}.")

</code></pre>
