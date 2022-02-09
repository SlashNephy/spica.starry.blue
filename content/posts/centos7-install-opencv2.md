---
title: CentOS 7 の Python 3.5.2 に OpenCV を導入する
date: 2016-09-18T15:38:04+09:00
draft: false
toc: false
images:
tags:
  - python
  - centos7
---

MatLabのインストールは前提。
以下の操作は root ユーザーで実行することにします。

まず始めに依存関係をインストールします。
```shell
$ yum install -y git gcc bzip2 bzip2-devel openssl openssl-devel readline readline-devel sqlite-devel
$ yum install -y cmake libjpeg-devel libtiff-devel libpng-devel jasper-devel
$ yum install -y mesa-libGL-devel libXt-devel libgphoto2-devel nasm libtheora-devel
$ yum install -y autoconf automake gcc-c++ libtool yasm openal-devel blas blas-devel atlas atlas-devel lapack lapack-devel
```

# Python 3.5.2
今回は `/usr/local` にインストールします。
```shell
$ cd /usr/local/src
$ wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz
$ tar zxf Python-3.5.2.tgz

$ cd Python-3.5.2
$ ./configure --prefix=/usr/local --enable-shared
$ make
$ make altinstall

$ echo /usr/local/lib > /etc/ld.so.conf.d/python3.5.2.conf
$ ldconfig
```

# numpy
```shell
$ pip3.5 install numpy
```

# linux/videodev.h
```shell
$ yum install libv4l libv4l-devel
$ ln -s /usr/include/libv4l1-videodev.h /usr/include/linux/videodev.h
```

# x264
```shell
$ cd /usr/local/src
$ git clone --depth 1 git://git.videolan.org/x264

$ cd x264
$ ./configure --enable-shared --enable-static --enable-pic --disable-opencl --disable-lavf
$ make
$ make install
```

# fdk-aac
```shell
$ cd /usr/local/src
$ git clone --depth 1 git://github.com/mstorsjo/fdk-aac.git

$ cd fdk-aac
$ autoreconf -fiv
$ ./configure
$ make
$ make install

$ export LD_LIBRARY_PATH=/usr/local/lib
$ export LD_RUN_PATH=$LD_LIBRARY_PATH
```

# lame
```shell
$ cd /usr/local/src
$ wget http://downloads.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz
$ tar zxvf lame-3.99.5.tar.gz

$ cd lame-3.99.5
$ ./configure --enable-nasm
$ make
$ make install
```

# xvidcore
```shell
$ cd /usr/local/src
$ wget http://downloads.xvid.org/downloads/xvidcore-1.3.2.tar.gz
$ tar xzvf xvidcore-1.3.2.tar.gz

$ cd xvidcore/build/generic
$ ./configure
$ make
$ make install
```

# ffmpeg
```shell
$ cd /usr/local/src
$ git clone --depth 1 git://source.ffmpeg.org/ffmpeg.git

$ cd ffmpeg
$ ./configure --arch=x86_64 --enable-gpl --enable-libmp3lame --enable-libtheora --enable-libx264 --enable-libxvid --enable-nonfree --enable-openal --disable-static --enable-shared --enable-version3 --enable-zlib --enable-pthreads
$ make -j8
$ make install

$ sh -c "echo /usr/local/lib >> /etc/ld.so.conf.d/ffmpeg.conf"
$ ldconfig
```
-j8 オプションはコア数に応じて変更するとよいらしいです。

# VTK
http://www.vtk.org/download/ から `Source  VTK-7.0.0.zip` を落としてきました。
```shell=
$ cd /usr/local/src
$ wget http://www.vtk.org/files/release/7.0/VTK-7.0.0.zip
$ unzip VTK-7.0.0.zip

$ cd VTK-7.0.0
$ ccmake .
```

ここで画面が切り替わるので `EMPTY CACHE` と表示される場合には `[c]` を押して少し待ちましょう。

今回は Python 3 バインディングをインストールするため、Python 2向けのオプションを変更する必要があります。

`[t]` を押しすべてのオプションを表示します。以下のオプションを`[enter]`キーで変更します。`ON` `OFF` は一度押すとトグルできます。

> `PYTHON_EXECUTABLE` → `/usr/local/bin/python3.5`
> `PYTHON_INCLUDE_DIR` → `/usr/local/include/python3.5m`
> `PYTHON_LIBRARY` → `/usr/local/lib/libpython3.5m.so`
> `VTK_INSTALL_PYTHON_MODULE_DIR` → `/usr/local/lib/python3.5/site-packages`
> `VTK_PYTHON_VERSION` → `3.5`
> `VTK_WRAP_PYTHON` → `ON`
> `VTK_WRAP_TCL` → `ON`

の行まで移動したらEnterキーを押します。

ONになったことを確認したら `[c]` を押し、configure を実行します。

エラーが発生しなかった場合 `[g]` オプションが表示されるので `[g]` を押し、Makefileを生成します。

成功したのに `[g]` が表示されない場合は、もう一度 `[c]` をすると出ることがあります。

```shell
$ make
$ make install

$ cd Wrappers/Python
$ make
$ make install

$ cd Wrappers/PythonCore
$ make
$ make install

$ ldconfig
```

# CMake
デフォルトで yum で入る CMake は古いため、Python 3.5.2 を認識してくれないので最新版をビルドします。
ここでインストールする CMake は `/usr/local/bin/cmake` に配置します。

```shell
$ cd /usr/local/src
$ git clone https://github.com/Kitware/CMake.git

$ cd CMake
$ ./bootstrap && make && make install
```

# OpenCV
```shell
$ cd /usr/local/src
$ git clone https://github.com/opencv/opencv.git

$ cd opencv
$mkdir build

$ cd build
$ /usr/local/bin/cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D BUILD_opencv_python3=ON \
-D BUILD_NEW_PYTHON_SUPPORT=ON \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D PYTHON_EXECUTABLE=$(which python) \
-D PYTHON3_EXECUTABLE=$(which python3.5) \
-D PYTHON3_LIBRARY=/usr/local/lib/libpython3.5m.so \
-D PYTHON3_INCLUDE_DIR=/usr/local/include/python3.5m \
-D PYTHON3_PACKAGES_PATH=/usr/local/lib/python3.5/site-packages \
-D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv/modules ..
$ make -j8
$ make install
```

# 確認
```shell=
$ python3.5
Python 3.5.2 (default, Sep 18 2016, 11:51:17)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
```
