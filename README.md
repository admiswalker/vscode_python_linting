# vscode_python_linting

このリポジトリでは，VSCode の Workspace に，Python コードのリンティング環境を構する．VSCode の設定ファイル（`settings.json`）は，ワークスペースの `.vscode` ディレクトリ下に設置する．これにより，プロジェクトの git リポジトリ下で，リンティングの設定を管理する．また，リンティングに使う Python パッケージは venv に requirements.txt で一括インストールし，settings.json で venv にパスを通すことで，環境の違いによる動作不良を軽減する．

## settings.json の設定
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
}
```

## lintter と formatter のインストール
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

## docstring のインストール
1. VSCode の Extention で `autoDocstring` をインストールする

## スペルチェッカーのインストール
1. VSCode の Extention で `Code Spell Checker` をインストールする

## 参考資料
- [VSCodeのPython環境とユーザ/ワークスペース周りの設定](https://qiita.com/tamo_breaker/items/132c219d4e20105d44da)
- [VS Code コーディング規約を快適に守る](https://qiita.com/firedfly/items/00c34018581c6cec9b84)
- [Pythonのコードフォーマッターについての個人的ベストプラクティス](https://qiita.com/sin9270/items/85e2dab4c0144c79987d)