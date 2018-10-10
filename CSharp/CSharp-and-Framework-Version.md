# C#の言語バージョンと.NetFramework

>using static hogehoge

を用いていたことにより先輩のPCではビルドが通らなかった。

## Premise
自分の環境
- Microsoft Visual Studio Express 2015 for Windows Desktop
- .NET Framework 4.5

先輩の環境
- Microsoft Visual Studio (
  Express？)
 2012 for Windows Desktop
- .NET Framework 4.5

## Result
C#の新機能の多くは古い.NET Framework上でも動くが、一部機能では動かす術を持たない。

'using static'の記述はC# 6に該当する。  
C# 6 = Visual Studio 2015, .NET Framework 4.6, Windows10 と同時期の Release.  

'using static' は .NET Framework 2.0で動作可能。  
（= .NET Framework 1.0/1.1はサポートが切れているため、2.0で動くものは実質どこでも動く。）

そのため、ビルドしたアプリケーションはおそらく動くが、  
Visual Studio 2012 で開発している先輩のPCではビルドすることができないと考えられる。

## Details

## Trable

## Reference
[++C++; // 未確認飛行 C「C#の言語バージョンと.NET Frameworkバージョン」](https://ufcpp.net/study/csharp/cheatsheet/listfxlangversion/)
