# PTAM-lab11

Данная лабораторная работа посвящена изучению процесса создания сеансов совместной разработки с использованием инструмента ngrok.

## Tutorial

```bash
cd ~
mkdir install
mkdir tmp
export HOME_PREFIX=`pwd`/install
echo $HOME_PREFIX
export USERNAME=`whoami`
cd tmp
```

## Установка libevent

```bash
wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
tar -xvzf libevent-2.1.8-stable.tar.gz
cd libevent-2.1.8-stable
./configure --prefix=${HOME_PREFIX}
make && make install
cd ..
```

## Установка ncurses

```bash
wget http://invisible-island.net/datafiles/release/ncurses.tar.gz
tar -xvzf ncurses.tar.gz
cd ncurses-5.9
./configure --prefix=${HOME_PREFIX}
make && make install
cd ..
```

## Установка tmux

```bash
wget https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
tar -xvzf tmux-2.5.tar.gz
cd tmux-2.5
./configure --prefix=${HOME_PREFIX} CFLAGS="-I${HOME_PREFIX}/include -I${HOME_PREFIX}/include/ncurses" LDFLAGS="-L${HOME_PREFIX}/lib"
make && make install
cd ..
```

## Установка ngrok

```bash
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
mv ngrok ${HOME_PREFIX}/bin
```

## Настройка окружения

```bash
export LD_LIBRARY_PATH=${HOME_PREFIX}/lib
export PATH="${HOME_PREFIX}/bin:${PATH}"
tmux
cd ~
rm -rf tmp install
```

## Запуск совместной сессии

```bash
tmux new -s session_with_group
```

## Настройка ngrok (со стороны пользователя, открывающего доступ)

```bash
export NGROK_TOKEN=<мой токен (вставлять не буду)>
ngrok authtoken ${NGROK_TOKEN}
ngrok tcp 22
```

## Подключение второго участника

```bash
ssh ${USERNAME}@0.tcp.ngrok.io -p<порт_ngrok_сервера>
<пароль от учетной записи (по понятным причинам не вставляю)>
tmux a -t session_with_group
```

## Разделение экрана в tmux

```bash
<C-B>"
```

# Вывод

В ходе лабораторной работы были освоены: сборка и установка tmux и ngrok из исходных кодов, настройка туннеля с помощью ngrok для удалённого доступа к SSH, организация совместной сессии в tmux. Это позволяет организовать совместную разработку и отладку программ в реальном времени, даже если участники находятся за NAT или файрволом.
