# 1 - Dependencies management

***git branch name:*** dependencies

## Theory [2]

As usual, we will start with a few theoretical questions:

* [0.5] What is Docker, and how it differs from dependencies management systems? From virtual machines?

Docker — это программная платформа, позволяющая быстро создавать, тестировать и развертывать приложения. Docker упаковывает программное обеспечение в стандартизированные блоки внутри одной операционной системы, называемые контейнерами, которые содержат все необходимое для работы программного обеспечения, включая библиотеки, системные инструменты, код и среду выполнения. 
При этом системы управления зависимостями - по сути управляют наборами программ и с их зависимостями внутри системы. При этом контейнер докера - это изолированная система со своими зависимостями и окружением, отделенное от других приложений и ОС.
Виртуальная машина это по сути программная или аппаратна система, которая эмулирует работу "витруальной" версии компьютера с выделенными ему ресурсами ЦП, памяти и тд.   То етсь виртальная машина реализует виртуализацию на уровне железа, а докер на уровне ОС. В отличае от докера виртуальные машины менее гибкие, каждая ВМ имеет свою ОС. Плюс контейнер может быть легко перенесен на другую машину. Docker потребляют меньше ресурсов, быстрее развёртываются, проще масштабируются и меньше весят.

* [0.5] What are the advantages and disadvantages of using containers over other approaches?

Частично я ответил в прошлом вопросе. Но если кратко, то к преимуществам докер-контейнеров относится: быстрая скорость работы, доступность, быстрота развертывания, меньшие ресурсозатраты, позволяет работать с разными файловыми системами.
К недостаткам можно отнести: безопасность, необходимость работать на системе, на которой создан, сложность удаления, часто необходима тонкая настройка

* [0.5] Explain how Docker works: what are Dockerfiles, how are containers created, and how are they run and destroyed?

Работа докера происходит на уровне ОС - докер хоста. При этом докер создает образ (image) поверх файловой системы в который можно установить программы, конфигурации и необходимые фаловые зависимости. Управление формированием образа происходит с помощью докерфайла, который является инструкцией для сборки образа. В результате появляется контейнер, который по сути является результатом выполнения образа. Внутри данного контейнера могут быть получены, отредактированы и записаны данные, которые при необходимости можно извлечь с помощью volumes. После удаления контейнера данные сотрутся. 
СОздание контейнера поисходит с помощью команды docker build, которая принимает на вход dockerfile, внутри которого написано что будет рецепт поэтапного создания образа. После чего, можно запустить образ командой docker run --rm -it dockerfilehw:latest bash для работы внутри контейнера.

* [0.25] Name and describe at least one Docker competitor (i.e., a tool based on the same containerization technology).

Kaniko - альтернатива докера которая используется в создании образов контейнеров внутри Kubernetes от Google. Kaniko может делать образы из Docker-файлов и не зависеть от докера. Основное различие с Docker в ориентированность на рабочие процессы Kubernetes. 

* [0.25] What is conda? How it differs from apt, yarn, and others?

Conda — это система управления зависимостями. Conda может работать локально и независимо для каждого пользователя и включает в себя библиотеку установленных пакетов conda. Например, у вас может быть одна среда с NumPy 1.7 и ее зависимостями, а другая среда с NumPy 1.6 для устаревшего тестирования. В отличие от язык-специфичных библиотек, conda не ограничена яхыком. Так, если пакеты Pip — это NumPy или matplotlib, то пакеты Conda включают библиотеки Python (NumPy или matplotlib), библиотеки C (libjpeg) и исполняемые файлы (например, компиляторы C и даже сам интерпретатор Python). Более того, conda в отличии от других менеджеров, таких как apt предоставляет расширенный функционал для работы с окружениями.

## Problem [6.5]

The problem itself is relatively simple. 

Imagine that you developed an excellent RNA-seq analysis pipeline and want to share it with the world. Based on your experience, you are confident that the popularity of the pipeline will be proportional to its ease of use. So, you decided to help your future users and to pack all dependencies in a Conda environment and a Docker container.

