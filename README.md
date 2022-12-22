# 2 - Working with remote servers

**git branch name:** jbrowser

## Theory [2]

* [0.4] What are computer ports on a high level? How many ports are there on a typical computer?

Порт  — это способ использования одного физического сетевого соединения для обработки входящих и исходящих запросов, назначая каждому номер порта и является 16-битным числом. Порты основаны на программном обеспечении и управляются операционной системой компьютера. Некоторые из этих номеров имеют специальное назначение и связаны с определенными типами процессов или служб, например порт 21 связан с передачей FTP файлов, а HTTP всегда имеет порт 80. Это позволяет компьютерам различать разные виды трафика. Посколько виртуальные порты - это 16-битное число, то существует 65535. Порты от 0 до 1023 называются общеизвестными, с 2024 по 49151 - зарегистрированные (или пользовательские) и порты с 49152 по 65535 - динамические (или частные). Интересный пример из интернета: порт - это как добавочный номер компании, а IP как основной номер телефона. По IP можно связаться с нужной компанией (компьютером), а добавочный номер уже соединит с нужным человеком (службой на компьютере). Набор добавочного номера 0 зачастую соединяет с оператором, что аналогично общеизвестным портам, которые всегда обозначают определенные службы.

* [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.

По своей сути протоколы - это свод правил и инструкций взаимодействия между компютером и сервером/другим компьютером. HTTP (Hyper Text Transfer Protocol) протокол изначально использовался для передачи текста, но сейчас используется и для других задач. Особенностью HTTP является типизация и согласование представления данных, что позволяет системам строиться независимо от передаваемых данных. Задача, которая традиционно решается с помощью протокола HTTP — обмен данными между пользовательским приложением, осуществляющим доступ к веб-ресурсам (обычно это веб-браузер) и веб-сервером. Как правило, передача данных по протоколу HTTP осуществляется через TCP/IP-соединения. HTTPS означает безопасный протокол передачи гипертекста через TLS или SSL. HTTPS шифрует данные открытым ключом, а затем получатель расшифровывает его. Открытый ключ лежит на сервере и входит в SSL-сертификат. В случае HTTP - не нужны SSL-сертификаты. Кроме защищенности, HTTP и HTTPS используют разные порты, 80 и 443, соответственно. SSH - это «Безопасная оболочка». Чтобы установить соединение, он имеет встроенный процесс аутентификации имени пользователя и пароля. Для подключения этого процесса аутентификации используется порт 22. В отличии от HTTPS: 1) SSH использует процесс аутентификации по имени / паролю для установления безопасного соединения; 2) SSH основан на туннелировании сети; 3) С технической точки зрения, SSH предназначен для защиты компьютерных сетей и связи между двумя компьютерами через Интернет, в то время как HTTPS - для защиты онлайн-передачи данных и шифрования связи между браузером и сервером. Существуют и другие протоколы, которые отличаются портами: POP3 - 110. SMTP - 25, FTP - 21, TFTP - 69 и др.

