# space_weather_prediction（宇宙天気予報）
このページは宇宙天気予報の初歩的な処理を説明しています。
太陽風についてのデータは[NOAA（アメリカ海洋大気庁）のオープンデータ](https://www.swpc.noaa.gov/products/real-time-solar-wind)
ページを利用しています。
## 目次
1.始めに  
2.必要な環境  
3.データの入手方法  
4.機械学習の方法  
5.さいごに  

## 1.始めに
近年、我々の生活はGPSや衛星写真などますます宇宙空間からの情報を活用していますが、その生活が安定的に営まれるためにも宇宙天気予報による太陽風による磁気嵐への対策が急がれます。現在日本ではNICT（国立研究開発法人　情報通信研究機構）、アメリカではNOAAが主に宇宙天気予報を推し進めており、いまだ物理によって解明されていないため予測しにくい太陽活動を機械学習などを用いて予測しています。NOAAのオープンデータ（上を参照）は最大7日間のみのデータしか提供していないので、本来ならば機械学習を用いるデータ量ではないのですが、今回は実験的に行いたいと思います。  

## 2.必要な環境
- pythonを実行できる環境（私は[Anaconda](https://www.anaconda.com/download/)を使っています）
- TensorFlow
- Keras　
  
以上が今回のプログラムコードを実行するために必要な環境です。  
  
Anacondaとは何かということやAnacondaをインストールする方法はまとまったページがあるので是非すぐ下のリンクご参照ください。  
リンク：[『ディストリビューション「Anaconda」とは？』](http://programming-study.com/trouble/anaconda/)  
TensorFlowやKerasについてもインストールをする手順を説明したページがありましたのでご参照ください。  
リンク：[『中年の親父が人工知能ブームの影響を受けてこっそり勉強しているブログ』](http://coldsnap.hatenablog.jp/entry/2017/08/27/114900)  
  
## 3.データの入手
上記の[NOAAのページ](https://www.swpc.noaa.gov/products/real-time-solar-wind)を開きます。    
![image](https://user-images.githubusercontent.com/39754583/46477890-ad809e00-c826-11e8-84b3-55c8e96fe32b.png)    
下の方にスクロールしていくと    
![image](https://user-images.githubusercontent.com/39754583/46477915-bec9aa80-c826-11e8-93db-5003b702f745.png)　　  
上の画像のようになっていると思いますので、「7 days」を選んで「save as text」を押します。そうすると一週間分の太陽風のデータが「rtsq_plot_data」というファイル名で入手できます。  
実際に私が使ったデータは「rtsq_plot_data1」（2018-09-25 12:00:00.000~2018-10-02 02:47:00.000のデータ）というファイル名で上のinputの中に入れておきます。

## 4.機械学習の方法
機械学習の方法（コードの内容）は上のファイルの「kernel」を開き、「spaceweather_*** 」というファイルをクリックしてください。もし２節で述べた環境が整っている場合はコードをコピペして自分の環境で実行してみるといいかもしれないです！なお、「*** 」には"temp"や"speed"の表記がありますがこれは温度や速さのことです。  
ファイルを開くと各コードの近くに説明を載せています。  
今回機械学習している部分を抜粋したものを下に載せました。

```python:spaceweather_dens.py

model = Sequential()
model.add( LSTM( hidden_neurons, batch_input_shape = 
    ( None, length_of_sequence, in_out_neurons ), return_sequences = False ) )
model.add( Dense( in_out_neurons ) )
model.add( Activation( "linear" ) )
optimizer = Adam( lr = 0.001 )
model.compile( loss = "mean_squared_error", optimizer = optimizer )

model.fit(input, expected,
    batch_size = 500,
    epochs = 80,
    validation_split = 0.1
)

```
7行目まででどのように学習するかを設定して、model.fitでその設定に基づいて学習しています。詳しくは「keras 機械学習」などでググってもらったらたくさん解説がのっています。

## 5.さいごに
今回使用した機械学習部分以外にも様々なモデルがあるのでぜひ調べてみて、コードを書き替えてみて、より正確なモデルを作ってみてください。

