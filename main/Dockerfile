FROM ubuntu:latest

# INSTALL PHP
RUN apt update
RUN apt install php php-gd php-mbstring php-intl php-zip php-mbstring php-pgsql php-mysql php-dom php-xml php-curl php-pdo php-dev -y

# Прочие библиотеки
RUN apt install vim ssh curl git -y

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
RUN . ~/.nvm/nvm.sh && nvm install 20.18.1 && nvm use 20.18.1
ENV NODE_PATH /usr/bin/versions/node/v20.18.1/lib/node_modules
ENV PATH=/usr/bin/versions/node/v20.18.1/bin:$PATH
RUN . ~/.nvm/nvm.sh && npm -g install yarn 

# install python
RUN apt install software-properties-common -y
RUN add-apt-repository ppa:deadsnakes/ppa -y
RUN apt install python3.13-full -y

# install mongodb
RUN pecl install mongodb
RUN echo "extension=mongodb.so" > /etc/php/8.3/cli/conf.d/30-mongodb.ini

# install phantomjs for arm64
RUN apt update
RUN wget https://launchpad.net/ubuntu/+source/phantomjs/2.1.1+dfsg-2ubuntu1/+build/19118060/+files/phantomjs_2.1.1+dfsg-2ubuntu1_arm64.deb
RUN apt install ./phantomjs_2.1.1+dfsg-2ubuntu1_arm64.deb -y
RUN apt install -f
RUN export DISPLAY=:0    
RUN export QT_QPA_PLATFORM='offscreen'

RUN apt install -y wget gnupg 
# RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
# RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
# RUN apt update

# install dependencies for chromium
# RUN apt install -y libgtk2.0-0t64 libgtk-3-0t64 libgbm-dev libnotify-dev libnss3 libxss1 libasound2t64 libxtst6 xauth xvfb
# RUN apt install -y fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst fonts-freefont-ttf libxss1 --no-install-recommends
# RUN apt install -y chromium-browser
# RUN rm -rf /var/lib/apt/lists/*
RUN add-apt-repository ppa:xtradeb/apps -y
RUN apt update
RUN apt install -y chromium

RUN apt-get install -y iputils-ping

CMD ["/bin/bash"]

WORKDIR /var/www

# EXPOSE 8000 8080