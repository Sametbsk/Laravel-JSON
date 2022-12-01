## Laravel ile JSON


[Laravel özelliklerini](https://laravel.com/docs/9.x/eloquent-serialization#serializing-to-json) kullanan json işlemleri<br><br>
Laravelde verileri json a dönüştürmek için `toJson` kullanabiliriz

    return $user->toJson();
___

#### JSON dan veri gizleme

Şifreler gibi görünmesini istemeyeceğimiz bilgileri json dosyasından gizleyebiliriz. Bunun için models kısmına `$hidden` özelliğini ekleriz<br>

    protected $hidden = ['password'];

Aynı işlemi farklı biçimde yapmak da mümkün<br>
Yine models kısmında `$visible` kullanarak izin listesi oluşturabiliriz.Sadece buradaki veriler json a eklenecektir.

    protected $visible = ['first_name', 'last_name'];
    
Genelde görünür olarak kullanılan veriyi gizlemek için`makeHidden` tersi için `makeVisible`kullanılır.

    return $user->makeHidden('attribute')->toArray();
    
___
#### JSON a veri ekleme

Dizileri json a dönüştürürken veritabanında sütunu olmayan veriler eklemek isteyebilirsiniz<br>?<br>?<br>?





## Dizileri JSON'a dönüştürme(JSON encode)
___

    $araclar = array('klavye','mouse','monitör');  // veya array(0 =>'klavye',1=>'mouse',2=>'monitör');
    echo json_encode($araclar);
    
Çıktısı şu şekilde olur<br>
[ “klavye”, ”mouse”, ”monitör” ]
___
    
    $ulasim = array('karayolu' => 'araba', 'havayolu' => 'uçak', 'deniz' => 'gemi');
    echo json_encode($ulasim);

Çıktı şu şekilde olur<br>
{ “ karayolu ”: ” araba ”, ” havayolu ”: ” uçak ”, ” deniz ”, ” gemi ”,” iki kelime ”, ” bisiklet ”}
___
## JSON'dan veri okuma(JSON decode)

`json_decode()` işlevi ikinci parametreyi bir boole olarak alır. true verirsek ilişkilendirilebilir dizi verir

    $kisiler = '{"Ahmet":22,"Büşra":25,"İrem":20}';

    $data = json_decode($kisiler, true);
    print_r($data);
    
Çıktı şu şekilde olur<br>
Array ( [Ahmet] => 35 [Büşra] => 37 [İrem] => 43 )
___
    $araclar = '["klavye","mouse","monitör"]';
    $araclar = json_decode($araclar);
    echo $araclar[0];  //  klavye
    echo $araclar[1];  //  mouse
    echo $araclar[2];  //  monitör
___
    $ulasım = '{"karayolu":"araba","havayolu":"uçak","deniz":"gemi","iki kelime":"bisiklet"}';
    $ulasım = json_decode($ulasım);
    
    foreach ($ulasım as $key => $value){
        echo $key.' => '.$value.'<br>';  
    }
    
Karayolu => araba<br>
Havayolu => uçak<br>
Deniz => gemi<br>
İki kelime => bisiklet
___
    $ulasım = '{"karayolu":"araba","havayolu":"uçak","deniz":"gemi","iki kelime":"bisiklet"}';
    $ulasım = json_decode($ulasım);
    
    echo $ulasım->karayolu;        //  araba
    echo $ulasım->havayolu;        //  uçak
    echo $ulasım->deniz;           //  gemi
    echo $ulasım->{'iki kelime'};  //  arasında boşluk olan kelimeler böyle yazılır
    
___
   
   İç içe verileri birleştirme
   
    $Ogrencidata = '[{
      "id":12,
      "isim":"Ali",
      "mail":"Alisefa@gmail.com",
    },
    
    {
       "id":13,
      "isim":"Mehmet",
      "mail":"Mehmetonal@gmail.com",
    }]';
    
    $data = json_decode($Ogrencidata);
    foreach($data as $Ogrenci) {
      foreach($Ogrenci as $mykey=>$myValue) {
          echo "$mykey - $myValue </br>";
       }
    }    
    
Çıktısı şu şekilde olur<br>

id - 12<br>
isim - Ali<br>
mail - alisefa@gmail.com<br>
id - 13<br>
isim - Mehmet<br>
mail - Mehmetonal@gmail.com

___

    $string = '{
       "area": [
           {
               "area": "php"
           },
           {
              "area": "laravel"
           }
           ]
    }';
    
Yukarıdaki JSON dosyasından sadece area altındaki verileri çekmek istiyoruz

               $area = json_decode($string, true);
               $area = $area['area'];
               
               foreach($area as $i => $v)
               {
                    echo $v['area'].'<br/>';
               }
               
Çıktı şu şekilde olur<br>
php<br>laravel

