# WSLにpyenvとpyenv-virtualenvを導入し、Python3環境を構築する

## 概要

WSL(Windows Subsystem for Linux)にpyenvとpyenv-virtualenvを導入し、Python3環境を構築する作業手順を解説します。

## ハードウェア

今回使用するハードウェアは次の機種です。

    機種名：Panasonic Let's noote LX3ED0CS
    モニタ：14.0型フルHD液晶(TN)
    CPU：Core i5-4300U
    メモリ：8GB
    ストレージ：240GB(SSD)

## ソフトウェア

今回使用するソフトウェアは次のバージョンです。

    OS：Windows 10 Pro (1909)
    Ubuntu：18.04.4 LTS (Bionic Beaver)

## Python関連のバージョン

今回は次のバージョンを導入しました。

    Python          3.6.1
    numpy           1.12.1
    scipy           0.19.0
    scikit-learn    0.18.1
    matplotlib      2.0.2
    pandas          0.20.1
    jupyter         1.0.0

## WSLの有効化

    1. 「コントロールパネル」を起動する
    2. 「プログラムと機能」の下で「Windowsの機能の有効化または無効化」をクリックする
    2. 表示されたダイアログの中から「Windows Subsystem for Linux」をONにする
    3. [OK]をクリックしてPCを再起動する

## Ubuntuのインストール

    1. 「Microsoft store」を起動して[検索]から「ubuntu」を検索する
    2. 「ubuntu」を選択し、インストールする

## Linuxのバージョン

cat /etc/os-releaseを実行すると、次の内容が表示されました。

    NAME="Ubuntu"
    VERSION="18.04.4 LTS (Bionic Beaver)"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 18.04.4 LTS"
    VERSION_ID="18.04"
    HOME_URL="https://www.ubuntu.com/"
    SUPPORT_URL="https://help.ubuntu.com/"
    BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
    PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
    VERSION_CODENAME=bionic
    UBUNTU_CODENAME=bionic

## パッケージの更新

    $ sudo apt-get update
    $ sudo apt-get upgrade -y
    $ sudo apt-get dist-upgrade
    $ sudo apt-get autoclean

## ビルド環境の導入

ソースから落としたPythonをビルドするためのツールやライブラリを導入します。

    $ sudo apt-get install zlib1g-dev libssl-dev libsqlite3-dev libbz2-dev libncurses5-dev libgdbm-dev liblzma-dev libssl-dev tcl-dev tk-dev libreadline-dev

## pyenvとpyenv-virtualenvの導入

複数のPythonを切り替えて使えるように、plenvを導入します。

    $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv

続けてpyenv-virtualenvを導入します。

    $ git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv

## 環境変数の設定

.bashrcの最下行に次のコードを追加します。

    $ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
    $ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
    $ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\n  eval "$(pyenv virtualenv-init -)"\nfi' >> ~/.bashrc

その後、sourceコマンドで.bashrcをリロードします。

    $ source ~/.bashrc

## Pythonの導入

今回は、書籍「Python機械学習 第2版」の環境を導入したいので[3.6.1]を導入します。

    $ pyenv install 3.6.1

## バージョンの有効化

globalコマンドで導入したバージョンを有効にして動作確認します。

    $ pyenv global 3.6.1
    $ pyenv rehash
    $ python --version

## virtualenvの設定

ここでは「sample」というフォルダを作成し、このフォルダに移動したら自動的に「python 3.6.1」に切り替わる設定を行います。

    $ mkdir sample
    $ cd sample
    $ pyenv virtualenv 3.6.1 sample
    $ pyenv local sample

上記の設定後はプロンプトの左端に(sample)と表示されるようになります。

## pipの動作確認

pipが導入されていることを確認します。

    $ pip list

## pipとsetuptoolsの更新

下記のコマンドで最新バージョンに更新します。

    $ pip install --upgrade pip
    $ pip install --upgrade setuptools

## パッケージの導入

参考書籍に提示されていたバージョンを入れています。

    $ pip install numpy==1.12.1
    $ pip install scipy==0.19.0
    $ pip install scikit-learn==0.18.1
    $ pip install matplotlib==2.0.2
    $ pip install pandas==0.20.1
    $ pip install jupyter==1.0.0

## jupyter notebookの起動

.bashrcの最下行に利用するブラウザのフルパスを登録して再読み込みします。

    $ echo 'export BROWSER="/mnt/c/Program Files (x86)/Google/Chrome/Application/chrome.exe"' >> ~/.bashrc
    $ source ~/.bashrc

jupyter notebookを起動して、表示されたURLをコピーして、起動したブラウザのアドレス欄に貼り付けます。

    $ jupyter notebook
    ＊ コンソールに「http://localhost:8888/?token=b66183f709c2304f12ca4ce15564772261fd49a2d0589296」のように表示されたアドレスをコピーしてブラウザに貼り付ける

このとき自動起動したブラウザに「ファイルが見つかりませんでした」と表示される件については対応策を調査中です。
