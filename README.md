# windows 安装启动
0. 本地已经安装nodejs
1. 下载ruby [rubyinstaller-2.3.3-x64.exe](https://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.3.3-x64.exe), 并安装（高版本可能有问题）。
2. 下载devkit [DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe](https://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe), 并安装。
3. 打开cmd，进入devKit安装目录
```shell
D:\>cd soft
D:\soft>cd devkit
D:\soft\devkit>ruby dk.rb init  # 执行该命令后会生成 config.yml， 修改文件，添加ruby的目录
D:\soft\devkit>ruby dk.rb install
```
4. 点击msys.bat
```shell
gem install bundler
```
5. 下载[slate](https://github.com/lord/slate),修改Gemfile，追加下面一行
```shell
gem 'wdm', '>= 0.1.0'
```

6. 继续执行
```shell
cd /d/xxx/slate
bundle install
bundle exec middleman server #启动
#bundle exec middleman build --clean #生成静态页
``` 

# linux 安装启动
0. 安装nodejs 
```shell
apt-get install nodejs
```
1. 下载[ruby](https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.5.tar.gz)，下载[slate](https://github.com/lord/slate)
2. 安装ruby，执行命令
```shell
$ wget https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.5.tar.gz
$ tar zxvf ruby-2.3.5.tar.gz
$ cd ruby-2.3.5/
$ ./configure  --with-openssl-dir=/*此处填写openssl的目录*/
$ make
$ make install
$ cd /home/xxx/slate/
$ gem install bundler
$ bundle install
$ bundle exec middleman server #启动
$ bundle exec middleman build --clean #生成静态页
```

## 异常情况处理
```shell
$ gem install bundle
ERROR:  Loading command: install (LoadError)
        cannot load such file -- zlib
ERROR:  While executing gem ... (NoMethodError)
    undefined method `invoke_with_build_args' for nil:NilClass
```
解决办法
```shell
$ cd ruby-2.3.5/ext/zlib
$ ruby ./extconf.rb
$ make
$ make install
```
然后重新编译ruby

# 使用
进入项目的source目录修改index.html.md相关内容