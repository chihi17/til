# 2024未解決作業

- [byte配列とuintと16進数の取り扱い](#byte配列とuintと16進数の取り扱い)
- [commitとpushの違い](#commitとpushの違い)
- [markdownlintの対象ファイルを絞ろうとしたらnvmとNode.jsとnpmを入れる話になった](/report/20241216.md) ※別ファイル

## byte配列とuintと16進数の取り扱い

<div style="text-align: right;">2024.11.28</div>

目的：byte配列に入れたものを16進数のデータで取り扱うためにuintに入れたい。たぶん。

```csharp
byte[].Length //配列byte[]の長さ、配列に含まれる要素の数

//uintを参照したい場合のメソッド
ToUInt32(byte[] value, int startIndex)
```

```csharp
byte[] data = { 0x87, 0xd6, 0x12, 0x00 };
int number = BitConverter.ToInt32(data, 0);  //number=1234567
```

> 「1234567」という数値は16進数で表すと「0x0012D687」となりますが、バイト配列には下位のバイトから格納されていることが分かります。このように下位バイトから格納していく方法はリトルエンディアンと呼ばれ、逆に上位バイトから格納していく方法はビッグエンディアンと呼ばれます。

> Windowでは通常リトルエンディアンが使われますが、バイト配列にはビッグエンディアンで格納されているケースもあるでしょう。そのような場合はSystem.ArrayクラスのReverseメソッドで順番を逆にしてから変換しましょう。

```csharp
byte[] data = { 0x00, 0x12, 0xd6, 0x87 };
Array.Reverse(data);
int number = BitConverter.ToInt32(data, 0);  //number=1234567
```

要するにひっくり返せるらしい。  
今回の目的とはあんまり関係なさそう。

```csharp
// Example of the BitConverter.ToUInt32 method.
using System;

class BytesToUInt32Demo
{
    const string formatter = "{0,5}{1,17}{2,15}";

    // Convert four byte array elements to a uint and display it.
    public static void BAToUInt32( byte[ ] bytes, int index )
    {
        uint value = BitConverter.ToUInt32( bytes, index );

        Console.WriteLine( formatter, index,
            BitConverter.ToString( bytes, index, 4 ), value );
    }

    public static void Main( )
    {
        byte[ ] byteArray = {
             15,   0,   0,   0,   0,  16,   0, 255,   3,   0,
              0, 202, 154,  59, 255, 255, 255, 255, 127 };

        Console.WriteLine(
            "This example of the BitConverter.ToUInt32( byte[ ], " +
            "int ) \nmethod generates the following output. It " +
            "converts elements \nof a byte array to uint values.\n" );
        Console.WriteLine( "initial byte array" );
        Console.WriteLine( "------------------" );
        Console.WriteLine( BitConverter.ToString( byteArray ) );
        Console.WriteLine( );
        Console.WriteLine( formatter, "index", "array elements",
            "uint" );
        Console.WriteLine( formatter, "-----", "--------------",
            "----" );

        // Convert byte array elements to uint values.
        BAToUInt32( byteArray, 1 );
        BAToUInt32( byteArray, 0 );
        BAToUInt32( byteArray, 7 );
        BAToUInt32( byteArray, 3 );
        BAToUInt32( byteArray, 10 );
        BAToUInt32( byteArray, 15 );
        BAToUInt32( byteArray, 14 );
    }
}

/*
This example of the BitConverter.ToUInt32( byte[ ], int )
method generates the following output. It converts elements
of a byte array to uint values.

initial byte array
------------------
0F-00-00-00-00-10-00-FF-03-00-00-CA-9A-3B-FF-FF-FF-FF-7F

index   array elements           uint
-----   --------------           ----
    1      00-00-00-00              0
    0      0F-00-00-00             15
    7      FF-03-00-00           1023
    3      00-00-10-00        1048576
   10      00-CA-9A-3B     1000000000
   15      FF-FF-FF-7F     2147483647
   14      FF-FF-FF-FF     4294967295
*/
```

（formatterの機能は表示の文字数の話だ…  
使わなすぎて忘れていたけど本でみた覚えアリ）

わかりやすく抜粋

```csharp
byte[ ] byteArray = {
	15,   0,   0,   0,   0,  16,   0, 255,   3,   0,
	0, 202, 154,  59, 255, 255, 255, 255, 127 };

Console.WriteLine( BitConverter.ToString( byteArray ) );
//0F-00-00-00-00-10-00-FF-03-00-00-CA-9A-3B-FF-FF-FF-FF-7F
```

16進数表示するのにToStringなんだ…？

```csharp
public static void BAToUInt32( byte[ ] bytes, int index )
{
	uint value = BitConverter.ToUInt32( bytes, index );

	Console.WriteLine( formatter, index,
		BitConverter.ToString( bytes, index, 4 ), value );
}

BAToUInt32( byteArray, 1 );
BAToUInt32( byteArray, 0 );
BAToUInt32( byteArray, 7 );

/*
index   array elements           uint
-----   --------------           ----
    1      00-00-00-00              0
    0      0F-00-00-00             15
    7      FF-03-00-00           1023
*/
```

index1,0,7の中身は0,15,255から4個分（array elementsの結果参照）で、  
10進数の255は16進数のFF、2進数の1111 1111

uintなんで1023なのかは明日考える…

<div style="text-align: right;">2024.11.29</div>

16進数 03FF = 10進数 1023 …

|Data Type	|Binary	    |Value |Min                 |Max	               |
|-----------|-----------|------|--------------------|----------------------|
|uint8	    |11111111	|255   |0                   |2<sup>8</sup> -1      |
|int8	    |11111111	|-1    |-2 <Sup>(8-1)</sup> |2 <sup>(8-1)</sup> -1 |

|Data Type	|bit  |byte	|Min            |Max           |
|-----------|-----|-----|---------------|--------------|
|uint32	    |32	  |4	|0	            |4,294,967,295 |
|int32	    |32   |4	|-2,147,483,648	|2,147,483,647 |

なぜ 0x00000000 がUInt32の判定はいるの？

> 0x44434241(4byte) = 0b01000100_01000011_01000010_01000001(32bit)

4byte 32bit だから…？

参考に一読した記事：  

- [C#でバイト配列から数値や文字列に変換する](https://araramistudio.jimdofree.com/2023/08/01/c-%E3%81%A7%E3%83%90%E3%82%A4%E3%83%88%E9%85%8D%E5%88%97%E3%81%8B%E3%82%89%E6%95%B0%E5%80%A4%E3%82%84%E6%96%87%E5%AD%97%E5%88%97%E3%81%AB%E5%A4%89%E6%8F%9B%E3%81%99%E3%82%8B/)
- [BitConverter.ToUInt32 メソッド](https://learn.microsoft.com/ja-jp/dotnet/api/system.bitconverter.touint32?view=net-9.0)
- [Integer（整数）について](https://zenn.dev/posita33/books/ue5_starter_cpp_and_bp_001/viewer/chap_a0112_decimal_and_integer)
- [数値のデータ型を明示的に指定するには？](https://atmarkit.itmedia.co.jp/ait/articles/0405/07/news065.html)
- [アドレスとメモリの内容の理解メモ](https://qiita.com/ky0he1_sec/items/04dbf83158b34b92a77b) ※記事内容が他人のメモすぎる

<div style="text-align: right;">2024.12.04</div>

## commitとpushの違い

問題：２回commitした後、まとめてpushしたら、VSCodeのGitHistoryを見た時に、２つ前のcommitにorigin/mainのマークが残ってしまった。

手順：

1. update report20241202 commit
2. fix violation of markdownlint commit
3. push

VSCodeのGitHistoryの状態：

> fix violation of markdownlint [main]  
update report20241202  
update report20241129 [origin/main]

現象：さらに1commit1pushすると最新にmain,origin/mainの両マークが移動した。

手順：

1. update report20241202 commit
2. fix violation of markdownlint commit
3. push
4. update report20241203

VSCodeのGitHistoryの状態：

> update report20241203 [main] [origin/main]  
fix violation of markdownlint  
update report20241202  
update report20241129

現象：  
GitHubのActivityには"update report20241202"は表示されず、"fix violation of markdownlint"の詳細に  
"username pushed __2 commits__ to main"と表示される。

目的：commitとpushの違いを理解する必要がある

参考に一読した記事：  

- [Gitのコミットとプッシュの違いをわかりやすく解説](https://trends.codecamp.jp/blogs/media/difference-word148)
