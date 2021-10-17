# jconfig_tmct
Ported Intel(ALTERA) JRunner to Raspberry Pi.

This is a port of the JTAG configuration tool "JRunner" published by Intel (ALTERA) to the Raspberry Pi.

Intel(ALTERA)が公開しているJTAGコンフィグレーションツール「JRunner」をRaspberry Piに移植したものです。  
※日本語の解説文は英文解説の後にあります。

---

## What is JRunner?
JRunner is a tool to connect parallel port of PC and JTAG terminal of Intel(ALTERA) FPAG for configuration, but it is almost worthless now because of the following reasons.

- **It is impossible to get a new PC which has parallel port and works properly.**  
USB-to-parallel converter is also not compatible and useless.
- **Supported OS up to Windows XP.**

...However, it has a lot of value as a technical document because it analyzes the cdf file and performs JTAG configuration, so I ported it to Raspberry Pi as "JCONFIG".

The I/O drive part uses the standard Raspberian GPIO driver, but by rewriting this part to just drive I/O, it can be ported easily to general microcontrollers.


## Port Assignment
The port assignment is as shown below.

Since this assignment is defined in jb_io.c, it can be rewritten here to assign it to a different port.

| Pin No. | Pin Name | Input/Output Direction | Signal Name | Remarks |
| :-- | :-- | :-- | :-- | :-- |
| 29 | GPIO5 | IN | JTAG_TDO | TDO input ...connect to TDO of target FPGA |
| 31 | GPIO6 | OUT | JTAG_TDI | TDI output, connected to TDI of target FPGA |
| 32 | GPIO12 | OUT | JTAG_TMS | TMS output: Connect to TMS of target FPGA. |
| 33 | GPIO13 | OUT | JTAG_TCK | TCK output: Connect to TCK of target FPGA. |
| 36 | GPIO16 | OUT | JTAG_INUSE | 'H' output only during configuration |


## How to install
All you need to do is to put the program file itself into an appropriate folder. No special settings are required, but if GPIO resources are being used for other tasks, they cannot be used.

The GPIO control uses the standard Raspberian device driver, so nothing else is needed.


## How to use
This is a command line only tool, no GUI.

./mt_jconfig [cdf_filename]

Specify a configuration definition file (*.cdf) in [cdf_filename].

The configuration definition file defines the JTAG chain and the target file to be written, and is easy to create using QuartusII Programmer.

It is easy to create a configuration definition file using the QuartusII Programmer, which has the following restrictions on the target file

- **The file path and file name must be in lower case to be read properly.**  
If the path and file name contain uppercase letters, reading will fail.
- **The format must be an uncompressed rbf file.**  
If you want Quartus to generate the file, you need to uncheck "Generate compressed bit stream".


## License
The license terms conform to the terms of JRunner. You are free to use it for commercial or private use as long as you do not remove the copyright notice, but you do so at your own risk.

You can find more information on the following page. (*Japanese only)

[tmct web-site: JCONFIG](https://ss1.xrea.com/tmct.s1009.xrea.com/doc/ta-ja-7e5g01.html)

---
## JRunnerとは？
JRunnerはパソコンのパラレルポートとIntel(ALTERA)のFPAGのJTAG端子を接続してコンフィグレーションを行うためのツールですが、下記の理由があり今となってはほぼ利用価値がないツールです。

- **パラレルポートが付いていて正常稼働するパソコンは新品での入手は不可。**  
USB-パラレル変換もソフト的な互換がなく使えない。
- **対応OSがWindows XPまで。**

…とはいえcdfファイルを解析してJTAGコンフィグレーションを行っているなど技術資料としての価値は高いので、試しにRaspberry Piに移植してみたものを「JCONFIG」として公開します。

I/O駆動部分はRaspberianのGPIO標準ドライバを利用していますが、この部分をただのI/O駆動に書き換えることで一般的なマイコンなどにも容易にポーティングが可能です。


## ポートアサイン
以下のようなポートアサインになっています。

このアサインは jb_io.c に定義しているので、ここを書き換えると別のポートにアサインすることもできます。

| Pin No. | ピン名 | 入出力方向 | 信号名 | 備考 |
| :-- | :-- | :-- | :-- | :-- |
| 29 | GPIO5  | IN  | JTAG_TDO   | TDO入力‥ターゲットFPGAのTDOに接続 |
| 31 | GPIO6  | OUT | JTAG_TDI   | TDI出力‥ターゲットFPGAのTDIに接続 |
| 32 | GPIO12 | OUT | JTAG_TMS   | TMS出力‥ターゲットFPGAのTMSに接続 |
| 33 | GPIO13 | OUT | JTAG_TCK   | TCK出力‥ターゲットFPGAのTCKに接続 |
| 36 | GPIO16 | OUT | JTAG_INUSE | コンフィグレーション中のみ 'H' が出力される |


## インストール方法
適当なフォルダにプログラムファイル本体を入れるだけです。とくに設定などは不要ですが、他のタスクにGPIOリソースを使われている場合は使用できません。

GPIO制御はRaspberian標準のデバイスドライバを使用しており、ほかには何も必要ありません。


## 使い方
コマンドライン専用ツールです。GUIはありません。

./mt_jconfig [cdf_filename]

[cdf_filename] にコンフィグレーション定義ファイル(*.cdf)を指定します。

コンフィグレーション定義ファイルはJTAGチェインと書き込み対象ファイルを定義したもので、QuartusII Programmerを使って作成するのが楽です。

なお、このツールには書き込み対象ファイルに下記の制約があります。

- **ファイルパス・ファイル名とも小文字でないと正常に読み込めない**  
パス・ファイル名に大文字が含まれる場合は読み込みに失敗します。
- **フォーマットは無圧縮のrbfファイルに限る**  
Quartusでファイルを生成させる際は「Generate compressed bit stream」のチェックを外しておく必要があります。


## ライセンス
ライセンス条項はJRunnerの条項に準拠します。著作権表示を消さない限り商用・私用問わず自由に利用して頂いて構いませんが、すべて自己責任にてご利用ください。

下記のページにもう少し詳しく書いてあります。

[tmct web-site: JCONFIG](https://ss1.xrea.com/tmct.s1009.xrea.com/doc/ta-ja-7e5g01.html)