Here is the list of tools and their versions that are used in your work:
* [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), v0.11.9
* [STAR](https://github.com/alexdobin/STAR), v2.7.10b
* [samtools](https://github.com/samtools/samtools), v1.16.1
* [picard](https://github.com/broadinstitute/picard), v2.27.5
* [salmon](https://github.com/COMBINE-lab/salmon), commit tag 1.9.0
* [bedtools](https://github.com/arq5x/bedtools2), v2.30.0
* [multiqc](https://github.com/ewels/MultiQC), v1.13

### Anaconda

* [1] Install conda, create a new virtual environment, and install all necessary packages.

Установил Анаконду:
bash Anaconda3-2022.10-Linux-x86_64.sh

Создание окружения:
conda create --name hw_env
conda activate hw_env

Добавил каналы:
conda config --add channels bioconda
conda config --add channels conda-forge

Установка пакетов в окружение:
conda install -c bioconda fastqc==0.11.9
conda install -c bioconda samtools==1.16.1
conda install -c bioconda star
conda install -c bioconda picard==2.27.5
не нашелся
conda install -c bioconda picard==2.27.4
conda install -c bioconda salmon==1.9.0
conda install -c bioconda bedtools==2.30.0
conda install -c bioconda multiqc==1.13
долго искал, сделал по-другому
conda install -c "bioconda/label/cf201901" multiqc\

* [0.75] You won't be able to install some tools - that's fine. List their names, and explain what should be done to make them conda-friendly ([conda-forge](https://conda-forge.org/docs/maintainer/adding_pkgs.html) channel, [bioconda](https://bioconda.github.io/contributor/workflow.html) channel).

Все пакеты (кроме одного) установились нормально. Сразу добавил каналы, где нужно искать заданные программы
conda config --add channels bioconda
conda config --add channels conda-forge # на случай если не будет в биоконде

Не удалось установить picard==2.27.5,
conda install -c bioconda picard==2.27.5

Он есть на гитхабе, но его нет в конде (по крайней мере в bioconda, conda-forge). Можно установить последнюю версию вручную. Но я просто поставил picard==2.27.4
 
* [0.25] [Export](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#exporting-the-environment-yml-file) the environment ([example](https://github.com/nf-core/clipseq/blob/master/environment.yml)) to the file and verify that it can be [rebuilt](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) from the file without problems.

Проверяем все ли работает:

1.$conda env list

conda environments:
base * /home/alexei/anaconda3
hw_env /home/alexei/anaconda3/envs/hw_env

conda env remove --name hw_env
conda env create -f hw_env.yml
conda activate hw_env # все работает

### Docker
* [3] Create a Dockerfile for a container with **all** required dependencies. Don't forget about comments; test that all tools are accessible and work inside the container. Hints:
 - If needed, grant rights to execute downloaded/compiled binaries using chmod (`chmod a+x BINARY_NAME`)
 - Move all executables to $PATH folders (e.g.`/usr/local/bin`) to make them accessible without specifying the full path.
 - Typical command to run a container interactively (`-it`) and delete on exit(`--rm`): `docker run --rm -it name:tag`
 
Ставим докер:
ставил как описано здесь https://docs.docker.com/engine/install/ubuntu/

Создаем dockerfile:
touch dockerfilehw

Создаем образ:
sudo docker build -t dockerfilehw .

Запускаем образ:
sudo docker run --rm -it dockerfilehw:latest bash

#нужно запустить . /.bashrc, чтобы все корректно работало

* [1] Use [hadolint](https://hadolint.github.io/hadolint/) and remove as many reported warnings as possible.

Не так много внесено исправлений. Добавлено --no-install-recommends, 
заменил в конце *zip на ./*zip

* [0.5] Add relevant [labels](https://docs.docker.com/engine/reference/builder/#label), e.g. maintainer, version, etc. ([hint](https://medium.com/@chamilad/lets-make-your-docker-image-better-than-90-of-existing-ones-8b1e5de950d))

Added version and description labels.

#Final dockerfile

```
FROM ubuntu:22.04
LABEL author=alexei_kotov
LABEL version="1.0.0"
LABEL description="my homework1"
# устанавливаем все, что необходимо для конфигурации пакетов
RUN apt-get update \
&& apt-get -y --no-install-recommends install wget \
&& apt-get -y --no-install-recommends install unzip \
&& apt-get -y --no-install-recommends install perl \
&& apt-get -y --no-install-recommends install openjdk-11-jdk xvfb \
&& apt-get -y --no-install-recommends install python3-pip \
&& apt-get -y --no-install-recommends install libgomp1 \
&& apt-get -y --no-install-recommends install libtbb12 \
&& touch /.bashrc

# устанавливаем пакеты:
# FastQC==0.11.9
RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip \
&& unzip fastqc_v0.11.9.zip \
&& chmod a+x /FastQC/fastqc \
&& echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

# STAR==2.7.10b
RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip \
&& unzip ./STAR_2.7.10b.zip \
&& chmod a+x ./STAR_2.7.10b/Linux_x86_64_static/STAR \
&& mv ./STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR \
&& rm -r ./STAR_2.7.10b

# samtools==1.16.1
RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip \
&& unzip ./samtools-1.16.1.zip \
&& mv ./samtools-1.16.1/misc /samtools\
&& rm -r ./samtools-1.16.1 \
&& echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

# picard==2.27.5
RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /bin/picard.jar \
&& chmod a+x /bin/picard.jar \
&& echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc

# bedtools==2.30.0
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary \
&& chmod a+x /bin/bedtools.static.binary\
&& echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

# MultiQC==1.13
RUN pip install multiqc==1.13

# salmon==1.9.0
RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz \
&& tar -zxvf ./salmon-1.9.0_linux_x86_64.tar.gz \
&& chmod a+x ./salmon-1.9.0_linux_x86_64/bin/salmon \
&& mv ./salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon \
&& rm -r ./salmon-1.9.0_linux_x86_64 

# Удаляем исходники
RUN rm ./*zip \
&& rm ./*tar.gz

# Чистим ненужные пакеты
RUN apt-get autoremove \
&& apt-get clean
```

## Extra points [1.5]

* [0.75] Minimizing the size of the final Docker image. That is, removing all intermediates, unnecessary binaries/caches, etc. Don't forget to compare & report the final size before and after all the optimizations.

Минимальизацию размера финального образа докер производил с самого первого шага создания.
1. удалял в процессе все загружаемые архивы zip и tar
2. Использовал --no-install-recommends с подсказки hadolint 
3. В конце установки выполнил autoremove и clean, правда это не сильно уменьшило размер

В результате изначальный размер докера, который у меня получился был более 2,5 гб. После оптимизации удалось уменьшить до 2 гб
