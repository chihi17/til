# CodeBlockでc#を構文強調で記述

C#はシンタックスハイライトで記述可能か？

## Result
Atom上はGitHubStyleでPreviewして、特に問題なく表示できているように見える。

\~~~ csharp から記述

~~~ csharp  
private void CodeSample()  
{  
  int a = 1;  
}  
>~~~

## Details


> C#:
  type: programming
  ace_mode: csharp
  codemirror_mode: clike
  codemirror_mime_type: text/x-csharp
  tm_scope: source.cs
  color: "#178600"
  aliases:
  - csharp
  extensions:
  - ".cs"
  - ".cake"
  - ".cshtml"
  - ".csx"
  language_id: 42

引用：[GitHub/linguist](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml#L506)  
参考：[Developers.IO:[GitHub] コードブロックでシンタックスハイライト可能な言語一覧](https://dev.classmethod.jp/tool/git/syntax-highlight-language-list/)

## Trable

1.
\~ で囲うとPackage:[markdown-pdf](https://atom.io/packages/markdown-pdf)でPDF化したときに表示がおかしくなる。  
Incident：\~~test~~ で打消し線になる ~~test~~

  ~~~ csharp
  private void CodeSample()
  {
    int a = 1;
  }
  ~~~

2.
\` で囲うとPDF化したときにブロック内の改行が解除される

  ``` csharp
  private void CodeSample()
  {
    int a = 1;
  }
  ```
