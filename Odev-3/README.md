# Ödev 3 - RPL Algoritmasının Farklı Link Kriterlerine Göre Çalıştırılması


| Grup Üyeleri    | Katkıları | Github | 
|-----------------|-----------|--------|
| Yasin Ünal      | ? | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/Pilestin) | 
| Osman Büyükşar  | ? | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/OsmanBuyuksar) | 
| Kadir Emre Özer | ? | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/logaritmabir)  | 
| Berkin Aker      | -  |    |   
| Muharrem Sevimli | -  |    |   
| Furkan Kaymak    | -  |    |   
  
  
## RPL Nedir

RPL IPv6 kullanan bir ağ yönlendirme protokolüdür. İsmi, “ripple” yani dalgalanmadan gelir. RPL veri yapısı olarak DAG veya DODAG yapısını kullanır. Ağın oluşumu kök düğüm tarafından başlatılır. 

Bu noktadan sonra root bazı mesajlar yayınlayarak ağın oluşturulmasını sağlar. Bu mesajlara DIO (DODAG Information Object ) adı verilir ve mesajı alan cihazlar ağa katılırlar. Sırasıyla yeni bir düğüm şu şekilde ağa katılır :  

- Ağa katılmamış olan bir düğüm DIS (DODAG Information Solicitation) mesajları yayınlayarak ağı tarar ve kendini duyurmaya çalışır. Ağdaki başka bir düğüme (veya köke) ulaşan bu mesaj , yeni bir DIO mesajı gönderilerek cihazın ağa tanıtılmasına sebep olur. Yeni düğüm gönderilen bu DIO’yu alır ve ağa katılır. 

- Bu aşamada yeni katılan düğüm ilk aşamada ebeveynini bulmaya çalışır. Bu noktada bağlantı kalitesine bakar ve hesaplama yapar. Ardından bulduğu ebeveyni onaylamak için DAO mesajı gönderir. Ebeveyn DAO mesajını alıp DAO-ACK mesajı ile karşılık gönderir ve bunu alan yeni düğüm bu noktadan sonra kendini ağın  bir parçası olarak görür.



  
