---
marp: true
---
# PowerShell の勉強会資料01


---
# PowerShell とは?

- PowerShell は Windows で使用できるシェルのこと
- 最近は Windows だけでなく Linux, macOS でも動かすことができる
- コマンドプロンプトや Bash などとは違いテキストだけでなく .NET のオブジェクトを扱うことができる
- PowerShell は現在 5.1 と 7.x 系統があり、 5.1 は Windows に同梱されている PowerShell で 7.x は .NET Core 3.1 以降に構築されたマルチプラットフォーム対応されたもの ([リンク](https://docs.microsoft.com/ja-jp/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.2))

---
# コマンド

- PowerShell のコマンドは cmdlets (コマンドレット) と呼ばれる
- コマンドレットはネイティブな PowerShell コマンドで、Linux のコマンドのようにスタンドアロンの実行ファイルではない
- コマンドレットは .NET や PowerShell のスクリプト言語で作ることができる
- `Get-Command` を実行するとコマンド一覧を取得できる

```powershell
function Invoke-Sample{
    <#
        .SYNOPSIS
        サンプルコマンドレット
    #>
    Write-Host "Hello,World!"
}
```

---
# エイリアス

- PowerShell には UNIX や cmd.exe のユーザーが使いやすいようによく使うコマンドレットに別名が付けられている

## 例
```powershell
cd   # Set-Location
pwd  # Get-Location
echo # Write-Output
```

- `Get-Alias` コマンドを使うことでエイリアスの一覧表示やエイリアスを調べることができる

```powershell
> Get-Alias cd

CommandType     Name                  Version    Source
-----------     ----                  -------    ------
Alias           cd -> Set-Location
```

---
# よく使うコマンド (Get-Content)

ファイルの内容を読み込むときに使用する

```powershell
> Get-Content .\test.json
[
    {
        "a": "test",
        "b": 1
    },
    {
        "a": "data",
        "b": 2
    }
]
```

---
# よく使うコマンド (Select-String)

Linux の grep のように正規表現で一致する行を取得する

```powershell
> Get-Content .\test.json | Select-String test

    "a": "test",
```

---
# よく使うコマンド (ConvertFrom-Json)

JSON を解析して PowerShell で扱うオブジェクトに変換する

```powershell
> Get-Content .\test.json | ConvertFrom-Json

a    b
-    -
test 1
data 2
```

---
# よく使うコマンド (Where-Object)

grep のオブジェクト版のような機能
C# の LINQ の Where や JavaScript の filter のような働き
`?` という記号がエイリアスに設定されている

```powershell
> $(Get-Content .\test.json | ConvertFrom-Json) | Where-Object -Property a -Match test
a    b
-    -
test 1

```

---
# よく使うコマンド (ForEach-Object)

データストリームに対する射影機能
C# の LINQ の Select や JavaScript の map のような動き
`%` という記号のエイリアスが設定されている

```powershell
> $(Get-Content .\test.json | ConvertFrom-Json) | ForEach-Object { $_.b * 10}
10
20

```

---
# よく使うコマンド (Get-NetIPAddress)

ipconfig や ifconfig のようにネットワークインターフェースの情報を取得する

```powershell
> Get-NetIPAddress

IPAddress       : 192.168.1.1
 ~~~
```
