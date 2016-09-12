title: 编译安装GCC 5.2.0
date: 2015-12-13 18:28:28
tags: C/C++
categories: C/C++
---
记录编译GCC 5.2.0时遇到的问题和解决方法，以备日后查询。

使用的服务器是CentOS6，自带的gcc编译器4.4.2版本，要是完全支持c++11需要4.8版本以上的，完全没法写C++11的代码，因为不能升级操作系统也没有root权限，只能自己下载源码编译。

安装过程只好记录下来。

### 安装依赖库
GCC依赖于gmp 4.2+, mpfr 2.4+和mpc 0.8+，这里直接下载安装最新的版本。

我将依赖库装在/home/users/xk/gcc-5.2目录下


#### 安装gmp 6.0
wget https://gmplib.org/download/gmp/gmp-6.0.0a.tar.bz2
tar xvf gmp-6.0.0a.tar.bz2
cd gmp-6.0.0
./configure --prefix=/home/users/xk/gcc-5.2
make -j4
make check
make install

#### 安装mpfr 3.1.3
mpfr依赖于gmp。

wget http://www.mpfr.org/mpfr-current/mpfr-3.1.3.tar.bz2
tar xvf mpfr-3.1.3.tar.bz2
cd mpfr-3.1.3
./configure --prefix==/home/users/xk/gcc-5.2 \
	--with-gmp-include=/home/users/xk/gcc-5.2/include \
	--with-gmp-lib=/home/users/xk/gcc-5.2/lib
make -j4
make check
make install

#### 安装mpc 1.0.3
mpc依赖于gmp和mpfr。
wget ftp://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz
tar xvf mpc-1.0.3.tar.gz
cd mpc-1.0.3
./configure --prefix==/home/users/xk/gcc-5.2 \
--with-gmp-include=/home/users/xk/gcc-5.2/include \
--with-gmp-lib=/home/users/xk/gcc-5.2/lib \
--with-mpfr-include=/home/users/xk/gcc-5.2/include \
--with-mpfr-lib=/home/users/xk/gcc-5.2/lib 
make -j4
make check
make install



### 编译和安装GCC

建议先阅读下官方的安装文档。

#### 下载GCC并解压。

wget ftp://ftp.gnu.org/gnu/gcc/gcc-5.2.0/gcc-5.2.0.tar.bz2
tar xvf gcc-5.2.0.tar.bz2
cd gcc-5.2.0

在/home/users/xk/编辑 .bashrc, 添加LD_LIBRARY_APTH=/home/users/xk/gcc-5.2/lib

#### 配置GCC

./configure \
	--prefix=--with-gmp-include=/home/users/xk/ \
	--with-gmp-include=/home/users/xk/gcc-5.2/include \
	--with-gmp-lib=/home/users/xk/gcc-5.2/lib \
	--with-mpfr-include=/home/users/xk/gcc-5.2/include \
	--with-mpfr-lib=/home/users/xk/gcc-5.2/lib \
	--with-mpc-include=/home/users/xk/gcc-5.2/include \
	--with-mpc-lib=/home/users/xk/gcc-5.2/lib \
	--enable-languages=c,c++ \
	--enable-threads=posix \
	--disable-multilib
	
详细的配置项说明可参考安装文档，这里只编译c和c++的编译器。

#### 编译GCC
后运行如下命令:

make -j8

#### 安装GCC
如果编译顺利通过，make install即可。

gcc和g++被安装到/home/users/xk/bin目录下，libgcc和libstdc++被安装到/home/users/xk/lib64(x64)。

#### 配置 .bashrc

vim ~/.bashrc

export LD_LIBRARY_PATH=/home/users/xk/gcc-5.2/lib:/home/users/xk/lib:/home/users/xk/lib64
export CPLUS_INCLUDE_PATH=/home/users/xk/include:/home/users/xk/boost-1-58/include
export GXX_ROOT=/home/users/xk/lib/gcc/x86_64-unknown-linux-gnu/5.2.0/
export C_INCLUDE_PATH=/home/users/xk/include:/home/users/xk/boost-1-58/include

source ~/.bashrc

### 测试
写一个简单的cc测试下新安装的编译器。
``` c++
#include <atomic>
#include <regex>
#include <iostream>
using namespace std;

int main() {
    atomic<long long> num(1L << 14);
    cout << ++num << endl;

    regex r("[0-9]+");
    string s("0abc11abc222cba");
    sregex_iterator ib(s.begin(), s.end(), r);
    sregex_iterator ie;
    cout << "search numbers in: " << s << endl;
    for (sregex_iterator i = ib; i != ie; ++i) {
        cout << "match: " << i->str() << endl;
    }
}
```
编译并运行:

g++ -std=c++11 test.cc -o test
./test

