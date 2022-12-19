## Amaç : 

Bu ödev kapsamında amacımız ağ içerisinde son düğüme bir paket gönderip , bu düğümden yapılan  kontrole göre yeniden farklı paket almaktı.

![image](https://user-images.githubusercontent.com/56133248/208489798-9a885f72-494c-42b8-bb8c-97784bbff689.png)


Şekilde de görüldğü gibi ilk olarak TURN_ON paketi ara düğümler vasıtasıyla son düğüme iletilir.

Son düğümde bu paketin içeriği kontrol edilir ve istenen paket olup olmadığı anlaşılır.

Eğer doğru paket ise bu düğüm tekrar ilk düğüme bilgimesajını iletir. 
Aksi halde yanlış olduğuna dair bilgi mesajı iletir.

Son olarak ilk paket gönderilen bu paketi kontrol eder ve doğru-yanlış olduğuna bakar.
Buna göre karar verip konsola sonucu yazdırır.

