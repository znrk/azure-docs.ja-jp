---
title: "Azure AD .NET の概要 | Microsoft Docs"
description: "サインインし、OAuth を使用して Azure AD で保護されている API を呼び出すために Azure AD と連携する .NET Windows デスクトップ アプリケーションを構築する方法を説明します。"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: 1865043ca9c9019b9813f11eb4a55f7f16d79287
ms.openlocfilehash: 145e60d078b3b4c1d554d2613c5fcaced004d303


---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Windows デスクトップ WPF アプリの Azure AD への統合
[!INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

デスクトップ アプリケーションを開発する場合、Azure AD を使用すると、Active Directory アカウントを使用してユーザーの認証処理を容易に行うことができます。  また、Office 365 API や Azure API などの Azure AD によって保護された任意の Web API をアプリケーションで安全に使用することもできます。

保護されたリソースにアクセスする必要のある .NET ネイティブ クライアントに対しては、Azure AD により、Active Directory 認証ライブラリ (ADAL) が提供されます。  ADAL の唯一の目的は、アプリからアクセス トークンを容易に取得できるようにすることです。  それがどれほど簡単であるかを示すために、ここで、次のような、.NET WPF To-Do List アプリケーションを構築します。

* [OAuth 2.0 認証プロトコル](https://msdn.microsoft.com/library/azure/dn645545.aspx)を使用して Azure AD Graph API を呼び出すためのアクセス トークンを取得します。
* 指定されたエイリアスを持つユーザーをディレクトリで検索します。
* ユーザーのサインアウト処理を行います。

完全に動作するアプリケーションを構築するには、次の手順を行う必要があります。

1. Azure AD にアプリケーションを登録する
2. ADAL をインストールおよび構成する
3. ADAL を使用して、Azure AD からトークンを取得する

最初に、[アプリのスケルトンをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip)するか、[完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)します。  また、ユーザーを作成し、アプリケーションを登録することを可能にするための Azure AD テナントも必要です。  テナントを所有していない場合は、「 [How to get an Azure Active Directory tenant (Azure Active Directory テナントの取得方法)](active-directory-howto-tenant.md)」を参照して取得してください。

## <a name="1-register-the-directorysearcher-application"></a>*1.DirectorySearcher アプリケーションを登録する*
アプリでトークンを取得できるようにするには、まず、アプリを Azure AD テナントに登録し、Azure AD Graph API にアクセスするためのアクセス許可を付与する必要があります。

1. [Azure ポータル](https://portal.azure.com)にサインインします。
2. 右上にある自分のアカウントをクリックして、**[ディレクトリ]** 一覧の下で、管理者アクセス許可を持つ Active Directory テナントを選択します。
3. 左側のナビゲーションで **[Azure Active Directory]** を選択します。
4. **[アプリの登録]** をクリックして、**[追加]** を選択します。
5. 画面の指示に従い、新しい **ネイティブ クライアント アプリケーション**を作成します。 アプリケーションの **[名前]** には、エンド ユーザーがアプリケーションの機能を把握できるような名前を設定します。 **[リダイレクト URI]** には、Azure AD がトークン応答を返すために使用するスキームと文字列の組み合わせを設定します。  アプリケーション固有の値 (たとえば、 `http://DirectorySearcher`) を入力します。 **[作成]** をクリックして、アプリケーションを作成します。
6. 引き続き Azure Portal でアプリケーションを選択して、**[設定]** をクリックして **[プロパティ]** を選択します。
7. アプリケーション ID の値を探してクリップボードにコピーします。
8. アプリケーションのアクセス許可を構成します。[設定] メニューで [必要なアクセス許可] セクションを選択して、**[追加]**、**[API を選択します]** の順にクリックし、[Microsoft Azure の Active Directory] (これは AADGraph API です) を選択します。 **[アクセス許可の選択]** をクリックし、[ディレクトリ データの読み取り] を選択します。 これにより、アプリケーションが Graph API を使用してユーザーをクエリできるようになります。

## <a name="2-install--configure-adal"></a>*2.ADAL をインストールおよび構成する*
アプリケーションを Azure AD に登録したので、ADAL をインストールし、ID 関連のコードを記述できます。  ADAL が Azure AD と通信できるようにするには、アプリの登録に関するいくつかの情報を ADAL に提供する必要があります。

* 最初に、パッケージ マネージャー コンソールを使用して、ADAL を DirectorySearcher プロジェクトに追加します。

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* DirectorySearcher プロジェクトで、 `app.config`を開きます。  Azure Portal で入力した値が反映されるように、 `<appSettings>` セクションの要素の値を置き換えます。  これらの値は、コードで ADAL を使用する際に常に参照されます。
  * `ida:Tenant` は、Azure AD テナントのドメイン (たとえば、contoso.onmicrosoft.com) です。
  * `ida:ClientId` は、ポータルからコピーしたアプリケーションのクライアント ID である必要があります。
  * `ida:RedirectUri` は、ポータルに登録したリダイレクト URL です。

## <a name="3----use-adal-to-get-tokens-from-aad"></a>*3.  ADAL を使用して AAD からトークンを取得する*
ADAL を使用することの基本的なメリットは、アプリがアクセス トークンを必要とする場合、アプリは `authContext.AcquireTokenAsync(...)` を呼び出すだけで、残りの処理は ADAL が実行してくれることです。  

* `DirectorySearcher` プロジェクトで、`MainWindow.xaml.cs` を開き、`MainWindow()` メソッドを見つけます。  最初の手順は、アプリの `AuthenticationContext` (ADAL のプライマリ クラス) を初期化することです。  ここでは、ADAL が Azure AD と通信し、トークンをキャッシュする方法を通知するために必要な調整項目を ADAL に渡します。

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* 次に、`Search(...)` メソッドを見つけます。このメソッドは、ユーザーがアプリの UI で [検索] ボタンをクリックすると呼び出されます。  このメソッドは、指定された検索用語で UPN が始まるユーザーをクエリするための、Azure AD Graph API に対する GET 要求を実行します。  ただし、Graph API をクエリするためには、要求の `Authorization` ヘッダーに access_token を含める必要があります。この処理を ADAL が実行します。

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* アプリが `AcquireTokenAsync(...)`を呼び出すことによってトークンを要求すると、ADAL はユーザーに資格情報を要求することなく、トークンを返そうとします。  ADAL は、トークンを取得するにはユーザーのサインインが必要であると判断した場合、ログイン ダイアログを表示し、ユーザーの資格情報を収集し、認証が成功するとトークンを返します。  また、何らかの理由によりトークンを返せない場合、 `AdalException`をスローします。
* `AuthenticationResult` オブジェクトには、アプリが必要とする可能性のある情報を収集するために使用される `UserInfo` オブジェクトが含まれていることに注意してください。  DirectorySearcher で、 `UserInfo` は、ユーザーの ID を使用してアプリの UI をカスタマイズするために使用されます。
* ユーザーが [サインアウト] ボタンをクリックした場合、次の `AcquireTokenAsync(...)` への呼び出しでは、ユーザーにサインインするように要求する必要があります。  ADAL を使用すると、この操作は、トークン キャッシュをクリアするのと同じぐらい容易に達成できます。

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* ただし、ユーザーが [サインアウト] ボタンをクリックしていない場合、ユーザーが次に DirectorySearcher を実行するときに備えて、ユーザーのセッションを維持する必要があります。  アプリが起動したら、既存のトークン用の ADAL のトークン キャッシュをチェックし、必要に応じて UI を更新できます。  `CheckForCachedToken()` メソッドで `AcquireTokenAsync(...)` に対して別の呼び出しを行います。その際に `PromptBehavior.Never` パラメーターを渡します。  `PromptBehavior.Never` は ADAL に対して、トークンを返せない場合、ユーザーにサインイン用の認証情報の入力を求めるのではなく、代わりに ADAL は例外をスローする必要があることを通知します。

```C#
public async void CheckForCachedToken()
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

お疲れさまでした。 これで、ユーザー認証を処理でき、OAuth 2.0 を使用して Web API を安全に呼び出すことができ、ユーザーについての基本情報を取得できる、動作する . WPF アプリケーションが完成しました。  テナントに一連のユーザーを設定します (設定していない場合)。  DirectorySearcher アプリを実行し、それらのユーザーの一人としてサインインします。  UPN に基づいて、他のユーザーを検索します。  アプリを閉じて、再び実行します。  ユーザーのセッションがそのままに維持されていることに注意します。  サインアウトし、別のユーザーとしてサインインします。

ADAL を使用することにより、これらの共通 ID 機能のすべてを容易にアプリケーションに組み込むことができます。  キャッシュ管理、OAuth プロトコル サポート、ログイン UI を使用してのユーザーの提示、有効期限切れとなったトークンの更新など、面倒な操作を容易に実装できます。  習得する必要があるのは、単一の API 呼び出し、 `authContext.AcquireTokenAsync(...)`のみです。

完全なサンプル (構成値を除く) を取得するには、 [ここ](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)をクリックしてください。  ここからは、さらなるシナリオに進むことができます。  次のチュートリアルを試してみてください。

[Protect a Web API using Bearer tokens from Azure AD](active-directory-devquickstarts-webapi-dotnet.md) (Azure AD からのベアラー トークンを使用することによる Web API の保護)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]



<!--HONumber=Dec16_HO5-->


