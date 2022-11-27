# trainer.py 概要

## trainer.py

* コンストラクタ __init__(self, exp):
    * 035: self.exp: Exp クラスのインスタンス.
        * Exp クラス: exp.py: 
        実験条件を管理するクラス. 
    <br>

    * 039 040: 訓練データとテストデータを選択している. 
        * select_dataset(exp, is_train): utils.py: 
        第二引数で訓練データか否かを指定する.
    <br>

    * 042: self.modeler: Modeler クラスのインスタンス. 
        * Modeler クラス: Modeler.py: 
        遺伝子表現から実際の CNN を構築する. 
    <br>

    * 044: self.analizer: Analizer クラスのインスタンス.
        * Analizer クラス: analizer.py: 
        実験結果を可視化するクラス. 引数で結果を格納するファイル名を指定する. 
    <br>

    * 045: self.logger: ログを出力する関数. [ログとは.](https://qiita.com/FukuharaYohei/items/92795107032c8c0bfd19)
        * get_logger: utils.py: ログ内容を指定したファイルに書き込む. 
    <br>

    * 046: self.best_predict: 最も良い CNN の識別率. (best test accuracy)
    <br>

* run(self): 実験を回す関数.
    * 055: 各種パラメータ値のログを取る.
        * show_exp(): utils.py: 各種パラメータ値のログを取る.
    <br>

    * 058: is_resume: すでに得られた個体で本学習するか否か. 
    <br>
    
    * すでに得られた個体で本学習しない場合. (普通こっち)
        * 059 ~ 062: GA による探索. 
        * 060: best_ind: 最良個体
            * ga(): trainer.py: ga を実行する.
        <br>

        * 065: 最良個体の構造のログを取る. 
        <br>

    * すでに得られた個体で本学習する場合. 
        * only_predict ??
        <br>

    * Fully train (本学習??)
        * 083: 訓練する.
        * 084: fittness 評価.
        * 089: model で実際に予測する.
        * 091: 実験結果を可視化.
        * 092: model を保存.
    <br>


    * 関数に self をつけるのは, Trainer クラスのメソッドだから...
    <br> 

* ga(self): gaを実行する関数. 
    * Returns: best_ind: 探索で得られた最良個体
    <br>
    * 105 106: [DEAP についての記事](https://darden.hatenablog.com/entry/2017/04/18/225459#creatorcreate%E9%96%A2%E6%95%B0) (creator.create()について記載あり)
    <br>
    * 105: base.Fitness クラスを継承して, weights=(1.0,)というメンバ変数を追加した適応度を表す FitnessMax というクラスを creator モジュール内に作成している. weights=(1.0,)は, 評価する目的関数が1つの場合で, 個体の適応度を最大化したい場合にこう書く. ( 最小化したい場合 → weights=(-1.0,) と書く. )
    <br>
    * 106: list クラスを継承して, fitness=creator.FitnessMax というメンバ変数を追加した Individual クラスを作成している. つまり, 遺伝子を保存する個体 list に, 適応度 fitness というメンバ変数を追加して Individual としている. 
    <br>

    * base.Toolbox.register(): 引数の初期値がない関数の初期値を指定できる. 
        * 例
        ```
        def test(a,b,c):
            print a,b,c
        
        toolbox = base.Toolbox()
        toolbox.register("test_wda", test, 1,2,3)

        toolbox.test_wda()
        ```
        出力
        ```
        1,2,3
        ```
    <br>

    * 109: ```create_gene``` に ```self.exp``` を渡して遺伝子を初期化する関数 ```create``` を作成.
        * ```create_gene```: gene.py: 遺伝子初期化戦略に基づき遺伝子を初期化する関数. 
    <br>

    * 110: 109 で作成した, ```Toolbox.create``` を ```1``` 回実行して, その値を ```creator.Individual``` に格納して返す関数 ```individual``` を作成している. 
        * ```tools.initRpeat```: 3個の引数 container, func, n を持っており, n 回 func を実行して, その値をcontainerに格納して返す関数である. 
        ```tools.initRpeat``` の内容. 
        ```
        def initRepeat(container, func, n):
            return container(func() for _ in xrange(n))
        ```
    <br>

    * 111: 個体を ```toolbox.individual``` で作成して ```list``` に格納し, 世代を生成する ```population``` 関数を作っている. ( 第 3 引数 ```n``` は, 141 で指定. )
    <br>

    * 113: eval(individual): ある個体の遺伝子から, 実際にモデルを構築して, その適応度(予測精度) を返す. 
    <br>

    * 124 125 126: 関数の読み替え ??
<br>

* train(self, net, dataset, is_ga): モデルを学習する関数.
    * net: 学習するモデル.
    * dataset: 使用するデータセット.
    * is_ga: 探索フェーズか否かを示すフラグ.
    * Returns: 予測精度 = correct / total * 100