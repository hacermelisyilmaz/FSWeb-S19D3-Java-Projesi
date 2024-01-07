# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	
        INSERT INTO yazar(yazarad, yazarsoyad) VALUES("Kemal", "Uyumaz")
	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	
        INSERT INTO tur(turadi) VALUES("Biyografi")
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 
	
        INSERT INTO ogrenci(ogrno, ograd, ogrsoyad, cinsiyet, sinif) VALUES (162, "Çağlar", "Üzümcü", "E", "10A"), (163, "Leyla", "Alagöz", "K", "9B"), (164, "Ayşe", "Bektaş", "K", "11C")
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
	
        INSERT INTO yazar(yazarad, yazarsoyad) SELECT ograd, ogrsoyad FROM ogrenci ORDER BY RAND() LIMIT 1
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
	
        INSERT INTO yazar(yazarad, yazarsoyad) SELECT ograd, ogrsoyad FROM ogrenci WHERE ogrno >= 10 AND ogrno <= 30 
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	
        INSERT INTO yazar (yazarad, yazarsoyad) VALUES ('Nurettin', 'Belek');
        DECLARE @InsertedID INT;
        SET @InsertedID = @@IDENTITY;

        INSERT INTO yazar (yazarad, yazarsoyad) VALUES ("Nurettin", "Belek") SELECT @IDENTITY AS yazarno // ??
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
	
        UPDATE ogrenci SET sinif = "10C" WHERE ogrno = 3
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
	
        UPDATE ogrenci SET sinif = "10A" WHERE sinif = "9A"
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
	
        UPDATE ogrenci SET puan = puan + 5
	
	10) 25 numaralı yazarı silin.

        DELETE FROM yazar WHERE yazarno = 25

	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
	
        SELECT * FROM ogrenci WHERE dtarih IS NULL
	
	12) Doğum tarihi null olan öğrencileri silin. 
	
        DELETE FROM ogrenci WHERE dtarih IS NULL
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
	
        UPDATE kitap SET puan = puan + 2 WHERE kitapadi LIKE "a%"
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
	
        INSERT INTO tur (turadi) VALUES ("Kişisel Gelişim")
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
	
        UPDATE kitap SET turno = (SELECT turno FROM tur WHERE turadi = "Kişisel Gelişim") WHERE kitapadi = "Başarı Rehberi"
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
	
        CREATE OR REPLACE PROCEDURE ogrencilistesi LANGUAGE "sql" AS $BODY$
            SELECT * FROM ogrenci
        $BODY$
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
	
        CREATE OR REPLACE PROCEDURE ekle(
            IN ograd character varying, 
            IN ogrsoyad character varying, 
            IN cinsiyet character varying, 
            IN sinif character varying, 
            IN dtarih character varying, 
            IN grade integer) 
        LANGUAGE "sql" AS ogr 
        AS $BODY$
            INSERT INTO ogrenci
            VALUES (ogr.ograd, ogr.ogrsoyad, ogr.cinsiyet, ogr.sinif, ogr.dtarih, ogr.grade)
        $BODY$
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
	
        CREATE OR REPLACE PROCEDURE sil(IN ogrencino integer) 
        LANGUAGE "sql" AS $BODY$
            DELETE FROM ogrenci WHERE ogrno = ogrencino
        $BODY$
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
	
        CREATE OR REPLACE PROCEDURE sinifGuncelle(IN ogrencino integer, IN ogrencisinif character varying) 
        LANGUAGE "sql" AS $BODY$
            UPDATE ogrenci SET sinif = ogrencisinif WHERE ogrno = ogrencisinif
        $BODY$
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
	
        CREATE OR REPLACE PROCEDURE ogrenciAra(IN tamad character varying) 
        LANGUAGE "sql" AS $BODY$
            SELECT * FROM ogrenci WHERE CONCAT(ograd, " ", ogrsoyad) LIKE CONCAT("%", tamad, "%")
        $BODY$
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
	
        DROP PROCEDURE IF EXIST "ogrencilistesi";
        DROP PROCEDURE IF EXIST "sil";
        DROP PROCEDURE IF EXIST "ekle";
        DROP PROCEDURE IF EXIST "sinifGuncelle";
        DROP PROCEDURE IF EXIST "ogrenciAra";
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
	
        SELECT * FROM kitap WHERE turno = (SELECT turno FROM tur WHERE turadi = "DRAM")
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	
        SELECT * FROM kitap WHERE yazarno IN (SELECT yazarno FROM yazar WHERE yazarad LIKE "e%")
	
	24) Kitap okumayan öğrencileri listeleyiniz.
	
        SELECT * FROM ogrenci WHERE ogrno NOT IN (SELECT * FROM islem)
	
	25) Okunmayan kitapları listeleyiniz

        SELECT * FROM kitap WHERE kitapno NOT IN (SELECT * FROM islem)
	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.

        SELECT * FROM kitap WHERE kitapno NOT IN (SELECT kitapno FROM islem 
        WHERE EXTRACT (MONTH FROM atarih::timestamp) = 5)
        AND EXTRACT (MONTH FROM vtarih::timestamp) = 5)