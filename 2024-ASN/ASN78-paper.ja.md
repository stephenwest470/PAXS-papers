# 古人骨・mtDNA・民族学・考古学データを用いた<br>日本列島の人口動態Agent-based Simulation<br>―弥生時代前半期の渡来人・稲作伝播を対象に―
### 春日 勇人（日本人類学会会員）
### WEST Stephen（岡山大学社会文化科学研究科 D3）

---

# はじめに
弥生時代前半期における渡来人の人数と稲作伝播を対象に、古人骨・ゲノム・民族学・考古学データに基づいた人口動態Agent-based Simulation（ABS）の適用を試みた。その実験にABS機能を持つGISオープンソースソフトウェア「PAX SAPIENTICA」を使用した。結婚居住形態・渡来人の人数・稲作文化の継承に関する変数を定義してシミュレーションを行い、実行結果を弥生時代中期のゲノムデータと人口推定と比較した。比較分析により、各変数の妥当性を評価し、より実態に近い値を求めた。ABSの適用により、過去の人間社会に関する仮説を検証し、より明らかにできると考えている。

---

# 1. シミュレーションモデル
シミュレーションモデルの記述は、ODD（Overview、 Design concept、 and Details、概要・デザインコンセプト・詳細）プロトコル（Grimm et al. 2006・2010）に基づく。これはエージェントベースモデルが再現性に欠けるという批判に応えるため、説明の標準化と完全性の向上を目的としたものである（坂平・寺野2014、坂平2018）。

---

# 2. シミュレーションの目的
縄文晩期末から弥生時代中期にかけての地域別の人口数とその増加率、水田稲作の伝播、渡来系mtDNAの拡散を可視化することを目的とする。

---

# 3. エージェントと属性変数

## 3.1. 空間

### 3.1.1. 空間の大きさ
シミュレーション空間はＺ＝10のスケールとする。Ｚ＝10の時の１区画は世界地図を2^10分割した大きさ、１セルは世界地図を2^18分割した大きさとなる。
空間は本州島、四国島、九州島の全領域を扱う。具体的には範囲（区画）がＸ877から917まで、Ｙ381から421まで、区画の大きさがＸ41区画×Ｙ41区画、グリッドの大きさがＸ10496セル×Ｙ10496セルである。１セルは日本列島（正確には緯度35度の場合）では約125.3647ｍである。シミュレーション空間の範囲と区画に分割したグリッドを図🔢に示す。
坂平文博のABMではシミュレーション空間は北部九州を模した50セル×50セルであり、１セルは約２～３ｋｍであるため（坂平2018）、本論文のシミュレーション空間は16～24倍細かい単位である。また、及川遺跡推定で用いられた空間が１セル１ｋｍのため、本論文のシミュレーション空間は８倍細かい。つまり、本研究は先行研究より10倍程度細かいミクロなシミュレーション空間であることから集落動態との親和性が高くより密度の高い推定が出来ると考えられる。

### 3.1.2. 空間の分類
シミュレーション空間に用いる地図状の地理情報データは「傾斜、陸地、令制国」の🔢３個である。

### 3.1.3. 傾斜

### 3.1.4. 地区・陸地
地区は令制国で区分されている。

### 3.1.5. 可住地及び農耕可住地
人間と集団が生成される条件を満たすセルを可住地とする。人間と集団は可住地以外のセルに居住できないとする。可住地の条件は陸地（海や湖川ではない土地）かつ山地ではない傾斜区分（丘陵や平地）である。
農耕文化を持つ人間かつ集団は更に湖川から🔢（５？）ｋｍ以内とする。セル数でいうと湖川から🔢（40？）セル以内とする。この条件を満たすセルを農耕可住地と呼ぶ。

## 3.2. 人間エージェント
### 3.2.1. ID
固有のIDを持つ。婚姻関係のある相手にIDを渡すことで婚姻関係を識別する。

### 3.2.2. 所属集団
人間エージェントは集団エージェントに属する。

### 3.2.3. 性別
「男性」または「女性」とする。

