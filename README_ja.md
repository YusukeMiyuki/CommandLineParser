# CommandLineParser

自作のコマンドラインパーサー。
属性でコマンドラインオプションやヘルプメッセージなど指定設定できる。
引数チェック機能もほんのちょびっと入っている。

## 属性の種類

* CommandLineTargetClass

    クラスに対して指定する。コマンドラインオプションの設定対象クラスであることを表す。

* CommandLineAttr

    フィールドやプロパティに対して設定する。  
    引数で「オプション名」、「必須かどうか」、「オプションのヘルプメッセージ」を指定できる。

## 実装済みの機能
実装済みの機能は以下のとおり。

* 必須オプションが存在するか
* ヘルプオプションが指定されているか
* エラーが発生しているか

なお、エラーが発生しているかは「必須オプションが存在するかどうか」のみで決まる。

## 使い方
sample code は以下のとおり。

```cs
[CommandLineTargetClass]
class Program
{
    [CommandLineAttr("i", true, "help: input file")]
    public static string InputFile { get; set; }

    [CommandLineAttr("o", false, "help: output file")]
    public static string OutputFile { get; set; } = @"C:\temp\outputFile.txt";

    static void Main()
    {
        var parser = new CommandLineParser(typeof(Program));

        if (parser.IsHelp)
        {
            Console.WriteLine(parser.HelpMessage);
            return;
        }

        if (parser.IsError)
        {
            Console.WriteLine(parser.ErrorMessage);
            return;
        }

        // main process start ...
    }
}
```

## ヘルプメッセージのフォーマット


ツール名 バージョン 会社名 著作権  
ツールの説明

usage: ツール名 必須オプション { param } ... [ options ... ]

オプション1\t: 設定したヘルプメッセージ  
オプション2\t: 設定したヘルプメッセージ  
...  

Display this message.  
-h  
-help

## 制限

* オプション指定時の prefix は「-」固定
* 必須オプションのチェックしかしてくれない
* 指定されたオプション引数の値のチェックは何もしない
* オプションの重複指定はできない
* オプション名を空文字にすることはできない
* 「int」「double」「string」「bool」しかフィールドやプロパティに値を格納できない
* ヘルプメッセージのコマンドラインオプションは「オプション名の長さ順」で固定
