# UnityTemplate

新規Unityゲームを立ち上げるときに使用するテンプレート。

## Why?

Unityゲーム開発でよく使われるライブラリ、ミドルウェア、設定などをこちらのリポジトリで管理することで、  
新規プロジェクト作成時の手間を省くことができます。

## Default Settings

- Project Settings
  - Package Manager
    - OpenUPM
      - Scope(s)
        - com.google
        - com.github-glitchenzo
  - Player
    - Scripting Backend: IL2CPP
    - API Compatibility Level: .NET Framework
- TMP Essential Resources インポート済み

## Include

- Unity Companion License (Unity製ライブラリ)
  - Addressables (com.unity.addressables)
  - Scriptable Build Pipeline (com.unity.scriptablebuildpipeline)
  - Burst (com.unity.burst)
  - Post Processing (com.unity.postprocessing)

※他にもプロジェクト作成時にデフォルト追加されたパッケージ、あるいは依存として追加されたパッケージもあります。詳細はPackage Managerにて確認ください

- MIT licence
  - [GlitchEnzo/NuGetForUnity](https://github.com/GlitchEnzo/NuGetForUnity)
    - Install via OpenUPM
  - [Cysharp/UniTask](https://github.com/Cysharp/UniTask)
    - Install via Git
  - [Cysharp/R3](https://github.com/Cysharp/R3)
    - Install via Git & NuGetForUnity
  - [Cysharp/MasterMemory](https://github.com/Cysharp/MasterMemory)
    - Install via NuGetForUnity
  - [Cysharp/ZString](https://github.com/Cysharp/ZString)
    - Install via Git & NuGetForUnity(`System.Runtime.CompilerServices.Unsafe/6.0.0`に依存)
  - [Cysharp/ZLinq](https://github.com/Cysharp/ZLinq)
    - Install via Git & NuGetForUnity
    - ZStringと同じく`System.Runtime.CompilerServices.Unsafe`に依存しますが、依存するバージョンは`6.1.2`ですので、`6.1.2`にアプデ
  - [Yarn Spinner](https://github.com/YarnSpinnerTool/YarnSpinner-Unity)
    - Install via Git
  - [annulusgames/LitMotion](https://github.com/annulusgames/LitMotion)
    - Install via Git

## Removed

私の独断で不要と思ったため、以下のパッケージを削除しました。

- Version Control (com.unity.collab-proxy)
- Visual Scripting (com.unity.visualscripting)

## Get Started

### MasterMemory

MasterMemoryのドキュメントによりますと、Unityではデフォルトinitキーワード使えないので、もし必要なら手動で設定してください。  
MasterMemoryで生成されるコードの名前空間を指定することも可能です。  
参考URL: <https://github.com/Cysharp/MasterMemory?tab=readme-ov-file#getting-startedunity>

```csharp
// Optional: Unity can't load default namespace to Source Generator
// If not specified, 'MasterMemory' will be used by default,
// but you can use this attribute if you want to specify a different namespace.
[assembly: MasterMemoryGeneratorOptions(Namespace = "MyProj")]
// Optional: If you want to use init keyword, copy-and-paste this.
namespace System.Runtime.CompilerServices
{
    internal sealed class IsExternalInit { }
}
```

## R3

> The biggest difference is that in normal Rx, when an exception occurs in the pipeline, it flows to `OnError` and the subscription is unsubscribed, but in R3, it flows to `OnErrorResume` and the subscription is not unsubscribed.
> I consider the automatic unsubscription by OnError to be a bad design for event handling. It's very difficult and risky to resolve it within an operator like Retry, and it also led to poor performance (there are many questions and complex answers about stopping and resubscribing all over the world). Also, converting OnErrorResume to OnError(OnCompleted(Result.Failure)) is easy and does not degrade performance, but the reverse is impossible. Therefore, the design was changed to not stop by default and give users the choice to stop.

R3では、Observer側のパイプラインで例外発生したときに、パイプラインは止まらず、処理を継続します。Subscriptionも自動的に解除されません。
Subscribe時に`OnComplete`で例外をハンドルするか、あるいは`ObservableSystem.RegisterUnhandledExceptionHandler`を実装してください。
