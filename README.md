# Sudoku_Solver_Generator_v4.2 beta
  アップしました(20230328)。安定までには、しばらくかかります。５月頃、再度訪れてください。


# Sudoku_Solver_Generator
![GNPX](/images/GNPX_start.png)

数独の問題を 解く・作る C#プログラムです。
解析アルゴリズムは、全て論理的手法で、バックトラックは使いません。
以下のアルゴリズムを実装しています。  

>Single, LockedCandidate, (hidden)LockedSet(2D-7D),
 (Finned)(Franken/Mutant/Kraken)Fish(2D-7D),
 Skyscraper, EmptyRectangle, XY-Wing, W-Wing, RemotePair, XChain, XYChain,
 SueDeCoq, (Multi)Coloring,
 ALS-Wing, ALS-XZ, ALS-Chain,
 (ALS)DeathBlossom(Ext.), (Grouped)NiceLoop, ForceChain and
 GeneralLogic.

プログラム機能として、単解析、複数解析、数独問題作成（数独のパターン、レベル指定が可能）、問題の変換・数独問題の標準化ができます。


## ダウンロード、日本語化
プログラムは、環境に応じて（または手動で）英語と日本語に切り替わります。
ソースコード（VSプロジェクト）は、英語版 WEB ページからダウンロードしてください。  
https://github.com/GIDOO-code/Sudoku_Solver_Generator_v4
<br>
なお、圧縮アプリは、必ずしも最新版ではありません。最新版は、GNPXのC#ソースコード・プロジェクトです。



## 実行方法
ダウンロード・解凍したら、　フォルダー”/SUDOKU_App/net7.0....”内の　アプリ”SUDOKU_Regular.exe”のエイリアス（ショートカット）をつくり、これを１段上のフォルダー”SUDOKU_App”に移動する。　エイリアスのダブルクリックで実行する。<br>
最初の実行時にwindowsの”危険”警告がでる。
できれば、GNPXのC#ソースコード・プロジェクトをダウンロードして、内容を確認してから実行するのが良いでしょう。
SUDOKUをプログラムで解くのを趣味とする方が、また１人増えるので、私はこの方が良い。