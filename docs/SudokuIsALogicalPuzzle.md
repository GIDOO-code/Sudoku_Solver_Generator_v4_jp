***
# 数独は論理パズル
### (数独を解くのに、バックトラック法を用いない理由)
数独は論理なパズルです。解くには、次の手順を繰り返す。
1) 数独解法アルゴリズムは、各ステップで「ロックされた(Locked)」パターンを探す。
2) 「ロックされた(Locked)」パターンから、これに関係する候補数字の真偽が決まる。
3) 次のステップに進む。
***

## 制約
数独は、縦/横/ブロックに1～9の数字を配置する。
セルには、1～9の数字のどれかを配置する。
従って、数独には縦/横/ブロック/セルの合計324の制約がある。
縦/横とブロックの制約は、独立ではない。
セルの縦/横/ブロック/セルを表すのに、次の図の記号を用いる。<br>
<img src="images/puzzle/CellBlock1.png" height="250"/><br>
***

# ステップ（フェーズ）
数独を解く中間段階のステップ（フェーズ）と呼ぶ。
途中段階のステップでは、一部のセルには未確定の数字（候補）がある。
数独アルゴリズムは、未確定セルの候補数字の真あるいは偽を決定する。
これにより、次のステップに進む。<br>
<img src="images/puzzle/NextPhase.png" height="250"/><br>
***


## Link (simple link)
未確定セルの候補数字は、行/列/ブロックの制約が"link"を構成する。
解読段階の1つのステップでは、多くの link が存在する。

具体例を示す。数独の1つのフェーズに注目すると、未確定のセルには候補数字がある。
各々の候補の数字は、行/列/ブロック/セルの制約で関連付けられた Link を形成する。
次の図に示す Link は、ほんの一例です。<br>
<img src="images/puzzle/SW_Link_exsample.png" width="300"/><br>


2セルのリンクは "Strong link" と呼ばれる。
3セル以上のリンクは "Weak link" と呼ばれる。
”Strong link" と "Weak link" のイメージを以下に示す。

Strong linkでは、一方は真で他方は偽となる。
<img src="images/puzzle/StrongLink.png" height="250"/><br>

Weak linkでは、どれかが真で、残りの全てが偽となる。
どれかが偽のとき、残りのどれが真であるかは決まらない。
なお、Strong link は Weak link でもあり、
アルゴリズムで Weak link が求められるとき、Strong link を用いる場合もある。 
<img src="images/puzzle/WeakLink.png" height="250"/><br>



## Link-2 (ALS, AIC)
数独アルゴリズムは、linkの組み合わせで構成されている。
ALS(AlmostLockedSet) と AIC(Alternate Inference Chain) は、linkの拡張である.

ALSは、行/列/ブロックが共通のn個のセルに n+1個の候補数字がある状態です。
ALSは、同じ行/列/ブロックの制約の集合体ですが、新しいタイプのリンクと理解するのが良いだろう。<br>
<img src="images/puzzle/ALS.png" height="250"/><br>

ALSは、数独を解く上での強力な手段であり、多くの解析アルゴリズムで使われている。<br>
<img src="images/puzzle/ALS_Locked.png" height="250"/><br>

AICは、行（列）とブロックの制約が重なる部分のリンクです。
たとえば、候補の数字が次のように配置されている場合、Weak linkとなる。
セル間リンクで構成する NiceLoop などのアルゴリズムは ALS-Link、AIC-Link 
を使うアルゴリズムに拡張できる。<br>
<img src="images/puzzle/AIC_Link.png" height="250"/><br>
***


## Locked
数独アルゴリズムを理解するには、次のことが重要です。
数独の数字が確定した状態、または候補の一部の数字が真または偽となったことを、Locked状態という。
ここでは、Locked を概念として理解してください。
以下に、いくつかの Locked状態 を示す。<br>
解析アルゴリズムは、次のHTMLページで解説している。 
https://gidoo-code.github.io/Sudoku_Solver_Generator/page2.html<br>

### 1)Single
”1セルに候補数字が1つの状態は数字が確定する”ことは、数独の定義から自明である。<br>
<img src="images/puzzle/Single.png" height="400"/><br>

### 2)1D LockedCandidate
数独では、行/列とブロックの制約は独立ではない。
候補が行/列とブロックが重なる位置で限定されている場合、Locked になる。
 Locked を壊す候補数字は偽となる。<br>
