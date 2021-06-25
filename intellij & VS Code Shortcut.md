# Intellij & VS Code Shortcut
intellij与vs code比较
https://gist.github.com/masa7351/b79e7ec1f3a55318096d7d4f573d4f28
# テキストエディター

| 目的                                                      | Visual Studio Code                                           | IntelliJ                       | 補足                                                         |
| --------------------------------------------------------- | :----------------------------------------------------------- | ------------------------------ | ------------------------------------------------------------ |
| 行複製                                                    | Option + Shift + ↑ or ↓                                      | Command + D                    | Option + Shift + ↑ or ↓はIntelliJでは行入れ替えのショートカットキー |
| 行削除                                                    | Command + Shift + K<br/>Command + X ※<br/>※ Clipbordに登録されるので、Command + Vで貼り付けできる | Command + Delete               |                                                              |
| 行追加<br />※行末にいない状態で追加                       | Command + Enter<br/>Command + Shift + Enter ※<br/>※ Shiftをつけてると、上に行が追加される | Shift + Enter                  |                                                              |
| 行入れ替え                                                | Option + ↑ or ↓                                              | Option + Shift + ↑ or ↓        |                                                              |
| マルチカーソル                                            | Option + Command + ↑ or ↓                                    | -                              |                                                              |
| マウスを使ったマルチカーソル                              | Option + Shift を押しながらカーソル移動                      | Optionを押しながらカーソル移動 |                                                              |
| コード整形                                                | -                                                            | Command + Option + L           | Visual Studio CodeはPrettierプラグインを入れることで、ファイル保存時に自動的にコード整形を行ってくれる |
| 行末まで削除                                              | Control + K                                                  | ？？？                         |                                                              |
| 不要なImportを削除                                        | -                                                            | Control + Option + O           |                                                              |
| インポート(※インポート対象部分にカーソルを移動させて実行) | -                                                            | Option + Enter                 |                                                              |
| 現在選択している他のワードを選択状態にする                | Command + D                                                  | Ctrl + G                       | ブロック内の同一ワードのリネームとして活用                   |
| コード折りたたみ                                          | Option + Command + [ or ]                                    | Command + 「+」or「-」         |                                                              |
| カーソルを行先頭 / 行末に移動                             | Command + [ or ]                                             | Command + [ or ]               | Mac共通のショートカット<br /><br />Shiftを組み合わせると選択状態でカーソル移動 |
| カーソルを単語単位で移動                                  | Option + [ or ]                                              | Option + [ or ]                | Mac共通のショートカット<br /><br />Shiftを組み合わせると選択状態でカーソル移動 |



# 検索

| 目的             | Visual Studio Code  | IntelliJ            | 補足 |
| ---------------- | ------------------- | ------------------- | ---- |
| ファイル名検索   | Command + P         | -                   |      |
| ファイル横断検索 | Command + Shift + F | Command + Shift + F |      |
| なんでも検索     | -                   | Shiftを2回連打      |      |



# ウィンドウ

| 目的                  | Visual Studio Code        | IntelliJ                                                     | 補足                 |
| --------------------- | ------------------------- | ------------------------------------------------------------ | -------------------- |
| サイドバーの開閉      | Command + B               | Command + 1                                                  |                      |
| タブ移動              | Option + Command + ← or → | Command + Shift + [ or ]                                     |                      |
| 2ファイル間のタブ移動 | -                         | Command + [ or ]                                             |                      |
| ウィンドウ分割        | Command + 2, 3, 4         | Command + Shift + A でアクション窓を開いて、splitと入力で画面分割 | 縦・横分割の選択可能 |
| ターミナル開閉        | Ctrl + `                  | Option + F12                                                 |                      |
| タブを閉じる          | Command + W               | Command + W                                                  |                      |



# IntelliJその他ショートカット



| 目的                                       | ショートカット               |                                                              |
| ------------------------------------------ | ---------------------------- | ------------------------------------------------------------ |
| 実行                                       | Option + Control + R         | Run実行可能な行にカーソルを移動して、Option + Enterでも実行することもできる |
| 最近開いたファイルを表示                   | Command + E                  |                                                              |
| 選択箇所をfor, while, try …で囲む          | Command + Option + T         |                                                              |
| 選択範囲の拡大/縮小                        | Option + ↑ or ↓              |                                                              |
| 宣言へジャンプ                             | Command + B                  |                                                              |
| 実装クラスへジャンプ                       | Command + Option + B         |                                                              |
| 使用されている箇所へジャンプ(クラス、変数) | Command + Option + F7        |                                                              |
| 呼び出し先を調べる                         | Option + Ctrl + H            |                                                              |
| メソッドに切り出す                         | Command + Option + M         | Methodの頭文字のM                                            |
| フィールドとして宣言に切り出す             | Command + Option + F         | Fieldの頭文字のF                                             |
| 継承関係をクラス図で表示                   | Command + Option + U         |                                                              |
| リファクタリングメニュー表示               | Command + Option + Shift + T |                                                              |



# IDEオススメ設定

## IntelliJの自動Importの有効化

1. Preferenceを開く
2. General > Auto Import 
3. 「Add unambiguous imports on the fly」にチェックを入れる。



## Visual Studio Codeファイルを新しいタブで開くのをデフォルトにする

IntelliJは新しいファイルをタブとして開くのでVisual Studio Codeも挙動に合わせます。

1. Preferenceを開く
2. 検索ボックスに「workbench.editor.enablePreview」を入力する
3. チェックボックスのチェックを外す