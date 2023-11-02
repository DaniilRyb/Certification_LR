# Лабораторные по Сертификации средств защиты информации
## Отчет ЛР1
### Инструкция по подготовке к фаззингу 
### Сам фаззер - это AFL от Google
https://hardik05.wordpress.com/2020/08/22/fuzzing-ffmpeg-with-afl-on-ubuntu/

клонируем репозиторий:
```
git clone --recursive https://github.com/FFmpeg/FFmpeg.git
```
Компиляция:

после клонирования репозитория git выполните следующую команду настройки, чтобы выполнить сборку:

```
./configure --prefix="$HOME/ffmpeg_build" --pkg-config-flags="--static" --extra-cflags="-I$HOME/ffmpeg_build/include" --extra-ldflags="-L$HOME/ffmpeg_build/lib" --extra-libs="-lpthread -lm" --bindir="$HOME/bin" --enable-gpl --enable-libass --enable-libfreetype --enable-libmp3lame --enable-libopus --enable-libvorbis --enable-libx264 --enable-libx265 --enable-nonfree --cc=afl-clang --cxx=afl-clang++ --extra-cflags="-I$HOME/ffmpeg_build/include -O1 -fno-omit-frame-pointer -g" --extra-cxxflags="-O1 -fno-omit-frame-pointer -g" --extra-ldflags="-L$HOME/ffmpeg_build/include -fsanitize=address -fsanitize=undefined -lubsan" --enable-debug
```

также:

1. будут использоваться компиляторы afl-clang и afl-clang++.

2. будет использоваться очиститель адресов и очиститель неопределенного поведения.

3. это включит режим отладки.

4. это включит статические библиотеки.

как мы можем видеть, он также включает следующие модули:

–enable-libass

–enable-libfreetype

–enable-libmp3lame

–enable-libopus

–enable-libvorbis

–enable-libx264

–enable-libx265

далее собираем:

```
make
```

команда для запуска процесса фаззинга
```
../AFL/afl-fuzz -i input/ -o output -m none -- ./ffmpeg -i @@ test
```
## Отчет ЛР2
## Контрольный суммы


```
daniil@daniil:~/FFmpeg$ sha256sum ffmpeg
e737d4879fb09cb9674bbe75c694c86ed6c12fc364ea60beb881e79110731cc5  ffmpeg
```


