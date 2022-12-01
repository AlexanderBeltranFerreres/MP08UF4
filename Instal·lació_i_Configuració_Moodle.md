# INSTAL·LACIÓ I CONFIGURACIÓ MOODLE



### INSTAL·LACIÓ

Ho hem de fer desde una màquina virtual d'ubutu server.
Per poder treballar amb la màquina més còmode i per evitar problemes fem la seguent comanda aixi treballarem desde la terminal de la màquina real.

```sh
ssh nom@IP
```
Ara ens hem d'instal·lar el moodle 

![](moodle.png)

Ara ens instal·lem el Apache2.
```sh
sudo apt-get install apache2
```
![](apache2.png)

Un cop tinguem el apache hem d'instal·lar MariaDB.
```sh
sudo apt-get install mariadb-server
```
![](MariaDB.png)

Quan s'ens hagui instal·lat hem de configurar MariaDB.

![](config.png)

Finalment ens instal·lem una versio de PHP compatible amb el moodle.
```sh
sudo aptitude install php7.3 php7.3-mysql php7.3-intl php7.3-curl php7.3-xml php7.3-gd
```
![](php.png)

Hem de canviar el fixer dir.conf per donar prioritat al php amb la comanda:
```sh
sudo nano /etc/apache2/mods-enabled/dir.conf
```
Ha de quedar així.

![](dir.conf.png)

Ara el descomprimim el .zip del moodle que ens hem intal·lar al principi. L'hem d'extrèure al directori /var/www/html per fer-lo accessible via web.

```sh
sudo unzip moodle-4.0.5.zip -d /var/www/html/
```
![](unzip.png)


Hem de canviar les propietat per a que pugui ser escrit pel servidor web.

```sh
sudo chown www-data:www-data /var/www/html/moodle
```

Ara hem de crear un directori de fitxers per a que moodle pugui guardar els seus fitxers, el nom que li he ficat al directori és moodle_data.

![](mkdir.png)

Configurar base de dades per a Moodle

Primer hem d'accedir a la base de dades amb:

```sh
mysql -u root -p
```

Un cop dintre hem de crear un usuari per al moodle

```sh 
CREATE USER 'ubuntumoodle'@'192.168.203.227' IDENTIFIED BY 'ubuntumoodle';
```
![](moodleuser.png)

Ara hem de crear una base de dades per al Moodle amb:

```sh
CREATE DATABASE moodle;
```

![](newDB.png)


Finalment donem permisos amb:
```sh
GRANT ALL PRIVILEGES ON moodle.* TO 'ubuntumoodle'@'192.168.203.227';
FLUSH PRIVILEGES;
```
![](flush.png)



Si ho hem fet tot bé ens hauria de sortir així quan fiquem la ip/moodle al buscador.
![](comp.png)


### CONFIGURACIÓ

Un cop dintre el primer que hem de fer és seleccionar d'idioma d'intal·lació.
![image](https://user-images.githubusercontent.com/114162412/205076721-c77717b7-4834-4579-8aba-ed7863666ed5.png)

Ens dirá que falla el modul zip del PHP.
![image](https://user-images.githubusercontent.com/114162412/205077052-b6ddc022-075c-4181-aa9d-4c417685c2e2.png)

Entrem a la terminal i dinte de la carpeta del moodle busquem al PHP el paquet zip amb la comanda:
```sh
sudo apt search php | grep zip
```

Ens mostra tots els paquets que estàn disponibles.
![image](https://user-images.githubusercontent.com/114162412/205078037-93539a46-ab5a-4180-81b3-79ed1cfa2847.png)

Ara intal·lem el php-zip 
![image](https://user-images.githubusercontent.com/114162412/205079065-1056f4dc-ce2f-4144-aa56-9e75edca2222.png)

Un cop instal·lat fem un reload i ja ens anirà.
![image](https://user-images.githubusercontent.com/114162412/205079524-9ef096bc-c12a-4a99-a240-b4f3a8ae05b3.png)
![image](https://user-images.githubusercontent.com/114162412/205079660-10410ca1-14db-4b78-8595-791af0008ccf.png)





