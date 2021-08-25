---
layout: post
title: HTML Canvasメモ
tags:
- programing
- html
- canvas
- javascript
- memorandum
---

HMTL Canvasを活用してドット絵を描画するための覚え書き。  

<!--more-->

HTML上に`<canvas id="canvas_area"></canvas>`というcanvasのエリアがある前提で記述する。  

## データ定義について

画像のデータは`0-9`の数字と`a-z`のアルファベットを用いた文字列で表記する。  
例えば3x3の正方形の中央にドットを打つ場合は、  

```javascript
const data =
  '000' +
  '010' + 
  '000';
```

とする。  

一文字一文字を色に対応させるためにパレットの連想配列を用意する。  
今回は対応する文字に対して、RGBの値を格納した配列を持たせている。  

```javascript
const colorData = {
  '0': [0, 0, 0],
  '1': [255, 255, 255]
};
```
この場合は3x3の黒の四角形の中心に白の点が描画されることになる。  


## 描画プログラムについて

今回は下記のプログラムにデータを渡し、描画を行う。

```javascript
'use strict';

const data =
  '0000000000' +
  '0011111100' +
  '0011111100' + 
  '0011111100' +
  '0000000000';

const colorData = {
  '0': [0, 0, 0],
  '1': [255, 255, 255]
};

const canvas = document.getElementById('canvas_area');
const context = canvas.getContext('2d');

const imageData = context.createImageData(10, 5);
const dataArray = data.split("");

dataArray.forEach( (color, index) => {
  imageData.data[index * 4 + 0] = colorData[color][0];
  imageData.data[index * 4 + 1] = colorData[color][1];
  imageData.data[index * 4 + 2] = colorData[color][2];
  imageData.data[index * 4 + 3] = 255;
});

context.putImageData(imageData, 20, 20);
```

実行してみると`<canvas id="canvas_area"></canvas>`に塗りが赤、線が黒の四角が描かれる。  

では各命令や処理を細かく見ていく。

## 詳解

```javascript
const context = canvas.getContext('2d');
```

取得したキャンバスエリア`canvas`から`CanvasRenderingContext2D`オブジェクトを取得する。  
キャンバスエリアに2D描画をするためのメソッドやプロパティなどを持つ。  

- [CanvasRenderingContext2D - MDN](https://developer.mozilla.org/ja/docs/Web/API/CanvasRenderingContext2D)


```javascript
const imageData = context.createImageData(10, 5);
```

2D画像を描画するための`ImageData`オブジェクトを作成。  
引数には幅と高さを渡す。  

- [Canvas とピクセル操作 - MDN](https://developer.mozilla.org/ja/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas)


```javascript
imageData.data[index * 4 + 0] = colorData[color][0];
imageData.data[index * 4 + 1] = colorData[color][1];
imageData.data[index * 4 + 2] = colorData[color][2];
imageData.data[index * 4 + 3] = 255;
```

`ImageData`の各ピクセルのRGBAの値を`0-255`で設定する。  
並びとしては4つの配列の連なりでR(赤),G(緑),B(青),A(不透明度)となっている。  
なので10x5の画像の場合は10x5x4で200の配列で情報を管理していることになる。  

今回は`画像情報のindex値 * 4(色数) + n`で配列の添字を算出している。  


- [ImageData.data - MDN(en-US)](https://developer.mozilla.org/en-US/docs/Web/API/ImageData/data)


## 参考

- [HTML5のCanvasにドット絵を描く](https://qiita.com/Yamazin/items/cdca50471af475e6300a)
