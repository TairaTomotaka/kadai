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
    
    * 本学習する場合
        * 060: best_ind: 最良個体??
            * ga(): trainer.py: ga を実行する.
        <br>

        * 065: 最良個体の構造のログを取る. 
        <br>
    
    * 本学習しない場合. 

* toolbox.register(): 引数の初期値がない関数の初期値を指定できる. 