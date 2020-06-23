# MarkdownをPDFへ

Markdownで作成した作業マニュアルをPDF化したい


## Premise

- AtomにてMarkdownで記述
- 展開したPDFは紙に印刷して利用されることも考える必要がある

## Result

- styles.lessファイルにcss記述することで、任意のデザインに変更できる

## Details

- 特記事項なし<br>
 [AtomエディタのMarkdown PreviewのCSSを実務書類的に調整する](https://blog.s0014.com/posts/2015-10-16-mk/)に倣って解決した。

今回、マニュアル用に設定したstyle.less

 ```css
 .markdown-preview {
   @c_border: #666; // border-color

   max-width: 900px;
   margin: 0 auto;
   padding: 25px;
   color: black;
   hr {
     margin: 50px 0;
     background-color: transparent;
     &:after{
       content: "";
       display: block;
       border-top-style: ridge;
     }
     &.pb {
       // <hr class="pb">を入れる事で、
       // プリント時の改ページを指定することができる。
       page-break-after: always;
       &:after {
         display: none;
       }
     }
   }
   h1, h2, h3, h4, h5 {
     font-weight: normal;
     border-color: @c_border;
   }

   h1 {
     font-size: 35px;
     border: none;
     margin: 30px auto;
     text-align: right;
     //text-shadow: 5px,5px,2px,gray;
     letter-spacing: 5px

     position: relative;
     padding: 0.25em 0;
   }
   h1:after {
     content: "";
     display: block;
     height: 4px;
     background: -webkit-linear-gradient(to right, rgb(255, 186, 115), #ffb2b2);
     background: linear-gradient(to right, rgb(255, 186, 115), #ffb2b2);
   }

   h2 {
     background: linear-gradient(transparent 70%, rgb(255, 186, 115) 70%);
 /*
     border-bottom: solid 3px #cce4ff;
     position: relative;
   }
   h2:after {
     position: absolute;
     content: " ";
     display: block;
     border-bottom: solid 3px #ffba73;
     bottom: -3px;
     width: 30%;
     */
   }

   h3 {
     font-size: 18px;
     font-weight: bold;
     margin-bottom: 10px;

     background:  #fffaf4; //c2edff;/*背景色*/
     padding: 0.2em;/*文字まわり（上下左右）の余白*/
   }
   // 僕の用途ではh4以降は基本的に必要ないので、
   // 設定していません。

   // 見出し以外のタグを字下げする
   // ぱっと思いつく、よく使うタグを指定
   p, table, ul, ol, dl, pre, blockquote {
     margin-left: 25px;
     ul, ol, dl {
       margin-left: 0px;
     }
   }
   table {
     border-collapse: collapse;
     border-spacing: 0;
     max-width: 800px;
     th {
       text-align: center;
       background-color: #eee;
       border-color: @c_border;
     }
     tr {
       border-top: #666;
     }
     td {
       border-color: @c_border;
     }
   }

   footer{
       /*footerの装飾*/
       width: 100%;
       background-color: #fff;
       color: #000;
       text-align: center;
       padding: 10px 0;

       postion: absolute; /*絶対位置*/
       bottom: 0;
   }

   .margin-clear {
     margin-left: 0;
   }
   // テキストの中央揃え
   .center {
     text-align: center;
     &:extend(.margin-clear);
   }
   // テキストの右寄せ
   .right {
     text-align: right;
     &:extend(.margin-clear);
   }
 ```
 
> [CSSおしゃれな見出しのデザイン例](https://saruwakakun.com/html-css/reference/h-design)

## Trable

- PDF化したい場合以外はGitHubPreviewの初期状態ままで作業したいため、普段はコメントアウトすることで対応している。若干の不便さは残る。
- マニュアルなどの作成時には表紙を要求されることもあるため、表紙のレイアウトも考えたい。


## Reference

- [AtomエディタのMarkdown PreviewのCSSを実務書類的に調整する](https://blog.s0014.com/posts/2015-10-16-mk/)
