# Ödev 3 - RPL Algoritmasının Farklı Link Kriterlerine Göre Çalıştırılması

<!-- 

| Grup Üyeleri    | Katkıları | Github | 
|-----------------|-----------|--------|
| Yasin Ünal      | ? | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/Pilestin) | 
| Osman Büyükşar  | ? | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/OsmanBuyuksar) | 
| Kadir Emre Özer | ? | [<image style="width:30px" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png">](https://github.com/logaritmabir)  | 
| Berkin Aker      | -  |    |   
| Muharrem Sevimli | -  |    |   
| Furkan Kaymak    | -  |    |   

-->

  
  
## RPL Nedir

RPL IPv6 kullanan bir ağ yönlendirme protokolüdür. İsmi, “ripple” yani dalgalanmadan gelir. RPL veri yapısı olarak DAG veya DODAG yapısını kullanır. Ağın oluşumu kök düğüm tarafından başlatılır. 

Bu noktadan sonra root bazı mesajlar yayınlayarak ağın oluşturulmasını sağlar. Bu mesajlara DIO (DODAG Information Object ) adı verilir ve mesajı alan cihazlar ağa katılırlar. Sırasıyla yeni bir düğüm şu şekilde ağa katılır :  

- Ağa katılmamış olan bir düğüm DIS (DODAG Information Solicitation) mesajları yayınlayarak ağı tarar ve kendini duyurmaya çalışır. Ağdaki başka bir düğüme (veya köke) ulaşan bu mesaj , yeni bir DIO mesajı gönderilerek cihazın ağa tanıtılmasına sebep olur. Yeni düğüm gönderilen bu DIO’yu alır ve ağa katılır. 

- Bu aşamada yeni katılan düğüm ilk aşamada ebeveynini bulmaya çalışır. Bu noktada bağlantı kalitesine bakar ve hesaplama yapar. Ardından bulduğu ebeveyni onaylamak için DAO mesajı gönderir. Ebeveyn DAO mesajını alıp DAO-ACK mesajı ile karşılık gönderir ve bunu alan yeni düğüm bu noktadan sonra kendini ağın  bir parçası olarak görür.

## Senaryo-1 Algoritması ( Çalışma Mantığı ) 

Senaryo 1 için hop sayısına bakılması gerekiyor, bu da root’a olan uzaklığımız anlamına gelir. Bu bilginin elde edilmesi için düğümün öncelikle ağa katılması gerekir. Bunun için ağa bağlanacak düğüm DIS yayınlar, bu sayede çekim alanındaki düğümlerin zamanlayıcılarını yeniden başlatarak DIO alma olasılığını arttırır.

<div align="center">
  <img src="https://user-images.githubusercontent.com/56133248/220167080-970bdf7e-ec78-495c-a963-07571155e3cd.png">
</div>
<div align="left">
    DIO alındıktan sonra komşu düğüm ile DIO paketleri alıp- verilerek link-probing yani linkin kalitesi ölçülür. Ölçülen değerlere göre düğüm ağa   bağlanırken hangi düğümü parent seçeceğini belirler. Bir parent seçildiğinde, o parent’ın rank değerinin bir fazlası, bağlanan düğümün rankı olacak şekilde tanımlanır. Bu sayede ağa katılan her düğümde rank değişkeni root’a olan uzaklığı tutar. 
 </div>
  
  Senaryo-1 için DIO objesi alındığında rank’ın minimum olduğu düğüm seçilir. Bu sayede her düğüm ağa bağlanırken yol olarak root’a en kısa hop sayısı ile giden yolu seçer.
  
### Objective Function dosyası değiştirilenler 

1.senaryonun yapılması için sadece maliyet metriğinin değiştirilmesi yeterli, dolayısıyla rpl-of-minhop.c dosyasında mrhof.c dosyasından farklı olarak sadece nbr_link_metric() fonksiyonu ve nbr_path_cost() fonksiyonu değiştirilmiştir. Diğer fonksiyonların varsayılan halleri senaryoyu sağladığı için onlarda herhangi bir değişiklik yapılmamıştır.

nbr_link_metric() fonksiyonunda dönüş değeri; 

return nbr != NULL ? nbr->rank : 0xffff; 

olarak değiştirilmiştir bu sayede link metriğinde önemli olan tek faktör rank yani kökten olan uzaklık olacak hale getirilmiştir. 

nbr_path_cost() fonksiyonunda ise dönüş değeri sadece nbr_link_metric() fonksiyonunun dönüş değeri olacak şekilde değiştirilmiştir. Bu sayede herhangi bir fonksiyon yolun maliyetini hesapladığında sadece düğümün kökten olan uzaklığını baz alacaktır.

Yapılan değişikliklerden sonra oluşturulan objective function dosyasının işletim sistemine dahil edilmesi için contiki-ng/os/net/routing/rpl-lite/rpl-conf.h dosyasında 97.satıra #define RPL_SUPPORTED_OFS {&rpl-of- minhop} yazılmıştır.
  
## Senaryo-2 Algoritması ( Çalışma Mantığı )

Burada yeni bir of dosyası oluşturulmadı. mrhof.c içerisinde düzenlemeye yapmaya çalışıldı. * mrhof.c içerisinde rpl işlevlerinin ne anlama geldiği, rpl-types.h kütüphanesi içerisinde 150.satırda başlayan yorum satırlarından öğrenildi. 

- nbr_link_metric() işlevi içerisinde kullanılan stats→etx yerine stats→rssi denemesi yapıldı. 

- Daha sonra nbr_path_cost() işlevinin içerisinde switch(curr_instance.mc.type) seçimi içerisine case RPL_DAG_MC_LQL: denenmeye çalışıldı fakat metric_object değişkeni enerji ve etx tabanlı olduğu için burada bağlantı kalitesine uygun bir “base” ataması yapılamadı.

- mrhof.c ‘nin kendisinden tanımlı within_hysteresis() işlevi gecikmesine göre bir best_parent() içerisinde seçim yaptığı için within_hysteresis() işlevi olmadan,bağlantı sinyaline dayalı bir ebeveyn tayini yapılmaya çalışıldı bu durum da hüsran. 


