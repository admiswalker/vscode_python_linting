# vscode_python_linting

このリポジトリでは，
Python でチーム開発するのに必要な VSCode のリンティングの設定を，
VSCode の Workspace 下に構築する．
Workspace に設定することで，必要な設定を git で追跡してチームで共有できるようにする．

具体的なリンティングの設定は，`.vscode/settings.json` に設置する．また，リンティングに使う Python パッケージは venv に requirements.txt で一括インストールし，settings.json で venv にパスを通すことで，環境の違いによる動作不良を軽減する．

また，チームで必要となる VSCode の Extention は，`.vscode/extensions.json` に記録して git で追跡する．`.vscode/extensions.json` に記録することで，メンバーが必要な Extension を（少しだけ）探しやすくなる．

## 構成

lintter と formatter には，それぞれ flake8 と black を使う．

追加の機能として，docstring の入力補完に `autoDocstring` を，スペルチェック `Code Spell Checker` を，それぞれ，VSCode の Extension としてインストールする．

## 使い方

1. リポジトリを clone
   ```bash
   git clone git@github.com:admiswalker/vscode_python_linting.git
   cd vscode_python_linting
   ```
1. Python 環境の構築  
   下記のコマンドを実行する．`.venv_vscode` ディレクトリに，python コードの linting に必要なパッケージがインストールされる．
   ```bash
   .vscode/install_pyenv.sh
   ```
1. VSCode の Extension のインストール
   1. VSCode の Extensions をクリック
   1. `@recommended` で検索
   1. 表示された Extension を全てインストールする（雲のマークをクリックすると，一括インストールできる）



## 参考資料
- [VSCodeのPython環境とユーザ/ワークスペース周りの設定](https://qiita.com/tamo_breaker/items/132c219d4e20105d44da)
- [VS Code コーディング規約を快適に守る](https://qiita.com/firedfly/items/00c34018581c6cec9b84)
- [Pythonのコードフォーマッターについての個人的ベストプラクティス](https://qiita.com/sin9270/items/85e2dab4c0144c79987d)
- [チームで推奨するVSCode拡張機能を共有するtips](https://future-architect.github.io/articles/20200828/)

## 付録
### 本環境を構築した手順

#### settings.json の設定
後の設定で利用する settings.json を先に用意する

`.vscode/settings.json` ファイルを作成し，下記を書き込む．
```json
{
    "python.defaultInterpreterPath": ".venv_vscode/bin/python3",
    "python.terminal.activateEnvInCurrentTerminal": false,
    "[python]": {
        "editor.defaultFormatter": null,
        "editor.formatOnSave": true,
        "editor.tabSize": 4
    },
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 1000,
    "python.linting.lintOnSave": true,
    "python.linting.pylintEnabled": false,
    "python.linting.pep8Enabled": false,
    "python.linting.flake8Enabled": true,
    "python.linting.flake8Args": [
        "--ignore=W293, W504",
        "--max-line-length=150",
        "--max-complexity=20"
    ],
    "python.formatting.provider": "black",
    "python.formatting.autopep8Args": [
        "--aggressive", "--aggressive",
    ],
    "autoDocstring.docstringFormat": "google",
    "extensions.ignoreRecommendations": false,
}
```

#### lintter と formatter のインストール
1. VSCode の Extention で `Python` をインストールする  
   venv など，仮想環境の Python を使う場合でも，VSCode の Extentionとして事前に Python をインストールしないと，
   settings.json で `python.defaultInterpreterPath` などの設定が有効にならない．
1. Python コードの Linting に必要な Python のパッケージを venv インストールする
   1. 必要なパッケージを requirements.txt に記載する
      ```bash
      echo "flake8" >> requirements.txt
      echo "black" >> requirements.txt
      ```
   1. .venv_vscode にパッケージをインストールする
      ```bash
      python3 -m venv .venv_vscode
      . .venv_vscode/bin/activate
      pip install -r requirements.txt
      ```

#### 推奨する Extension の登録
1. Extension のページを開く
1. Ctrl + Shift + P を押す
1. add と入力して，"Add Extension to Workspace Folder Recommendations" で Enter を押す  
   ここでは，方針に従い `autoDocstring` と `Code Spell Checker` を登録する．

なお，登録された Extention が，右下のホップアップからレコメンドされるように，
`settings.json` で `"extensions.ignoreRecommendations": false,` としてある．

#### VSCode の Extension のインストール
1. VSCode の Extensions をクリック
1. `@recommended` で検索
1. 表示された Extension を全てインストールする（雲のマークをクリックすると，一括インストールできる）