<img src="images/puzzle/LockedCandidate1.png" height="275"/><br>
<img src="images/puzzle/LockedCandidate2.png" height="275"/><br>

### 3)2D LockedSet
2つの候補のあるセルが Link で連結しているとき、Locked になる。

最初の例：
それぞれのセルでは、2つの候補の真あるいは偽は決定していないが、いずれかが真であることは決定している。
3セルと3候補、4セルと4候補でも同様の Locked状態 が成立する。

2番目の例：
2セルr48c6(=r4c6,r8c6) 2候補数字 #16(=#1,#6) で Locked になる。
また、同時に3セルr579c6は 3候補数字 #345 で Locked になる。
これら2つの LockedSet は双対の関係にある。
実際、LockedSet は双対の Locked が常に生じている。これより、5次(5D)以上の LockedSet アルゴリズムは必要ない。<br>
<img src="images/puzzle/LockedSet2.png" height="250"/><br>
<img src="images/puzzle/LockedSet3.png" height="250"/><br>
<img src="images/puzzle/LockedSet4.png" height="250"/><br>

### 4)Fish
以下の例は、X-wing (2D-Fish)アルゴリズムで、2つのlinkの候補数字が、別の2つのLinkでカバーされている。
一般的に、n個の link群(BaseSet) が、異なるn個の link群(CoverSet) でカバーされている状態は、Locked になる。 
BaseSetとCoverSetのリンク群の要素Linkは、行/列/ブロックのLinkであり、かつこれらは混在しても成立する。
(Franken-Fish,Mutant-Fish)
BaseSetとCoverSetの論理は、HPで解説している。
<https://gidoo-code.github.io/Sudoku_Solver_Generator_jp/page36.html>"Franken/Mutant Fish"
<https://gidoo-code.github.io/Sudoku_Solver_Generator/page36.html>"Franken/Mutant Fish"<br>
<https://gidoo-code.github.io/Sudoku_Solver_Generator_jp/page60.html>"GeneralLogic"
<https://gidoo-code.github.io/Sudoku_Solver_Generator/page60.html>"GeneralLogic"<br>
<img src="images/puzzle/Fish1.png" height="250"/><br>

### 5)Skyscraper
<img src="images/puzzle/Skyscraper.png" height="250"/><br>

### 6)Sue De Coq
2つのALSと候補数字がある特定の条件を満たすとき、一見奇妙な Locked になる。
多くの場合、他のもっと簡単な解法で解けるので、"Sue De Coq" が必要なケースは稀である。<br>
<img src="images/puzzle/SueDeCoq.png" height="250"/><br>

### 7)another
ここに示したアルゴリズムの他にも多数の解析アルゴリズムが知られている（以下のHPなど)。
また、C#プログラムをダウンロードするのもよいでしょう。
<https://gidoo-code.github.io/Sudoku_Solver_Generator_jp/page2.html> ”Sudoku Algorithm”
<https://gidoo-code.github.io/Sudoku_Solver_Generator/page2.html> ”Sudoku Algorithm”
<https://gidoo-code.github.io/Sudoku_Solver_Generator> ”Sudoku C#Program”<br>
***


# まとめ.
### 数独に背理法(バックトラック法)を用いない理由
* 数独解析アルゴリズムは、リンクと拡張リンクの組み合わせで構成される。
* 数独解析アルゴリズムで使用するリンクと拡張リンクは、すべて現在局面のリンクである。
* Locked状態から、候補数字の真または偽が決まる。
  
背理法では、候補数字の真または偽を仮定すると、そのたびに次の局面に進み、新たな局面の (拡張)Linkを用いて解析を進める。
#### 背理法は、知的でなくつまらないので、用いない。
***

# 補足
数独アルゴリズムのアルゴリズムは、"General Logic" にまとめられる。<br>
<http://www.sudokuone.com/sweb/general.htm>"A General Logic for Sudoku"
<https://gidoo-code.github.io/Sudoku_Solver_Generator_jp/page60.html> ”Sudoku Algorithm”
<https://gidoo-code.github.io/Sudoku_Solver_Generator/page60.html> ”Sudoku Algorithm”
ただし、"General Logic"は一般アルゴリズムであるがために効率的ではない。効率の改良が課題である。
***
