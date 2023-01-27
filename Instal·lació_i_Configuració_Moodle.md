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

  Gerente, creador de cursos, profesor, profesor no editor, alumno, húesped, usuario autentificado, usuario autentificado en hogar del sitio. 
  
2.Quins rols poden assignar cada tipus d'usuaris?
  
  El gerent pot assignar el rol de gerent, creador de cursos, professor, professior no editor i alumne.
  
  El professor pot assignar el rol de professor no editor i alumne. 
  
  Els tipus no poden assignar cap rol.
  
3.Crea tres usuaris:

  ·Un Professor anomenat Cognom_1_profe.
  
  ·Un Professor anomenat Cognom_2_profe.
  
  ·Un professor No editor anomenat Cognom_3_profe.
  
  

![image](https://user-images.githubusercontent.com/114162412/212105579-124766c1-ffa2-4377-98da-b43a69e2dac7.png)
 

![image](https://user-images.githubusercontent.com/114162412/212107241-80594b03-f18b-43ae-8ad9-bfcbbd8e99f6.png)


![image](https://user-images.githubusercontent.com/114162412/212108056-1e748c01-90c2-4142-a0e0-4e888d69457b.png)


![image](https://user-images.githubusercontent.com/114162412/212108506-0f3414ed-6df4-44b0-83ea-8fef17bcaea4.png)


![image](https://user-images.githubusercontent.com/114162412/212109444-a38c0b0a-ba0a-45ca-992e-7d38924930ab.png)


![image](https://user-images.githubusercontent.com/114162412/212109627-375b84be-cf38-4541-9627-e5bf20ffd226.png)


5.Afegeix un fitxer d'usuaris des de fitxer (4 companys/es de classe) simultàniament carregant un fitxer de dades, com es veu a la imatge.


Ens descarreguem el document de exemple i canviem el seu contingut per les dades correctes, despres el penjem.


![Selecció_952](https://user-images.githubusercontent.com/114162412/212111336-26ca08f4-fab0-4fec-adb9-a4c01dab65cf.png)


![image](https://user-images.githubusercontent.com/114162412/212113589-39616184-799d-4ea5-8798-0cbd68a5766f.png)


### PERSONALITZAR PERFIL 





Ficar foto de perfil: 



![Selecció_999](https://user-images.githubusercontent.com/114162412/213235159-b3eff583-edf0-4f4e-9382-09b1b213e8fe.png)



![Selecció_1000](https://user-images.githubusercontent.com/114162412/213235230-348dbd0f-e133-4587-b50f-952d7570c5fa.png)


Canviar contrassenya:



![Selecció_1001](https://user-images.githubusercontent.com/114162412/213235841-1ba5ce8f-3eb4-4a4a-b86b-55ca62d73af9.png)


![Selecció_1002](https://user-images.githubusercontent.com/114162412/213235901-627c2c58-7831-474c-8bc0-65568a555fe4.png)



Canviar idioma:



![Selecció_1003](https://user-images.githubusercontent.com/114162412/213236571-64a81889-6dc3-4bdf-ac25-500f23fd9384.png)



Ocultar correu: 


![Selecció_1004](https://user-images.githubusercontent.com/114162412/213237660-ce39aa27-a41a-4f61-abde-43169125ec8e.png)



Modificar l'agenda:


![Selecció_1005](https://user-images.githubusercontent.com/114162412/213237916-a28dc649-71e4-4c5e-9d4e-2ca242d00e4b.png)


Fes que només els contactes de l'usuari puguin enviar-li missatges.

![image](https://user-images.githubusercontent.com/114162412/213238211-7e838d2a-c655-411c-a7d9-32afbd0bcf93.png)


### CURSOS

- Crea un Curs nou anomenat "Programació amb Python" dins de la categoria 2n curs -> DAM.

![image](https://user-images.githubusercontent.com/114162412/213469862-6c8c8427-c94d-4d98-bba6-39ce9772e3e3.png)


- Inscriu a tots els alumnes al curs.

![Selecció_1008](https://user-images.githubusercontent.com/114162412/213470327-288c73ed-a254-4b68-8edd-773774364165.png)



![Selecció_1009](https://user-images.githubusercontent.com/114162412/213470351-b85cde38-8a67-4680-8d49-98e9ce5d4946.png)



![Selecció_1010](https://user-images.githubusercontent.com/114162412/213470393-c53b126d-9a00-4f2b-8f08-7dd48bb76c63.png)


- Inscriu a tots els professors al curs.

![image](https://user-images.githubusercontent.com/114162412/213470622-98ac11b6-dd00-4de1-bc5b-b62a8e18867c.png)


- Crea un fòrum general de dubtes (el d'anuncis no serveix).


![image](https://user-images.githubusercontent.com/114162412/213471711-543efc90-9d5f-4788-8631-1cfbe3ca2716.png)


- Fes que dos estudiants fassin consultes al fòrum.


![Selecció_1014](https://user-images.githubusercontent.com/114162412/213478120-fcdf129e-dbe2-4039-9c52-c5c8a6f66482.png)


![Selecció_1015](https://user-images.githubusercontent.com/114162412/213478252-8b8fa332-645d-461d-b3f5-03e02e1fb849.png)



- Contesta les preguntes amb un perfil de professor.

![Selecció_1016](https://user-images.githubusercontent.com/114162412/213478327-4bdc65f5-34bc-43f6-ab4f-526cefc204a8.png)


![Selecció_1017](https://user-images.githubusercontent.com/114162412/213478398-28dd6a0a-97e3-4f64-bed3-f89d057e1892.png)


![Selecció_1018](https://user-images.githubusercontent.com/114162412/213478457-c9346358-d453-42e0-a60b-77d983bb7d7f.png)


- Crea un tema anomenat "Fonaments de Python".

![image](https://user-images.githubusercontent.com/114162412/213480386-6d7c6483-4d3e-4c89-b6fc-fd1bbf57579f.png)


- Oculta la resta de temes del curs.

![Selecció_1020](https://user-images.githubusercontent.com/114162412/213480747-d5623bf4-2843-4c68-a63e-a277f1417830.png)


![Selecció_1021](https://user-images.githubusercontent.com/114162412/213480795-cad9f348-0407-4b13-bb42-251fa48867a4.png)


- Dins del tema crea una tasca anomenada prova inicial amb 10 preguntes molt bàsiques sobre Python. Pots trobar info [aqui](https://www.w3schools.com/python/).

![Selecció_1022](https://user-images.githubusercontent.com/114162412/213482075-e87438dd-2fc2-4bbf-be60-257726bd581c.png)


![Selecció_1023](https://user-images.githubusercontent.com/114162412/213482099-94f4d8ff-7a02-4870-90d8-ee51983e2778.png)


![Selecció_1024](https://user-images.githubusercontent.com/114162412/213485668-2dcaa766-c2c1-4d58-9aa1-dd8fd236aad8.png)


![Selecció_1025](https://user-images.githubusercontent.com/114162412/213485688-8dd75d2b-6f25-4281-a697-aee1af779ada.png)



- Fes que els alumnes contestin la tasca (mínim 3).

![Selecció_1026](https://user-images.githubusercontent.com/114162412/213489190-e973a7dd-82a7-4115-a221-adf96c4f590b.png)


- Qualifica la tasca de l'alumnat.
- Mostra les qualificacions de l'alumnat.

![image](https://user-images.githubusercontent.com/114162412/213489419-a2456f25-3610-4678-bbfa-61938bcba641.png)

- Com a perfil d'alumne mostra la teva qualificació.

![image](https://user-images.githubusercontent.com/114162412/213489612-f3ad0942-f3d3-416c-a40d-aa9be8be275c.png)

- Crea una activitat de consulta amb una pregunta sobre què costa més a l'alumnat sobre el tema del curs amb 4 opcions.

![Selecció_1029](https://user-images.githubusercontent.com/114162412/213723399-f4d7b83f-beb6-44ce-86f1-bc86b0001e79.png)


![Selecció_1030](https://user-images.githubusercontent.com/114162412/213723454-9427033c-55cb-42ea-a0fb-da360a0bc241.png)


![Selecció_1031](https://user-images.githubusercontent.com/114162412/213723530-3d043cef-2197-4e6b-ba42-f911307a53f7.png)


- Com a alumne/a contesta la consulta.

![Selecció_1032](https://user-images.githubusercontent.com/114162412/213724266-70c1a01a-e1e7-46a6-8a75-2955c66d44b6.png)


![Selecció_1033](https://user-images.githubusercontent.com/114162412/213724318-87eea011-1a85-4476-9292-04d44d0ddcea.png)


- Com a professor mostra les contestacions de la consulta.

![image](https://user-images.githubusercontent.com/114162412/213724978-1a2a9a94-55e1-403e-80b8-8c1441012ec6.png)

# Seguretat del lloc:

![image](https://user-images.githubusercontent.com/110727546/207085138-c3cbcb81-edee-45a1-8b11-daf20093e56d.png)


- Bloca una adreça IP d'un company per a que no pugui accedir al moodle i mostra-ho. VIGILA: Fica una adreça IP sencera si no bloquejaràs tota la xarxa, si us passa podeu arreglar-ho [així](https://moodle.org/mod/forum/discuss.php?d=323745).

![image](https://user-images.githubusercontent.com/114162412/213733364-5eaa7565-483d-4ac6-805d-d242e33da4e8.png)

- Canvia els paràmetres de les contrasenyes d'usuari per a que les contrasenyes:
  - Tinguin mínim 10 caràcters.
  - Tinguin 2 dígits.
  - Tinguin 3 maiúscules.

![image](https://user-images.githubusercontent.com/114162412/213739590-e4ff6f86-2da0-481f-a508-2b04658910f0.png)


# Plugins:

- Instal·la 2 [plugins](https://moodle.org/plugins/) de Moodle i explica què fan.

![image](https://user-images.githubusercontent.com/114162412/213741450-eac09ba3-2322-4e3e-8976-b93c0c03fa32.png)

![image](https://user-images.githubusercontent.com/114162412/213745773-6c1b64df-2c75-4ad1-8f75-98b9d813fa9d.png)


# Personalització del tauler d'usuaris:

![image](https://user-images.githubusercontent.com/110727546/207088651-6131a2b1-20c7-4a9f-b50a-317295ce70f1.png)

Canvia el tauler d'usuaris.

- Afegeix a dalt de tot "Cursos visitats recentment".
- Afegeix a baix de tot "Usuaris en línia".

![Selecció_1042](https://user-images.githubusercontent.com/114162412/215115623-bc91a103-7dd4-4dcf-b96c-36bbd28550ef.png)


![Selecció_1043](https://user-images.githubusercontent.com/114162412/215115724-55d23dce-2ff9-4529-8a0f-ce2ef1194480.png)


![Selecció_1044](https://user-images.githubusercontent.com/114162412/215115781-d3aaa953-8475-4982-9288-9bffc4a1ae61.png)



# Canvia el tema del Moodle:

Vigila que sigui un tema suportat per la teva versió de Moodle:

[Pàgina amb temes.](https://moodle.org/plugins/browse.php?list=category&id=3)
[Com canviar el tema.](https://docs.moodle.org/24/en/Installing_a_new_theme)
