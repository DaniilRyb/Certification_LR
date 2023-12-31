## Лабораторные по Сертификации средств защиты информации
## Отчет ЛР1 Фаззинг-тестирование
### Инструкция по подготовке к фаззингу 
### Сам фаззер - это AFL от Google
https://hardik05.wordpress.com/2020/08/22/fuzzing-ffmpeg-with-afl-on-ubuntu/

Клонируем репозиторий:
```
git clone --recursive https://github.com/FFmpeg/FFmpeg.git
```

После клонирования репозитория git выполните следующую команду настройки, чтобы выполнить сборку:

```
./configure --prefix="$HOME/ffmpeg_build" --pkg-config-flags="--static" --extra-cflags="-I$HOME/ffmpeg_build/include" --extra-ldflags="-L$HOME/ffmpeg_build/lib" --extra-libs="-lpthread -lm" --bindir="$HOME/bin" --enable-gpl --enable-libass --enable-libfreetype --enable-libmp3lame --enable-libopus --enable-libvorbis --enable-libx264 --enable-libx265 --enable-nonfree --cc=afl-clang --cxx=afl-clang++ --extra-cflags="-I$HOME/ffmpeg_build/include -O1 -fno-omit-frame-pointer -g" --extra-cxxflags="-O1 -fno-omit-frame-pointer -g" --extra-ldflags="-L$HOME/ffmpeg_build/include -fsanitize=address -fsanitize=undefined -lubsan" --enable-debug
```

как мы можем видеть, он включает следующие модули:

–enable-libass

–enable-libfreetype

–enable-libmp3lame

–enable-libopus

–enable-libvorbis

–enable-libx264

–enable-libx265

далее собираем исходники:

```
make
```

команда для запуска процесса фаззинга
```
../AFL/afl-fuzz -i input/ -o output -m none -- ./ffmpeg -i @@ test
```
Результат фаззинга за 3 часа:

![](https://github.com/DaniilRyb/Certification_LR/blob/master/LR1-image.png)

Полный отчет: https://github.com/DaniilRyb/Certification_LR/blob/master/output_ffmpeg.zip
## Отчет ЛР2 Съём контрольных сумм
![](https://github.com/DaniilRyb/Certification_LR/blob/master/LR2-image.png)

## Отчет ЛР3 Анализ покрытия
 Установим инструмент lcov.
 ```
 sudo apt-get install lcov
```
Соберем проект с поддержкой покрытия кода, добавим флаги для конфигурации --enable-gpl --samples=/path/to/fate/samples --extra-cflags="-fprofile-arcs -ftest-coverage" --extra-ldflags="-fprofile-arcs -ftest-coverage":
```
./configure --prefix="$HOME/ffmpeg_build" --pkg-config-flags="--static" --extra-cflags="-I$HOME/ffmpeg_build/include" --extra-ldflags="-L$HOME/ffmpeg_build/lib" --extra-libs="-lpthread -lm" --bindir="$HOME/bin" --enable-gpl --enable-libass --enable-libfreetype --enable-libmp3lame --enable-libopus --enable-libvorbis --enable-libx264 --enable-libx265 --enable-nonfree --cc=gcc --cxx=g++ --extra-cflags="-I$HOME/ffmpeg_build/include -O1 -fno-omit-frame-pointer -g" --extra-cxxflags="-O1 -fno-omit-frame-pointer -g" --extra-ldflags="-L$HOME/ffmpeg_build/include -fsanitize=address -fsanitize=undefined -lubsan" --enable-debug --enable-gpl --samples=/path/to/fate/samples  --extra-cflags="-fprofile-arcs -ftest-coverage"  --extra-ldflags="-fprofile-arcs -ftest-coverage"
```

Собираем проект

```
make
```
Очиститим данные покрытия, если они есть:
```
lcov --directory . --zerocounters
```
Запуститим набор тестов FATE:
```
make fate
```

Запускаем подсчет покрытия:
```
lcov --directory . --capture --output-file coverage.info
mkdir html-report
genhtml -o html-report coverage.info
```

![](https://github.com/DaniilRyb/Certification_LR/blob/master/LR3-image.png)
