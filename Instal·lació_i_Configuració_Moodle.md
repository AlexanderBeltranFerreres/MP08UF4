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
CREATE USER 'ubuntumoodle'@'localhost' IDENTIFIED BY 'ubuntumoodle';
```
![image](https://user-images.githubusercontent.com/114162412/205097392-24de7cd9-3466-46f6-b968-2e994df95f7f.png)


Ara hem de crear una base de dades per al Moodle amb:

```sh
CREATE DATABASE moodle;
```

![image](https://user-images.githubusercontent.com/114162412/205097073-36f80b93-65d3-44b9-b575-b107055df4ea.png)



Finalment donem permisos amb:
```sh
GRANT ALL PRIVILEGES ON moodle.* TO 'ubuntumoodle'@'localhost';
FLUSH PRIVILEGES;
```
![Selecció_612](https://user-images.githubusercontent.com/114162412/205098325-2e9cbade-be1c-402f-b566-e685f93497d6.png)



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

Ara instal·lem el php-zip 

![image](https://user-images.githubusercontent.com/114162412/205079065-1056f4dc-ce2f-4144-aa56-9e75edca2222.png)

Un cop instal·lat fem un reload i ja ens anirà.


![image](https://user-images.githubusercontent.com/114162412/205079524-9ef096bc-c12a-4a99-a240-b4f3a8ae05b3.png)



![image](https://user-images.githubusercontent.com/114162412/205079660-10410ca1-14db-4b78-8595-791af0008ccf.png)



Ara ens preguntara on volem guardar les dades, en aquest cas fiquem que es guarden a /home/moodle_data que l'hem creat antes.

![image](https://user-images.githubusercontent.com/114162412/205081333-fd8d70aa-d063-4879-b5bf-2bfde7b4a0d7.png)


Ens preguntá quin controlador de base de dades utilitzem, en aquest cas es Maria DB però si no estem segurs podem saber-ho ficant la seguent comanda a la terminal:
```sh
sudo mysql -v
```

![image](https://user-images.githubusercontent.com/114162412/205082231-5a282f4d-8664-4771-a3e4-4146776ae139.png)

En aquesta pàgina hem de ficar els caps que ens demanen.

![image](https://user-images.githubusercontent.com/114162412/205085162-38af7402-8d58-4d58-8a1e-c4f93b1b12d6.png)


Ara ens sortirà un error dient-mos que ens falta l'extensió mbstring de PHP.


![Selecció_613](https://user-images.githubusercontent.com/114162412/205100275-c5a0d59f-4e03-4948-85b1-fc7fcbca6ec2.png)


Anem a la terminal i buquem el que ens hem d'instal·lar.

![image](https://user-images.githubusercontent.com/114162412/205100209-45dc3fce-b482-43e8-96c7-edeedf2937d9.png)



busquem el paquet que ens volem instal·lar segons la versió de php i l'intal·lem.
```sh
sudo apt search php | grep mbstring
```

```sh
sudo apt install php7.3-mbstring
```

![image](https://user-images.githubusercontent.com/114162412/205100754-38c546f6-4d99-4f19-a298-7265d79820ef.png)

Fem un reload i ja ens hauia de sortir.
```sh
sudo service apache2 reload
```

![Selecció_616](https://user-images.githubusercontent.com/114162412/205102005-ec0c5605-6edd-4672-9b44-4667fb38930f.png)

Fiquem que acceptem els "terminos y condiciones" i ens farà unes comprovacións automàtiques. 
en aquest cas ens ha marcat revisar dos extencions que no tenim instal·lades aixì que les instal·lem les dos i fem un reload del servei.

![image](https://user-images.githubusercontent.com/114162412/205102566-ee09e438-9abc-4fc4-ba7d-7b411e6375a8.png)

```sh
sudo apt search php | grep xmlrpc
```
```sh
sudo apt install php7.3-xmlrpc
```

![Selecció_618](https://user-images.githubusercontent.com/114162412/205107094-8ff12067-bd4c-43c0-8afa-25f36473b484.png)


```sh
sudo apt search php | grep soap
```
```sh
sudo apt install php7.3-soap
```

![image](https://user-images.githubusercontent.com/114162412/205107398-cdc49a93-4030-422a-81bc-9f1425a6e18e.png)

Ara fem un reload i ja estaria.

![image](https://user-images.githubusercontent.com/114162412/205112625-d17d086b-b02e-4ebb-8512-e3ad208b4585.png)

Quan hagui fet totes les comprovacións necessàries ens sortirà aquesta pantalla on hem de posar l'informació general del administrador.

![image](https://user-images.githubusercontent.com/114162412/207886892-4342db86-dfa6-40d0-b7e8-43f9e491ec59.png)

Ho emplenem i fiquem update profile.

![image](https://user-images.githubusercontent.com/114162412/207887657-d2e80c98-f7b0-4020-af8d-021057fe3b2b.png)

![image](https://user-images.githubusercontent.com/114162412/207889128-64135954-a50b-48f0-bb96-89a36a5fb709.png)

### CREACIÓ DE CURSOS

Pimer hem de crear categoies i subcategories, per fer això hem de modificar la categorìa que ve per defecte i ficar el nom que volem.

![image](https://user-images.githubusercontent.com/114162412/207895820-eb416e2a-8e74-4dc4-9bc9-79aaffa16f8e.png)

Un cop tenim la categoria que tenim podem crear subcategories dintre s'aquesta i dintre de les subgategories també podem crear subcategories.

![image](https://user-images.githubusercontent.com/114162412/207896108-b8a54fea-725a-4715-a826-4f47a8e08944.png)

La pantalla que ens sortirà és igual que la de modificar la categorìa.
Fem tantes subcategorìes com vulguem, en aquest cas ha quedat aixì.


![Selecció_769](https://user-images.githubusercontent.com/114162412/207899326-33b39249-ca4b-4514-8514-dce638028c50.png)

![image](https://user-images.githubusercontent.com/114162412/207899415-344df872-d5e0-43f9-8887-857aecfcebb8.png)

Un cop creades les subcategorìes ja podem crear els cursos que volem, en aquest cas he creat 3 cursos dintre de CFGM SMX/2n curs i 4 cursos dintre de la subcategorìa  CFGM SMX/2n curs/ Grup B.
Per crear els cursos simplement hem de entrar dintre de la subcategoria i fem clic a add new course, ens sortirà aquesta pantalla on fiquem les dades ue volem.
![image](https://user-images.githubusercontent.com/114162412/207901766-374a3a86-6c1c-45fb-abdb-6efb0109fb4a.png)

Fem això tantes vegades com necessitem dintre de les subcategories que vulguem.
Aqui és poden veure els cursos creats dintre de les categories pertinents.

![Selecció_773](https://user-images.githubusercontent.com/114162412/207906873-86b061cc-1d68-49cc-8c37-bede82887cc3.png)


### TIPUS D'USUARIS

1.Quins tipus d'usuaris (rols diferents) existeixen a Moodle?
  Ordinaris, professors, creadors, administradors.
2.Quins rols poden assignar cada tipus d'usuaris?
  
3.Crea tres usuaris:
  ·Un Professor anomenat Cognom_1_profe.
  ·Un Professor anomenat Cognom_2_profe.
  ·Un professor No editor anomenat Cognom_3_profe.