* [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser.

(1) В общем смысле IP - это уникальный 32-битный идентификатор (адрес) устройства в сети интернет или локальной сети из 4 чисел от 0 до 255, разделенных точкой. (2) Если задуматься, то в мире устройств гораздо больше, чем можно сгененрировать IP адресов. Соотвественно, это решается тем, что есть глобальный интернет и локальная сеть (домашняя, областная, университетская и тд.) Соответственно, на входе в локальную сеть стоит маршрутизатор, который является шлюзом и одновременно находится во внешней сети и в локальной. Так вот белый IP - это тот адрес под которым устройства из локальной сети работают в глобальной сети. При этом серый адрес определяется маршрутизатором для устройств внутри локальной сети. По этому адресу устройство будет доступно внутри сети, но из Интернета устройство не будет видно. (3) Сначала браузер будет пытаться понять к какому IP-адресу сервера относится введенный сайт. Сначала будет проверена история подключений, кеш роутера, оперционной системы (ОС), кеш провайдера и далее по цепочке, пока не найдется адрес, после чего он запишется во все кеши и браузер продолжит. Далее нужно определиться с протоколом запроса, так как мы его не указали. Для этого будет проврка наличия указанного домена в списке HTTP Strict Transport Security, далее когда там найдется google.com браузер отправит ему запрос по https. Браузер и сервер создадут защищенное соединение. Для этого браузер создаст запрос на наличие SSL-сертификата, далее он получит публичный ключ, далее запрос в центр сертификации. Если информация подтверждается, генерируется сеансовый ключ, зашифровывается публичным ключом и отправляется на сервер. Сервер расшифровывает сообщение с помощью приватного ключа и сохраняет сеансовый ключ. Таким образом для общения далее будет использоваться сеансовый ключ. Далее во время общения нужно надежно упаковать пакеты с данными, чтобы ничего не потерялось. Для этого есть протокол нижележащего уровня - TCP. ОС, получив сообщение от браузера, инкапсулирует его в пакеты нижележащего уровня. Надежность доставки пакетов протокол TCP обеспечивает посредством их нумерации, контроля правильного порядка пакетов и подтверждения их передачи. На сетевом уровне, TCP пакет упаковывается в IP пакет и такие пакеты перемещаются из одной сети к другой, пока не достигнут получателя. По достижении сервера пакет распоковывается в обратном порядке. Далее сервер прочитав его формирует ответ, запаковывает его и отправляет обратно. Получив ответ, браузер распаковывает его, парсит и показывает нам то, что мы запросили.

* [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.

NGINX — это программное обеспечение с открытым исходным кодом, которое используется в качестве веб-сервера, а также используется в качестве обратного прокси-сервера, кэша HTTP и балансировщика нагрузки. Nginx разбивает каждый запрос пользователя на несколько мелких, упрощая таким образом обработку каждого. После обработки каждое соединение собирается в одном виртуальном контейнере, чтобы трансформироваться в единый первоначальный запрос, а после отправляется пользователю. Одно соединение может одновременно обрабатывать до 1024 запросов конечного пользователя. Для уменьшения нагрузки на оперативную память веб-сервер использует выделенный сегмент памяти, который называется «пул» (pool). Он динамический и расширяется при увеличении длины запроса. В качестве альтернативы приводятся pound, ha proxy, websockets, yaws


* [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.

SSH - это протокол прикладного уровня, который позволяет управлять сервером через интернет, а именно передачи данных через защищенное соединения, удаленного запуска программ, выполнения команд через командную строку, переадресации портов и тд. Есть два типа аутентификации. (1) Аутентификация по паролю. Требует фактического идентификатора пользователя и пароля от хоста, на котором находится сервер SSH. При аутентификации на основе пароля после установления безопасного соединения с удаленными серверами пользователи SSH обычно передают свои имена пользователей и пароли удаленным серверам для аутентификации клиентов. Эти учетные данные передаются через безопасный туннель, установленный с помощью симметричного шифрования. Сервер проверяет эти учетные данные в базе данных и, если они найдены, аутентифицирует клиента и разрешает ему общаться. (2) Аутентификация на основе открытого ключа. При аутентификации на основе открытого ключа после того, как клиент устанавливает соединение с удаленным сервером, клиент сообщает серверу пару ключей, с помощью которой он хотел бы пройти аутентификацию. Сервер проверяет наличие этой пары ключей в своей базе данных, а затем отправляет зашифрованное сообщение клиенту. Клиент расшифровывает сообщение своим закрытым ключом и генерирует хеш-значение, которое отправляется обратно на сервер для проверки. Сервер генерирует собственное хэш-значение и сравнивает его с отправленным клиентом. Когда оба хеш-значения совпадают, сервер убеждается в подлинности клиента и позволяет ему взаимодействовать с сервером.

## Problem [6.5]

A real-life situation that occurred to me several times over the years.

Imagine wrapping up a large bioinformatics project and wanting to share raw data with your colleagues in a friendly and straightforward format. The best option would be to use an online genome browser and host your data remotely, so it is easily accessible by anyone with a valid link. This is exactly what we will be doing here.

*Please consider doing this HW using Linux since setting up the SSH client on Windows is painful, and I won't be able to help you.*

**Remote Server**:
* [2] Create a new virtual machine in the Yandex/Mail/etc cloud (order at least 10GB of free disk space). Generate SSH key pair and use it to connect to your server.

Создал виртуальную машину
kotov_alexei@hw2machine
Адрес: 130.193.43.200

* [1] Download the latest human genome assembly (GRCh38) from the Ensemble FTP server ([fasta](https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz), [GFF3](https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz)). Index the fasta using samtools (`samtools faidx`) and GFF3 using tabix. 

#Загружаем геном
wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz

#Загружаем аннотацию
wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz

#Индексируем геном
sudo apt-get install samtools
gzip -d Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa

#Индексируем аннотацию
sudo apt-get install tabix
gzip -d Homo_sapiens.GRCh38.108.gff3.gz
(grep "^#" Homo_sapiens.GRCh38.108.gff3; grep -v "^#" Homo_sapiens.GRCh38.108.gff3 | sort -t"`printf '\t'`" -k1,1 -k4,4n) | bgzip > Homo_sapiens.GRCh38.108.sorted.gff3.gz
tabix -p gff Homo_sapiens.GRCh38.108.sorted.gff3.gz


* [1] Select and download BED files for three ChIP-seq and one ATAC-seq experiment from the ENCODE (use one tissue/cell line). Sort, bgzip, and index them using tabix.

#Брал в качестве линии K562
#bigBed narrowPeak for CHIP-seq SMAD3 in K562
wget -O SMAD3.bed.gz "https://www.encodeproject.org/files/ENCFF335ZTU/@@download/ENCFF335ZTU.bed.gz"
#bigBed narrowPeak for CHIP-seq MLX in K562
wget -O MLX.bed.gz "https://www.encodeproject.org/files/ENCFF682SSQ/@@download/ENCFF682SSQ.bed.gz"
#bigBed narrowPeak for CHIP-seq TGIF2 in K562
wget -O TGIF2.bed.gz "https://www.encodeproject.org/files/ENCFF336YLK/@@download/ENCFF336YLK.bed.gz"
#bigBed narrowPeak for CHIP-seq ATACseq in K562
wget -O ATAC.bed.gz "https://www.encodeproject.org/files/ENCFF223QDM/@@download/ENCFF223QDM.bed.gz"

#Распаковываем и сортируем
gzip -d *bed.gz
for i in *.bed; do sort -k 1,1 -k2,2n $i > $(echo $i| cut -d '.' -f 1)'_sorted.bed'; done

#впоследствии обнаружил, что названия хромосом в gff и bed отличаются, поэтому переименовал в bed хромосомы (например, из chr10 в 10)
#иначе они не будут отображаться в браузере
for i in *.sorted.bed; do awk '{gsub(/^chr/,""); print}' $i > $(echo $i| cut -d '.' -f 1)'_renamed.bed'; done

#затем запаковал и индексировал
for i in *sorted_renamed.bed; do bgzip -c $i > $i'.gz'; done
for i in *sorted_renamed.bed.gz; do tabix -p bed $i; done

**JBrowse 2**
* [1] Download and install [JBrowse 2](https://jbrowse.org/jb2/). Create a new jbrowse [repository](https://jbrowse.org/jb2/docs/cli/#jbrowse-create-localpath) in `/mnt/JBrowse/` (or some other folder).

cd ../..
cd mnt/
sudo mkdir JBrowse
sudo wget https://github.com/GMOD/jbrowse-components/releases/download/v2.3.2/jbrowse-web-v2.3.2.zip
sudo apt-get install unzip
sudo unzip jbrowse-web-v2.3.2.zip 
sudo rm jbrowse-web-v2.3.2.zip

* [0.25] Install nginx and amend its config(/etc/nginx/nginx.conf) to contain the following section:

#Устанавливаем
sudo apt install nginx

#Меняем конфиг
sudo nano /etc/nginx/nginx.conf 

```conf
http {
  # Don't touch other options!
  # ........
  # ........

  # Comment this line(!):
  # include /etc/nginx/sites-enabled/*;

  # Add this:
  server {
    listen 80 default_server;
    index index.html;
    server_name _;

    # Don't put JBrowse inside the home directory!
    # You will have problems with permissions
    location /jbrowse/ {
      alias /mnt/JBrowse/;	
    }
  }
}
```

* [0.25] Restart the nginx (reload its config) and make sure that you can access the browser using a link like this: `http://64.129.58.13/jbrowse/`. Here `64.129.58.13` is your public IP address.

#Перезапускаем 
sudo nginx -s reload

Проверяем ссылку, все работает.

* [1] Add your files (BED & FASTA & GFF3) to the genome browser and verify that everything works as intended. Don't forget to [index](https://jbrowse.org/jb2/docs/cli/#jbrowse-text-index) the genome annotation, so you could later search by gene names.

#Загружаем файлы в браузер

sudo jbrowse add-assembly Homo_sapiens.GRCh38.dna.primary_assembly.fa --load copy --out /mnt/JBrowse/
sudo jbrowse add-track Homo_sapiens.GRCh38.108.sorted.gff3.gz --out /mnt/JBrowse/
sudo jbrowse add-track MLX_sorted_renamed.bed.gz --load copy --out /mnt/JBrowse/
sudo jbrowse add-track SMAD3_sorted_renamed.bed.gz --load copy --out /mnt/JBrowse/
sudo jbrowse add-track TGIF2_sorted_renamed.bed.gz --load copy --out /mnt/JBrowse/

Ссылка на браузер
http://130.193.43.200/jbrowse/?session=share-iWIhhsqrm7&password=yQmjR


**Remember to put a [persistent link](https://jbrowse.org/jb2/docs/user_guides/basic_usage/#sharing-sessions) to a JBrowse 2 session with all your BED files and the genome annotation in the report (like [this](https://jbrowse.org/code/jb2/v2.3.1/?session=share-HShsEcnq3i&password=nYzTU)). I must be able to access it without problems.**