### 3.2.4. 生業文化
「狩猟採集」または「水田稲作」とする。開始時の人間は全て狩猟採集民とし、渡来人は全て水田稲作民とする。

エージェントの寿命は生業文化によって定義されてあり、従って生業によって人口増加率が異なる。狩猟採集民の倍加時間＝約610年、増加率＝約0.1%。水田稲作民の倍加時間＝約50年、増加率＝約1.4%。坂平・寺野(2016)の「生業文化に基づく人口増加率」パターン１（狩猟採集民：年0.1%, 農耕民：年1.3%）とほぼ同様である。

### 3.2.5. 余命と年齢
縄文・弥生人骨を参考とする。

狩猟採集民エージェントの寿命は縄文人骨を基に、水田稲作民エージェントの寿命は弥生人骨に基づいて推定されている。

人骨データは九州大学の古人骨データベースより (http://db.museum.kyushu-u.ac.jp/anthropology/ )。児童の人骨データが不十分であったため、民族学・歴史学データに基づいたVolk & Atkinson (2013)の15歳までの乳幼児死亡率(CMR)推定を参考とした。狩猟採集民のCMRを50%、水田稲作民のCMRを45%とした。上記のデータに基づき、Rでモデル生命表を作成した (スクリプト：https://github.com/stephenwest470/Remains2LifeTable )。

人間は初期配置時及び出生時に、死亡率の確率分布に基づいて余命が付与される。月齢（１年で１増える年齢ではなく、１ヶ月で１増える月齢）が余命を超えた場合、人間は死亡し削除される。死亡率の確率分布は生業・性別による生命表の死亡率(dx)を使用する。

月齢は１ステップの終了時に１ヶ月分増加させる。

### 3.2.6. mtDNA
mtDNAの参考文献に基づいて、縄文時代・弥生時代の各地域のハプログループから決定する。渡来人のmtDNAは、弥生時代・古墳時代におけるアジア大陸由来のハプログループから決定される。ハプログループの頻度は重みとされ、確率的にエージェントのハプログループが決定される。

- 南西諸島：	M7a(6)
- 九州縄文：	M7a(21), M80(1), N9a(1), N9b(4)
- 中国縄文：	M7a(1)
- 四国縄文：	M7a(1), N9b(1)
- 近畿縄文：	N9b(1)
- 中部縄文：	D4b(1), G2(1), M7a(3), M9a(3), N9b(7)
- 関東縄文：	B4b(3), B4f(1), B5b(1), D4b(1), E1a(1), G2a(1), M10(2), M7a(14), M7b(1), M8a(1), N9b(42)
- 東北縄文：	D4b(1), D4h(1), M7a(13), N9b(13)
- 北海道縄文：	D4h(6), M7a(3), N9b(19)
- 渡来人： A5a(5), B4c(5), B5a(1), B5b(1), C1a(1), C5b(1), D4a(5), D4b(14), D4c(8), D4e(3), D4g(8), D4i(2), D4m(1), D5a(2), D5c(2), G1a(2), M7a(1)

### 3.2.7. Y-DNA
女性は値を0とする（保持しない）。

### 3.2.8. 由来ゲノム
縄文系由来のゲノムを0とし、渡来系由来のゲノムを1とする。
出生時に子供は両親の由来ゲノムの平均値を獲得する。

### 3.2.9. パートナー変数
婚姻関係のあるパートナーの変数を保持する。
- ID
- 生業文化
- ゲノム変数

## 3.3. 集団エージェント
集団は個別のＩＤを持ち、集団はＸＹＺタイルに準じた座標を持つ。

人間を保持する。
範囲は1セル四方（約125m四方）
セルごとの人口。完全な農耕集団は最大80人、完全な狩猟採集集団は最大25人。農耕民の存在コストは1/80、狩猟採集民の存在コストは1/25とし、1を超えた場合は半分の集団が近場（同じ地域内の別セル）に移動する。
前回とだいたい同じ。
生業文化によって若干仕様が変わる。

## 3.3.1. 規模による分類と居住人数
集団の分類は、古代学研究会による集落動態の研究を参考とする。総括された集落構造の変化を参考とすると、竪穴建物の同時併存数が３～８棟程度が単位集団と分類され、10棟前後の場合は基礎集団に分類される傾向があり、500ｍ範囲内の居住域の数が２つ以上ある場合は複合型集落、１つの場合は基礎集団に分類される傾向がある（古代学研究会編2016）。また、竪穴建物の同時併存数は２棟以上であり、先行研究の単位として最も小さな単位である単位集団は少なくとも２棟以上の大きさを持つと考えられる。
本論文では、単位集団を２～８棟、基礎集団が９棟以上と定義する。１棟あたりの住居人数が５人程度と仮定し、単位集団は11人～40人、基礎集団は41人以上とする。４セル×４セル（約500ｍ四方）内に２つ以上の基礎集団が存在する場合、それらの集団は基礎集団が合わさった複合型集落と定義する。集団の分類を表🔢に示す。
近畿地方の大規模弥生集落遺跡では唐古・鍵遺跡が著名であり、弥生時代中～後期にまたがって、かつ居住域全域がすべて環濠に囲まれているとされる径500mを超える弥生集落は唐古・鍵遺跡以外報告されていない（若林2019）。そのため、複合型集落の大きさの上限値として参考とする。ムラを囲む1番内側の環濠は18万㎡の居住区を囲み、安藤広道による人口推定では竪穴建物数が判明している環濠集落（横浜市大塚遺跡２万㎡で100人）との面積比較から唐古・鍵遺跡の人口は約900人となる（唐古・鍵考古学ミュージアム編2020）。

２万㎡の範囲で100人が居住可能とされるため、本論文の１セルあたりでは約78.6人が居住できるとされる。そのため、１セルの上限人数を80人と定義する。また、径100～200ｍほどの集団単位である基礎集団は１～３セルを使用し、１つの基礎集団の最大人数は３セル×80人で240人とする。シミュレーション開始時の弥生時代末期（西暦200年）では４セル×４セル（約500ｍ四方）の最大収容人数を900人とする。

集団の移動に関してはサブモデルの集団移動ルールで定義する。

## 3.4. 地域
移動と婚姻を制御する。
範囲は512セル四方（約64km四方）とする。

---

# 4. ステップと実行順序

初期化とstep実行の２段階。
集団移動→渡来→婚姻→妊娠・出産→死亡→毎step終了処理の順番で実行される（仮）。
エージェントの実行順序は配列の先頭から探索する。

## 4.1. シミュレーション期間
シミュレーションは西暦200年1月から西暦724年12月まで行う。この期間に設定した理由は、鬼頭宏が算出（鬼頭1996・2000）した西暦200年の人口数を入力値とし、同じく鬼頭宏が算出した西暦725年の人口数を本論文の人口推定の出力値と比較するためである。

## 4.2. １ステップの定義
１ステップを１ヶ月（グレゴリオ暦）とする。ただし、１ヶ月はどの月も等しく30.436875日（365.2425÷12）とし各月の日数に差はないものとする。「集団移動」、「婚姻」、「出産」のサブモデルがある。１ステップ内で、この順番でそれぞれのサブモデルが実行される。また、エージェントの実行順序は、生成順である。

## 4.3. 実行順序
「集団移動」、「婚姻」、「出産」、「渡来」のサブモデルがある。１ステップ内で、集団移動、婚姻、出産、渡来の順番でそれぞれのサブモデルが実行される。エージェントの変数（月齢）の更新は毎ステップの終了時に行う。エージェントの変数の更新後にエージェントの判定（死亡判定）を行う。また、エージェントの各個体の実行順序は毎ステップ、ランダムである。

## 4.4. 毎ステップ終了時の人間の属性変数の更新
全ての人間の月齢を１増やす。

## 4.5. 毎ステップ終了時の人間の判定
全ての人間の月齢と余命を比較し、もし月齢が余命以上の場合はそのエージェントを削除する（すなわち亡くなる）。死亡したエージェントの配偶者は無配偶者となり、次のステップから再婚可能となる。また、死亡したエージェントが所属していた集団の人数を更新する。

---

# 5. デザインコンセプト
ＯＤＤプロトコルには11個のコンセプトがある。本シミュレーションではそのうち🔢個が該当した。各コンセプトについての説明を表🔢に示す。本論文のシミュレーションモデルは多少複雑ではあるが、本章のモデルの記述とデザインコンセプトを用いてモデルの再現性を確保する。

## 5.1. 基本原則 Basic principles
人口増加は「マルサスの人口増加モデル」に準ずる

## 5.2.創発 Emergence
人々の移動により、水田稲作民、ゲノムの割合が変化する。

集団移動による合流と分離により新たな集団が形成

## 5.3.適応 Adaptation

## 5.4.目標 Objectives

## 5.5.学習 Learning
(未実装)狩猟採集民が同じ集団に所属する水田稲作民から農耕技術を受け取る。

## 5.6.予測 Prediction
本論文のエージェントは予測をしない

## 5.7.感知 Sensing
- 集団移動時に土地の傾斜度を知覚し、住みやすい土地へ移動しやすい特性を持つ。
- 女性は婚姻時に近隣地域にいる男性を知覚する。

- 【集団移動】集団が他の集団と合流
- 【集団移動】集団が他の集団のいない場所へ移動
- 【集団移動】集団が湖川からの距離や傾斜、標高を認知
- 【婚姻】婚姻時に周囲にいる配偶者候補を探索

## 5.8.相互作用 Interaction
在地人と渡来人の混合

## 5.9.偶然性 Stochasticity

- エージェント（人間及び集団）の実行順序
- 【初期配置／出産】初期配置時及び出生時の人間の性別
- 【初期配置／出産】初期配置時及び出生時の人間の余命
- 【集団移動】集団のランダムな方向への移動
- 【婚姻】婚姻関係の成立
- 【婚姻】婚姻時の人間の移動
- 【出産】出産の発生及び出産数

## 5.10.集団 Collectives
集団ごとの人口動態を可視化

## 5.11.観察 Observation
- 地域（地方や令制国など）別人口増減
- 地域別集団分類ごとの人数及び各集団の総数
- 地域別渡来系比率
- 既存の大規模集落遺跡の立地に集団が居住しているか、かつ居住する集団の人口数
- シミュレーション終了時の地域別人口数と先行研究による奈良時代初期の地域別人口数との比較

---

# 6. 初期設定
シミュレーションは前1101年1月16日に開始する。最初の100年間(1200step)は正しい家族関係を構築する時間とし、前1001年1月から計測開始とする。
年齢は0歳から寿命から15歳引いた年齢でランダムに決定する。ただし、寿命が15歳以下の場合は0歳から始まるとする。
開始時は誰も婚姻関係がないものとする。

シミュレーションは西暦50年まで行う。Koyama(1978)と鬼頭(1996)は、弥生時代の人口推定西を1900BP(50AD)と連帯させたため、シミュレーション結果の人口と比較できるように設定した。

## 6.1. 集団の配置
Koyama(1978)の人口推定方法を参考に、縄文時代晩期(J5)における47都道府県の人口を推定した。シミュレーションで使用される令制国に、人口は比例配分された。集団には男女の数が等しくなるように所属させる。集団の初期人数は偶数とする。

---

# 7. 入力データ

---

# 8. サブモデル

## 8.1. 集団移動ルール
### 8.1.1. 先行研究
集団は移動する。しかし、移動する集団の規模や移動距離は定かではない。移動距離の定義は、金井東裏遺跡人骨の歯を用いたストロンチウム同位体比分析による出身地の推定を参考とした（群馬県埋蔵文化財調査事業団編2019）。この分析では、渡来系1号人骨と在地系3号人骨が同じ地域で3～15歳頃まで生活しており大人になってから群馬県へ来たことが判明した。地質データと分析データを照合すると1号人骨と3号人骨は長野県西部から愛知県を経由して岡山県周辺まで広がる地域の地盤の分析値であり群馬県より西の地域で生まれ育った可能性がある最短距離である長野県の松本市周辺で生まれ金井東裏遺跡へ移動したと考えると少なくとも約130kmの移動が想定される。

また、渡来人を表現したと考えられる筒袖の人物埴輪が千葉県の山倉1号墳から出土していることや日本書紀には「百済人の男女2000人以上を東国に移住した」と記述される。

本研究では毎月1％の確率で800セルから8000セルの位置をランダムに移動すると定義する。移動先は重み付きランダム選択で決定する。傾斜4.0度以下が重み9、傾斜8.9度以下が重み10、傾斜17.6度以下が重み４とする。集団は出産時を除き原則80人まで居住できる。集団が80人以上になった場合は半分の人数を近隣集落へ移動させる。

坂平文博のABMでは１ステップ＝１年でランダムに１セル（空間は北部九州を模した50セル×50セルであるから、１セルは約２～３ｋｍ）移動する（坂平2018）。本論文のABMの単位へ変換すると12ステップで最大16～24セル移動していることになる。
シミュレーション開始時に設定した最大移動量と移動確率を元に移動の処理を記述する。移動確率から集団が移動するか判定する。移動する判定が出た時、その集団の座標から最大移動量までの範囲内にある可住地をリスト化する。可住地一つ一つに重みを付加し、その可住地重みリストを入力とし重み付き乱択アルゴリズムを用いて移動先の座標を決定する。

### 8.1.2. 移動時の傾斜度

## 8.2. 渡来ルール
毎step、筑前国に5人渡来人が移住する。
（この項目で）縄文系と渡来系の変数をそれぞれ比較する。

## 8.3. 婚姻ルール
### 8.3.1. 先行研究
婚姻については鷲尾祐子の研究を参考とする。女性は13歳以上から婚姻可能とする。60歳以上は再婚しない（鷲尾2018）。また、古文書によると女性の80％以上が20代までに、90％以上が30代までに結婚し、95％の女性は生涯に一度以上は結婚する（鷲尾2018）ため確率調整の参考とする。一方の男性の結婚可能年齢は定かではないが17歳以上とする。男女ともに配偶者が亡くなった場合は再婚可能とする。
婚姻時、女性は周囲60セルの男性をランダムに選ぶ。選ばれた男性は女性のセルへ移動する。今回は一夫一妻制のみとする。

### 8.3.2. 婚姻可能年齢ルール
女性は13歳以上60歳未満の間で婚姻可能とし、男性は17歳以上で婚姻可能とする。この値は鷲尾祐子による３世紀の東アジアにおける婚姻研究を参考とした。女性は13歳から結婚の適齢が始まり60歳以上は再婚しない（鷲尾2018）。一方の男性の結婚可能年齢は定かではないが適齢の始まりが十代後半とされているため中間を取って17歳とする。

### 8.3.3. 婚姻確率
女性の初婚月齢ルール
孫呉嘉禾四年～六年（235～237年）における臨湘侯國の婚姻実態では女性の９割が20代までに、女性の約97％が30代までに結婚し、ほとんどの女性は生涯に一度以上は結婚する（鷲尾2018）というデータを元に初婚モデルを作成する。本論文では生涯未婚率を２％とする。初婚モデルは対数正規分布とし、1899年の結婚年齢の分布（速水1986）の近似曲線から分散及びxの基準値を決定する。本論文の婚姻モデル（女性の月齢ごとの初婚の重み）を数式🔢に示す。ageは年齢、σ^2は分散である。1899年の結婚年齢の分布の近似曲線からσ^2=0.25の分散の値が得られた。また、13歳が婚姻開始年齢であり、xの基準を０にするために年齢から13を引いている。
このモデルでは20代までの婚姻率が89.96％、30代までの婚姻率が97.13％となり、３世紀の東アジアにおける婚姻率とほぼ一致している。このモデルから、月齢ごとの初婚の重みを算出する。算出した初婚の重みを図🔢に示す。全ての月齢の重みを合計すると100％となる。
女性の初婚月齢は初期配置時または出生時に決定する。初婚月齢は、全ての月齢における初婚の重みを入力とし重み付き乱択アルゴリズムを用いて初婚の月齢を決定する。女性の月齢が初婚の月齢以上となり、かつ未婚の場合に「配偶者の選択ルール」が発動する。

### 8.3.4. 配偶者の選択ルール
婚姻の対象となった女性は周囲60マスに存在する、配偶者のいない男性からランダムに１人を選び、その男性と婚姻する。坂平文博のABMでは男性の周囲３セル以内（空間は北部九州を模した50セル×50セルであるから、３セルは約６～９ｋｍ）の女性をランダムに選び出す（坂平2018）。本論文のABMの単位へ変換すると周囲最大48～72セル以内の女性が婚姻対象となる。そのため、平均値の60マス以内を採用した。

### 8.3.5. 婚姻時の移動ルール
夫婦は同居し、同じ集団へ属する。
妻方居住婚、選択居住婚、夫方居住婚を定義する。妻方居住婚では、婚姻時に夫が妻の所属する集団へ移動する。夫方居住婚では、婚姻時に妻が夫の所属する集団へ移動する。選択居住婚は一定の確率で夫が妻の所属する集団へ移動するか、妻が夫の所属する集団へ移動するか決定する。

妻方居住婚では母系遺伝のmtDNAが拡散しやすい、一方で夫方居住婚では拡散しにくい。

## 8.4. 出産ルール

### 8.4.1. 妊娠
15歳以上、50歳未満の女性は妊娠可能とする。縄文時代の合計特殊出生率は８程度であるため、出産モデルの参考データとする。

婚姻率は対数正規分布とする。

### 8.4.2. 出産
妊娠の10step（10ヶ月）後に出産。
合計特殊出生率が8になるような式で確率分布。

婚姻している女性は合計特殊出生率が8になる確率分布で出産判定する。出産判定された女性は10ヶ月後（10ステップ後）に人間（赤ちゃん）が生成される。赤ちゃんは母親と同じセルに居住する。配偶者と死別している場合は出産判定されない。子供は母親となった女性エージェントと同じ座標位置に作成される。子供は50％の確率で男女に振り分けられ、年齢0歳0ヶ月と余命を持つ。

子供は、母親となった女性エージェントと同じ座標位置に作成される。子供は50％の確率で男女に振り分けられ、月齢０歳０ヶ月と余命を持つ。また、セル内が上限人数（80人）以上だった場合は特例として上限人数を超えてセル内の集団の一員となる。
余命は初めに性別ごとの重みにより未成人で亡くなるか成人で亡くなるか決定する（表🔢）。次に性別ごとかつ、未成人または成人で死亡ごとの重みによりどの年齢種別で亡くなるか決定する（表🔢）。最後に月齢種別ごとに月齢下限（０歳０ヶ月）から月齢上限の範囲で乱数を振り、余命を確定させる。

### 8.4.3. 流産・死産
一定の確率で出産判定時に死産か判定される。
死産の場合は子供は生成されない。

### 8.4.4. 遺伝と継承（2で解説？）

### 8.4.5. 余命（2で解説？）

## 8.5. 毎step終了処理

---

# 9. シミュレーションケースと評価

## 9.1. 比較対象
- 1. 西暦50年の人口推定と比較。
- 2. 人間エージェントの核ゲノムを模した縄文由来ゲノムを算出し、古代人骨の縄文系由来ゲノム率と比較。
- 3. 集団の持つmtDNAの割合と古代人骨のmtDNAの割合を比較。
- 4. 集団の持つY-DNAの割合と古代人骨のY-DNAの割合を比較。ただし、Y-DNAはサンプル数が少ないため参考程度とする。
- 5. 稲作の伝播と比較。
 
西暦50年の予想人口・ゲノム・農耕文化を「A-Model」と名称する。
- 人口：60万人
    - Koyama(1978)の1900BP(50AD)の人口推定を表す。
- ゲノム：70% 渡来系
    - 弥生・古墳時代の渡来由来mtDNAの比率に基づいて仮想した。
- 農耕文化：南九州から北東北まで
    - 水田稲作の分布圏を表す(藤尾2013)。

## 9.2. 定数
- 開始人口・西暦50年終了人口（考古学・人口学）
- 寿命（形質人類学・民族学）
- mtDNA初期配置（分子人類学）

## 9.3. 変数
- 渡来開始年
    - 前951年（1801ステップ目）から始まる。
    - 前801年（3601ステップ目）から始まる。
- 月間渡来人数
    - 1人（毎年12人）
    - 5人（毎年60人）
    - 10人（毎年120人）
- 婚姻居住形態 (妻方居住婚率)
    - 妻方 (1.0)
    - 選択 (0.5)
    - 夫方 (0.0)
- 婚姻最大移動距離
    - <10km (80セル)
    - <20km (160セル)
    - <40km (320セル)
- 月間集団移動
    - 0.14% (約60年に1回; 1/720)
    - 0.21% (約40年に1回; 1/480)
    - 0.42% (約20年に1回; 1/240)
- 集団移動距離
    - <100km (800セル)
    - <200km (1600セル)
    - <400km (3200セル)
- 農耕文化継承 (親の生業が異なる場合、農耕文化を継承する確率)
    - 25% (0.25)
    - 50% (0.5)
    - 75% (0.75)

## 9.5 モデル定義

全ての変数の組み合わせを実験することは困難であるため、ベースモデル(B-Model)を定義し、変数をひとつずつ変えて実験を行う。それらの実験をモデルと名称する(C-Model, D-Model, 等)。実験したモデルは以下のとおり：

<img width="500" alt="変数" src="https://github.com/AsPJT/ABM-Papers/blob/1bb384157ffb8b3e5e312fcc55e9dd7aff5f614b/Images/sim2024.10/sim2024.10-models-white.png">

## 9.6 実験結果

## B-Model (Base Model)
### 人口
<img width="500" alt="B-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/B-model-pop.png">

### 農耕文化
<img width="500" alt="B-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/B-model-culture.png">

### mtDNA
<img width="500" alt="B-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/B-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="B-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/B-model-snp.png">

## C-Model (渡来開始年 = 800 BC)
### 人口
<img width="500" alt="C-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/C-model-pop.png">

### 農耕文化
<img width="500" alt="C-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/C-model-culture.png">

### mtDNA
<img width="500" alt="C-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/C-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="C-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/C-model-snp.png">

## D-Model (月間渡来人数 = 1人)
### 人口
<img width="500" alt="D-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-pop.png">

### 農耕文化
<img width="500" alt="D-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-culture.png">

### mtDNA
<img width="500" alt="D-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="D-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-snp.png">

## E-Model (月間渡来人数 = 10人)
### 人口
<img width="500" alt="D-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-pop.png">

### 農耕文化
<img width="500" alt="D-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-culture.png">

### mtDNA
<img width="500" alt="D-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="D-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/D-model-snp.png">

## F-Model (婚姻居住形態 = 妻方)
### 人口
<img width="500" alt="F-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/F-model-pop.png">

### 農耕文化
<img width="500" alt="F-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/F-model-culture.png">

### mtDNA
<img width="500" alt="F-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/F-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="F-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/F-model-snp.png">

## G-Model (婚姻居住形態 = 夫方)
### 人口
<img width="500" alt="G-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/G-model-pop.png">

### 農耕文化
<img width="500" alt="G-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/G-model-culture.png">

### mtDNA
<img width="500" alt="G-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/G-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="G-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/G-model-snp.png">

## H-Model (婚姻可能距離 =<10km)
### 人口
<img width="500" alt="H-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/H-model-pop.png">

### 農耕文化
<img width="500" alt="H-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/H-model-culture.png">

### mtDNA
<img width="500" alt="H-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/H-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="H-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/H-model-snp.png">

## I-Model (婚姻可能距離 =<40km)
### 人口
<img width="500" alt="I-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/I-model-pop.png">

### 農耕文化
<img width="500" alt="I-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/I-model-culture.png">

### mtDNA
<img width="500" alt="I-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/I-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="I-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/I-model-snp.png">

## J-Model (月間集団移動 = 0.14%)
### 人口
<img width="500" alt="J-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/J-model-pop.png">

### 農耕文化
<img width="500" alt="J-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/J-model-culture.png">

### mtDNA
<img width="500" alt="J-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/J-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="J-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/J-model-snp.png">

## K-Model (月間集団移動 = 0.42%)
### 人口
<img width="500" alt="K-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/K-model-pop.png">

### 農耕文化
<img width="500" alt="K-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/K-model-culture.png">

### mtDNA
<img width="500" alt="K-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/K-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="K-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/K-model-snp.png">

## L-Model (集団移動距離 =<100km)
### 人口
<img width="500" alt="L-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/L-model-pop.png">

### 農耕文化
<img width="500" alt="L-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/L-model-culture.png">

### mtDNA
<img width="500" alt="L-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/L-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="L-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/L-model-snp.png">

## M-Model (集団移動距離 =<400km)
### 人口
<img width="500" alt="M-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/M-model-pop.png">

### 農耕文化
<img width="500" alt="M-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/M-model-culture.png">

### mtDNA
<img width="500" alt="M-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/M-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="M-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/M-model-snp.png">

## N-Model (農耕文化継承 = 25%)
### 人口
<img width="500" alt="N-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/N-model-pop.png">

### 農耕文化
<img width="500" alt="N-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/N-model-culture.png">

### mtDNA
<img width="500" alt="N-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/N-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="N-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/N-model-snp.png">


## O-Model (農耕文化継承 = 75%)
### 人口
<img width="500" alt="O-Model 人口" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/O-model-pop.png">

### 農耕文化
<img width="500" alt="O-Model 文化" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/O-model-culture.png">

### mtDNA
<img width="500" alt="O-Model mtdna" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/O-model-mtdna.png">

### 渡来人由来変異（ゲノム）
<img width="500" alt="O-Model ゲノム" src="https://github.com/stephenwest470/PAXS-papers/blob/d9cfdf1c227b624881acd0029f8036a18b5a2677/2024-ASN/Images/O-model-snp.png">

---

# 10. 考察
 考古学データによる予想を表すA-Modelに一致するモデルはなかったが、各モデルの結果から、各設定の人口動態にもたらす影響が明らかになった。

## 渡来開始年
弥生時代の開始年代についての論争が続く限り、渡来開始年は不明である。藤尾慎一郎・国立歴史民俗博物館のAMS炭素年代測定の編年によると、弥生時代早期 (山ノ寺・夜臼Ⅰ期) は2900BP (950 BC)から始まる (藤尾 2009).　しかし、これに対して反論があり、...

## 月間渡来人数
埴原和郎の「超大量渡来説」(Hanihara 1987)では、紀元前300年から紀元700年...

## 婚姻居住形態


## 婚姻可能距離


## 月間集団移動


## 集団移動距離


## 農耕文化継承


---

# 参考文献

## 計算機科学
- 坂平 文博・寺野 隆雄 2014 「弥生農耕文化の「主体」は誰だったか？ ―人類学・考古学へのエージェントベースシミュレーションの適用」『コンピュータ ソフトウェア】31巻3号


## 考古学
- 藤尾慎一郎 2009「研究の経緯と成果・課題」『国立歴史民俗博物館研究報告』第149集, 1–30
- 藤尾慎一郎 2013「弥生文化の輪郭 : 灌漑式水田稲作は弥生文化の指標なのか」『国立歴史民俗博物館研究報告』第178集, 85–120

## 民族学
- Volk, A. A., & Atkinson, J. A. (2013). Infant and child death in the human environment of evolutionary adaptation. Evolution and Human Behavior, 34(3), 182–192. https://doi.org/10.1016/j.evolhumbehav.2012.11.007


## 古人骨
- 古人骨. (n.d.). 九州大学総合研究博物館 データベース; 九州大学. http://db.museum.kyushu-u.ac.jp/anthropology/
- 

## ゲノム研究
