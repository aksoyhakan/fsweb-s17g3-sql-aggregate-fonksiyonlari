# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
SELECT O.ograd, O.ogrsoyad, I.atarih FROM ogrenci AS O, islem AS I WHERE O.ogrno=I.ogrno; 2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
SELECT K.kitapadi, T.turadi FROM kitap as K, tur as T WHERE K.turno=T.turno; 3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
SELECT O.ogrno, O.ograd, O.ogrsoyad, K.kitapadi FROM ogrenci AS O, islem AS T, kitap AS K WHERE T.ogrno=O.ogrno and (sinif IN ("10B","10C"));
#join ile yazın 4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
SELECT O.ograd, O.ogrsoyad, I.atarih FROM ogrenci AS O LEFT JOIN islem AS I ON O.ogrno=I.ogrno; 5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
SELECT K.kitapadi, T.turadi FROM kitap as K LEFT JOIN tur as T ON K.turno=T.turno; 6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
SELECT O.ogrno, O.ograd, O.ogrsoyad, K.kitapadi FROM ogrenci AS O LEFT JOIN islem AS I ON I.ogrno=O.ogrno LEFT JOIN kitap AS K ON I.kitapno=K.kitapno WHERE (sinif IN ("10B","10C")); 7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
SELECT O.ograd, O.ogrsoyad, I.atarih FROM ogrenci AS O LEFT JOIN islem AS I ON O.ogrno=I.ogrno; 8) Kitap almayan öğrencileri listeleyin.
SELECT O.ograd, O.ogrsoyad FROM ogrenci AS O LEFT JOIN islem AS I ON O.ogrno=I.ogrno WHERE ISNULL(I.atarih); 9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
SELECT K.kitapno,K.kitapadi,COUNT(I.kitapno) AS "Kitap Alınma Sayısı" FROM islem AS I LEFT JOIN kitap AS K ON I.kitapno=K.kitapno GROUP BY K.kitapno; 10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT K.kitapno, K.kitapadi,COUNT(I.kitapno) AS "Kitap Alınma Sayısı" FROM kitap AS K LEFT JOIN islem AS I ON K.kitapno=I.kitapno GROUP BY K.kitapno;
    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    select o.ogrno, ograd, ogrsoyad, kitapadi from ogrenci as o
    inner join islem as i on o.ogrno=i.ogrno
    inner join kitap as k on i.kitapno=k.kitapno
    order by ograd;
    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
    select o.ogrno, ograd, ogrsoyad, kitapadi,turadi from ogrenci as o
    inner join islem as i on o.ogrno=i.ogrno
    inner join kitap as k on i.kitapno=k.kitapno
    left join tur as t on t.turno=k.turno
    order by ograd;

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
    select ograd, ogrsoyad, count(islemno) "okuduğu kitap sayısı" from ogrenci as o
    inner join islem as i on o.ogrno=i.ogrno
    where sinif in("10A","10B")
    group by o.ogrno
    order by ograd;

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG
    SELECT AVG (sayfasayisi) FROM kitap;
    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
    SELECT * FROM kitap WHERE sayfasayisi>(SELECT AVG (sayfasayisi) FROM kitap);

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    SELECT COUNT(ogrno) FROM ogrenci;
    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    select count(ogrno) as "Toplam Sayı" from ogrenci;
    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    select distinct ograd from ogrenci;
    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    select max(sayfasayisi) from kitap;

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    select kitapadi,sayfasayisi from kitap where sayfasayisi=(select max(sayfasayisi) from kitap);

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    select min(sayfasayisi) from kitap;

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    select kitapadi,sayfasayisi from kitap where sayfasayisi=(select min(sayfasayisi) from kitap);


    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
    select max(sayfasayisi),kitapadi from kitap as k left join tur as t on t.turno=k.turno where t.turadi="Dram";

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
    select sum(sayfasayisi) as "Öğrenci Toplam Sayfa Sayısı", o.ogrno from ogrenci as o
    left join islem as i on o.ogrno=i.ogrno
    left join kitap as k on i.kitapno=k.kitapno
    where o.ogrno=15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
    select ograd, count(ograd) as adet from ogrenci group by ograd order by adet desc;

    26) Her sınıftaki öğrenci sayısını bulunuz.
    select sinif, count(sinif) as sayi from ogrenci group by sinif order by sayi desc;

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
    select sinif, cinsiyet, count(cinsiyet) as sayi from ogrenci group by sinif,cinsiyet order by sinif;

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
    select ograd, ogrsoyad, sum(sayfasayisi) as "okuduğu toplam sayfa sayısı" from ogrenci as o
    inner join islem as i on o.ogrno=i.ogrno
    left join kitap as k on i.kitapno=k.kitapno
    group by o.ogrno
    order by "okuduğu toplam sayfa sayısı" desc;

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.
    select ograd, ogrsoyad, count(kitapadi) as "okuduğu kitap sayısı" from ogrenci as o
    inner join islem as i on o.ogrno=i.ogrno
    left join kitap as k on i.kitapno=k.kitapno
    group by o.ogrno
    order by ograd;
