# MacでPythonの開発環境構築
- 2024/08/03
- Apple M1 Mac

## 環境構築手順
### 1. Homebrewをインストール
パッケージ管理システムを[公式サイト](https://brew.sh/ja/)からインストール。

インストール用のコマンドを実行。
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
HomebrewにPATHを追加するため、==> Next steps:に表示されるコマンドを複数実行。

### 2. xzをインストール
pyenvで特定バージョンのpythonをインストールする際、エラーが起こるためxzを予めインストール。
```
brew install xz
```

### 3. pyenvをインストール
pyenvを、Homebrewでインストール。
```
brew install pyenv
```
pyenvのPATHを通すため、各環境に合わせたコマンドを複数実行。

Homebrewの[公式Github](https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv)で確認可能。

zshのため、以下のコマンドを実行。
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

その後、sourceコマンドで変更を反映。
```
source ~/.zshrc 
```

### 4. Pythonをインストール
pyenvで提供されているpythonのバージョン一覧を確認。
```
pyenv install --list
->
3.12.4
...
```

表示されるバージョンの中から選択し、pythonをインストール。
```
pyenv install 3.12.4
```
（アンインストールしたい場合は、以下のコマンドを実行。）
```
pyenv uninstall 3.12.4
```

## 仮想環境の作成
任意の作業用ディレクトリに移動。
### 1. pyenvでPythonのバージョンを指定
pyenvでインストールしたPythonのバージョン一覧を取得。
```
pyenv versions
->
* system (set by /****/.pyenv/version)
  3.12.4
```
取得した一覧の中から、バージョンを指定。
```
pyenv local 3.12.4
```
```pyenv local```でディレクトリに指定されたPythonのバージョンを確認。
```
python3 --version
->
Python 3.12.4
```

### 2. venvで仮想環境を作成
プロジェクトに合わせた任意の名前を指定し、venvで仮想環境を作成。<br>
今回の例では、```.venv```という環境名で作成。
```
python3 -m venv .venv
```
作業ディレクトリの```.venv```というディレクトリ以下に仮想環境が作成される。<br>
その後、venvで作成した仮想環境```.venv```を起動。
```
source .venv/bin/activate
```

### 3. 仮想環境にパッケージをインストール
仮想環境起動後に実行するpython3コマンドの影響範囲は、仮想環境内のみに限定。<br>
パッケージをインストールする前に、仮想環境のpipをアップデート。
```
(.venv)python3 -m pip install --upgrade pip
```
次に、パッケージをインストール。今回の例では、スクレイピングライブラリ(Beautifulsoup)をインストール。
```
(.venv)pip install beautifulsoup4
```
パッケージ一覧を確認し、正常にインストールされていることを確認。
```
(.venv)pip list
->
Package        Version
-------------- -------
beautifulsoup4 4.12.3
pip            24.2
soupsieve      2.5
```

### 4. pythonファイルで実行確認
任意の.pyファイルを作成し、先ほどインストールしたパッケージを確認。
今回はtest.pyを作成し、以下のコードを書き込む。
```
from bs4 import BeautifulSoup
```
次に、python3で正常に実行できることを確認。
```
(.venv)python3 test.py
```
確認後、仮想環境を閉じて再度実行し、エラーが起きることを確認。
```
(.venv)deactivate
```
```
python3 test.py
->
ModuleNotFoundError: No module named 'bs4'
```
このエラーから、ローカル環境にはインストールされていないことが確認できた。<br>
以上の手順で、MacでPythonの開発環境および仮想環境の構築が完了。<br>