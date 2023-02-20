# Ödev 2

| Grup Üyeleri    | Katkıları | Github | 
|-----------------|-----------|--------|
| Osman Büyükşar  | Node'ların kodlanması ,  RPL , Client-Server , NETSTACK_ROUTİNG , callback function,    | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/OsmanBuyuksar) |   
| Kadir Emre Özer | Dökümantasyon araştırılması , Algoritmanın tasarımı , client-server | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/logaritmabir)  |  
| Yasin Ünal      | Dökümantasyon oluşturma, Proje yönetim , client-server , ara düğümler  | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/Pilestin) |   


## Amaç : 

Bu ödev kapsamında amacımız ağ içerisinde son düğüme bir paket gönderip , bu düğümden yapılan  kontrole göre yeniden farklı paket almaktı.

![image](https://user-images.githubusercontent.com/56133248/208489798-9a885f72-494c-42b8-bb8c-97784bbff689.png)


Şekilde de görüldğü gibi ilk olarak TURN_ON paketi ara düğümler vasıtasıyla son düğüme iletilir.

Son düğümde bu paketin içeriği kontrol edilir ve istenen paket olup olmadığı anlaşılır.

Eğer doğru paket ise bu düğüm tekrar ilk düğüme bilgimesajını iletir. 
Aksi halde yanlış olduğuna dair bilgi mesajı iletir.

Son olarak ilk paket gönderilen bu paketi kontrol eder ve doğru-yanlış olduğuna bakar.
Buna göre karar verip konsola sonucu yazdırır.

