# Manage the version of the specification

> 仕様書のversionに併せた分岐を用意する

## Premise
他のアプリとの通信仕様を含む仕様書の場合、  
仕様の不一致による通信Errorが発生する。  


## Result
仕様書のversionごとに併せた処理を行う様、  
flagで分岐、またこれらを一括管理するmethodを作成することで、  
仕様巻き戻しによる再作成等を回避できる。  

## Details

(wrote:2018/12/25)

ただし、仕様書ごとに足並みを併せたscheduleでReleaseを行う運用が  
正しく確立されているprojectにおいて、これは不要の可能性が高い。

**※以下、要検討**

~~~ csharp
private enum version
{
  1-0-0,
  1-1-0,
  1-2-0,
}

private void version(version verNo)
{
  switch(verNo)
  {
    case version.1-2-0:
      StaticObject.isVer120=true;
      break;

    case version.1-1-0:
      StaticObject.isVer110=true;
      break;

    case version.1-0-0:
      StaticObject.isVer100=true;
      break;
  }

  //自分のフラグより古いversionのflagはTrueになる様に連鎖させる
  if (StaticObject.isVer120)
  {
    StaticObject.isVer110 = true;
  }
  if (StaticObject.isVer110)
  {
    StaticObject.isVer100 = true;
  }
}
~~~

if文の部分はSwitch文に内包すると、branchが発生しても対応することができる。  

ただし、分岐しない場合は上記のif文の方が人為的なミスで途中のversionのflag変更がこぼれる可能性が少ないような気もする。

（Update:2018/12/28）
- 自分より新しいversionのflagがFalseになる様な連鎖が必要
- ここで記載するversionは「仕様書の」versionであることをわかる様な名称にする必要がある

（Update:2019/01/07）  
Sample
~~~ csharp
private void btnOk_Click(object sender, EventArgs e)
{
    var radioButtonChecked = gbUapp.Controls.OfType<RadioButton>().SingleOrDefault(rb => rb.Checked == true);
    if(radioButtonChecked == null)
    {
        _logBox.WarningWrite("mode:選択されていません。");
        return;
    }
    switch(radioButtonChecked.Name)
    {
        case "rb103E":
            _logBox.NormalWrite("mode:仕様103E");
            StaticObject.is104E = false;
            StaticObject.is105E = false;
            break;
        case "rb104E":
            _logBox.NormalWrite("mode:仕様104E");
            StaticObject.is104E = true;
            StaticObject.is105E = false;
            break;
        case "rb105E":
            _logBox.NormalWrite("mode:最新");
            StaticObject.is104E = true;
            StaticObject.is105E = true;
            break;
    }
}
~~~



## Trable

## Reference
