FROM ubuntu:latest

# INSTALL PHP
RUN apt update
RUN apt install php php-gd php-mbstring php-intl php-zip php-mbstring php-pgsql php-mysql php-dom php-xml php-curl php-pdo php-dev -y

# Прочие библиотеки
RUN apt install vim ssh curl -y

# добавление ключей, чтобы зайти на удалённый сервер
COPY ./id_rsa /root/.ssh/id_rsa
COPY ./id_rsa.pub /root/.ssh/id_rsa.pub

# фикс прав на ключ
RUN chmod 600 ~/.ssh/id_rsa

# INSTALL COMPOSER
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

USER root

# INSTALL NODE + YARN
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
RUN . ~/.nvm/nvm.sh && nvm install 20 && nvm use 20
ENV NODE_PATH /usr/bin/versions/node/v20.18.0/lib/node_modules
ENV PATH /usr/bin/versions/node/v20.18.0/bin:$PATH
RUN npm -g install yarn 

# Забыл гит поставить
RUN apt install git -y

CMD ["/bin/bash"]

WORKDIR /var/www

# EXPOSE 8000 8080