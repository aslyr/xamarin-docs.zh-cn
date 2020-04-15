---
title: Xamarin.Android 片段演练 - 第 1 部分
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 043ad02f9ca9148910364ac82917551ee58d72ba
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027407"
---
# <a name="fragments-walkthrough-ndash-phone"></a>片段演练 &ndash; 电话

本文是创建定目标到纵向 Android 设备的 Xamarin.Android 应用的演练的第一部分。 本演练将介绍如何在 Xamarin.Android 中创建片段，以及如何将片段添加到示例中。

[![](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

将为此应用创建以下类：

1. `PlayQuoteFragment` &nbsp; 此片段会显示威廉·莎士比亚戏剧中的引文。 它会由 `PlayQuoteActivity` 托管。
1. `Shakespeare` &nbsp; 此类将两个硬编码数组保留为属性。
1. `TitlesFragment` &nbsp; 此片段会显示威廉·莎士比亚所写戏剧的标题列表。 它会由 `MainActivity` 托管。
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 会启动 `PlayQuoteActivity`，以响应用户在 `TitlesFragment` 中选择戏剧。

## <a name="1-create-the-android-project"></a>1.创建 Android 项目

新建名为 FragmentSample  的 Xamarin.Android 项目。
# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![新建 Xamarin.Android 项目](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![新建 Xamarin.Android 项目](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

建议为本演练选中“新式开发”  。

创建项目后，将文件 layout/main.axml  重命名为 layout/activity_main.axml  。

-----

## <a name="2-add-the-data"></a>2.添加数据

此应用程序的数据将存储在两个硬编码字符串数组中，这两个数组是类名 `Shakespeare` 的属性：

* `Shakespeare.Titles` &nbsp; 此数组保留威廉·莎士比亚戏剧的列表。 这是 `TitlesFragment` 的数据源。
* `Shakespeare.Dialogue` &nbsp; 此数组保留 `Shakespeare.Titles` 中的一部戏剧的引文列表。 这是 `PlayQuoteFragment` 的数据源。

将新 C# 类添加到 FragmentSample  项目，并将它命名为 Shakespeare.cs  。 在此文件中，新建名为 `Shakespeare` 的 C# 类，其中包含以下内容

```csharp
class Shakespeare
{
    public static string[] Titles = {
                                      "Henry IV (1)",
                                      "Henry V",
                                      "Henry VIII",
                                      "Richard II",
                                      "Richard III",
                                      "Merchant of Venice",
                                      "Othello",
                                      "King Lear"
                                    };

    public static string[] Dialogue = {
                                        "So shaken as we are, so wan with care, Find we a time for frighted peace to pant, And breathe short-winded accents of new broils To be commenced in strands afar remote. No more the thirsty entrance of this soil Shall daub her lips with her own children's blood; Nor more shall trenching war channel her fields, Nor bruise her flowerets with the armed hoofs Of hostile paces: those opposed eyes, Which, like the meteors of a troubled heaven, All of one nature, of one substance bred, Did lately meet in the intestine shock And furious close of civil butchery Shall now, in mutual well-beseeming ranks, March all one way and be no more opposed Against acquaintance, kindred and allies: The edge of war, like an ill-sheathed knife, No more shall cut his master. Therefore, friends, As far as to the sepulchre of Christ, Whose soldier now, under whose blessed cross We are impressed and engaged to fight, Forthwith a power of English shall we levy; Whose arms were moulded in their mothers' womb To chase these pagans in those holy fields Over whose acres walk'd those blessed feet Which fourteen hundred years ago were nail'd For our advantage on the bitter cross. But this our purpose now is twelve month old, And bootless 'tis to tell you we will go: Therefore we meet not now. Then let me hear Of you, my gentle cousin Westmoreland, What yesternight our council did decree In forwarding this dear expedience.",
                                        "Hear him but reason in divinity, And all-admiring with an inward wish You would desire the king were made a prelate: Hear him debate of commonwealth affairs, You would say it hath been all in all his study: List his discourse of war, and you shall hear A fearful battle render'd you in music: Turn him to any cause of policy, The Gordian knot of it he will unloose, Familiar as his garter: that, when he speaks, The air, a charter'd libertine, is still, And the mute wonder lurketh in men's ears, To steal his sweet and honey'd sentences; So that the art and practic part of life Must be the mistress to this theoric: Which is a wonder how his grace should glean it, Since his addiction was to courses vain, His companies unletter'd, rude and shallow, His hours fill'd up with riots, banquets, sports, And never noted in him any study, Any retirement, any sequestration From open haunts and popularity.",
                                        "I come no more to make you laugh: things now, That bear a weighty and a serious brow, Sad, high, and working, full of state and woe, Such noble scenes as draw the eye to flow, We now present. Those that can pity, here May, if they think it well, let fall a tear; The subject will deserve it. Such as give Their money out of hope they may believe, May here find truth too. Those that come to see Only a show or two, and so agree The play may pass, if they be still and willing, I'll undertake may see away their shilling Richly in two short hours. Only they That come to hear a merry bawdy play, A noise of targets, or to see a fellow In a long motley coat guarded with yellow, Will be deceived; for, gentle hearers, know, To rank our chosen truth with such a show As fool and fight is, beside forfeiting Our own brains, and the opinion that we bring, To make that only true we now intend, Will leave us never an understanding friend. Therefore, for goodness' sake, and as you are known The first and happiest hearers of the town, Be sad, as we would make ye: think ye see The very persons of our noble story As they were living; think you see them great, And follow'd with the general throng and sweat Of thousand friends; then in a moment, see How soon this mightiness meets misery: And, if you can be merry then, I'll say A man may weep upon his wedding-day.",
                                        "First, heaven be the record to my speech! In the devotion of a subject's love, Tendering the precious safety of my prince, And free from other misbegotten hate, Come I appellant to this princely presence. Now, Thomas Mowbray, do I turn to thee, And mark my greeting well; for what I speak My body shall make good upon this earth, Or my divine soul answer it in heaven. Thou art a traitor and a miscreant, Too good to be so and too bad to live, Since the more fair and crystal is the sky, The uglier seem the clouds that in it fly. Once more, the more to aggravate the note, With a foul traitor's name stuff I thy throat; And wish, so please my sovereign, ere I move, What my tongue speaks my right drawn sword may prove.",
                                        "Now is the winter of our discontent Made glorious summer by this sun of York; And all the clouds that lour'd upon our house In the deep bosom of the ocean buried. Now are our brows bound with victorious wreaths; Our bruised arms hung up for monuments; Our stern alarums changed to merry meetings, Our dreadful marches to delightful measures. Grim-visaged war hath smooth'd his wrinkled front; And now, instead of mounting barded steeds To fright the souls of fearful adversaries, He capers nimbly in a lady's chamber To the lascivious pleasing of a lute. But I, that am not shaped for sportive tricks, Nor made to court an amorous looking-glass; I, that am rudely stamp'd, and want love's majesty To strut before a wanton ambling nymph; I, that am curtail'd of this fair proportion, Cheated of feature by dissembling nature, Deformed, unfinish'd, sent before my time Into this breathing world, scarce half made up, And that so lamely and unfashionable That dogs bark at me as I halt by them; Why, I, in this weak piping time of peace, Have no delight to pass away the time, Unless to spy my shadow in the sun And descant on mine own deformity: And therefore, since I cannot prove a lover, To entertain these fair well-spoken days, I am determined to prove a villain And hate the idle pleasures of these days. Plots have I laid, inductions dangerous, By drunken prophecies, libels and dreams, To set my brother Clarence and the king In deadly hate the one against the other: And if King Edward be as true and just As I am subtle, false and treacherous, This day should Clarence closely be mew'd up, About a prophecy, which says that 'G' Of Edward's heirs the murderer shall be. Dive, thoughts, down to my soul: here Clarence comes.",
                                        "To bait fish withal: if it will feed nothing else, it will feed my revenge. He hath disgraced me, and hindered me half a million; laughed at my losses, mocked at my gains, scorned my nation, thwarted my bargains, cooled my friends, heated mine enemies; and what's his reason? I am a Jew. Hath not a Jew eyes? hath not a Jew hands, organs, dimensions, senses, affections, passions? fed with the same food, hurt with the same weapons, subject to the same diseases, healed by the same means, warmed and cooled by the same winter and summer, as a Christian is? If you prick us, do we not bleed? if you tickle us, do we not laugh? if you poison us, do we not die? and if you wrong us, shall we not revenge? If we are like you in the rest, we will resemble you in that. If a Jew wrong a Christian, what is his humility? Revenge. If a Christian wrong a Jew, what should his sufferance be by Christian example? Why, revenge. The villany you teach me, I will execute, and it shall go hard but I will better the instruction.",
                                        "Virtue! a fig! 'tis in ourselves that we are thus or thus. Our bodies are our gardens, to the which our wills are gardeners: so that if we will plant nettles, or sow lettuce, set hyssop and weed up thyme, supply it with one gender of herbs, or distract it with many, either to have it sterile with idleness, or manured with industry, why, the power and corrigible authority of this lies in our wills. If the balance of our lives had not one scale of reason to poise another of sensuality, the blood and baseness of our natures would conduct us to most preposterous conclusions: but we have reason to cool our raging motions, our carnal stings, our unbitted lusts, whereof I take this that you call love to be a sect or scion.",
                                        "Blow, winds, and crack your cheeks! rage! blow! You cataracts and hurricanoes, spout Till you have drench'd our steeples, drown'd the cocks! You sulphurous and thought-executing fires, Vaunt-couriers to oak-cleaving thunderbolts, Singe my white head! And thou, all-shaking thunder, Smite flat the thick rotundity o' the world! Crack nature's moulds, an germens spill at once, That make ingrateful man!"
                                    };
}
```

## <a name="3-create-the-playquotefragment"></a>3.创建 PlayQuoteFragment

`PlayQuoteFragment` 是 Android 片段，它显示用户之前在应用程序中选择的莎士比亚戏剧中的引文。此片段不会使用 Android 布局文件；相反，它会动态创建自己的用户界面。 向项目添加名为 `PlayQuoteFragment` 的新 `Fragment` 类：

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![添加新 C# 类](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![添加新 C# 类](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

然后，将片段的代码更改为以下代码片段：

```csharp
public class PlayQuoteFragment : Fragment
{
    public int PlayId => Arguments.GetInt("current_play_id", 0);

    public static PlayQuoteFragment NewInstance(int playId)
    {
        var bundle = new Bundle();
        bundle.PutInt("current_play_id", playId);
        return new PlayQuoteFragment {Arguments = bundle};
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        if (container == null)
        {
            return null;
        }

        var textView = new TextView(Activity);
        var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
        textView.SetPadding(padding, padding, padding, padding);
        textView.TextSize = 24;
        textView.Text = Shakespeare.Dialogue[PlayId];

        var scroller = new ScrollView(Activity);
        scroller.AddView(textView);

        return scroller;
    }
}
```

这是 Android 应用中的常见模式，用于提供将实例化片段的工厂方法。 这确保创建的片段包含正常运行所必需的参数。 在本演练中，应用应使用 `PlayQuoteFragment.NewInstance` 方法，以便在每次选择引文时新建片段。 `NewInstance` 方法需要使用一个参数 &ndash; 要显示的引文的索引。

当需要在屏幕上呈现片段时，Android 会调用 `OnCreateView` 方法。 它会返回作为片段的 Android `View` 对象。 此片段不使用布局文件来创建视图。 相反，它通过将 TextView  实例化为保留引文来以编程方式创建视图，并在 ScrollView  中显示此小组件。

> [!NOTE]
> 片段子类必须有公共的默认无参数构造函数。

## <a name="4-create-the-playquoteactivity"></a>4.创建 PlayQuoteActivity

片段必须在活动内托管，所以此应用需要托管 `PlayQuoteFragment` 的活动。 活动将在运行时将片段动态添加到它的布局中。 将新活动添加到应用程序，并将它命名为 `PlayQuoteActivity`：

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![向项目添加 Android 活动](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![向项目添加 Android 活动](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

在 `PlayQuoteActivity` 中编辑代码：

```csharp
[Activity(Label = "PlayQuoteActivity")]
public class PlayQuoteActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        var playId = Intent.Extras.GetInt("current_play_id", 0);

        var detailsFrag = PlayQuoteFragment.NewInstance(playId);
        FragmentManager.BeginTransaction()
                        .Add(Android.Resource.Id.Content, detailsFrag)
                        .Commit();
    }
}
```

创建的 `PlayQuoteActivity` 会实例化新的 `PlayQuoteFragment`，并在 `FragmentTransaction` 上下文中在根视图中加载此片段。 请注意，此活动不会为用户界面加载 Android 布局文件。 相反，新的 `PlayQuoteFragment` 会添加到应用程序的根视图中。 资源标识符 `Android.Resource.Id.Content` 用于引用活动的根视图，而无需知道它的特定标识符。

## <a name="5-create-titlesfragment"></a>5.创建 TitlesFragment

`TitlesFragment` 子类化称为 `ListFragment` 的专用片段，它封装了用于在片段中显示 `ListView` 的逻辑。 `ListFragment` 公开 `ListAdapter` 属性（被 `ListView` 用来显示它的内容）和名为 `OnListItemClick` 的事件处理程序（可便于片段响应用户单击 `ListView` 显示的行）。

首先，请向项目添加新片段，并将它命名为 TitlesFragment  ：

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![向项目添加 Android 片段](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![向项目添加 Android 片段](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

编辑片段内的代码：

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;

    public TitlesFragment()
    {
        // Being explicit about the requirement for a default constructor.
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

在活动创建后，Android 会调用片段的 `OnActivityCreated` 方法；其中创建 `ListView` 的列表适配器。  `ShowQuoteFromPlay` 方法会启动 `PlayQuoteActivity` 的实例，以显示选定戏剧的引文。

## <a name="display-titlesfragment-in-mainactivity"></a>在 MainActivity 中显示 TitlesFragment

最后一步是，在 `MainActivity` 中显示 `TitlesFragment`。 活动不会动态加载片段。 相反，片段是静态加载的，具体方法是使用 `fragment` 元素在活动的布局文件中声明片段。 通过将 `android:name` 特性设置为片段类（包括类型的命名空间），标识要加载的片段。 例如，若要使用 `TitlesFragment`，则 `android:name` 会设置为 `FragmentSample.TitlesFragment`。

编辑布局文件 activity_main.axml  ，将现有 XML 替换为以下内容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment
        android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

> [!NOTE]
> `class` 特性可以有效替换 `android:name`。 对于哪种形式是首选，并没有正式的指导，有很多基本代码示例将 `class` 与 `android:name` 互换使用。

MainActivity 无需更改代码。 此类中的代码应与下面的代码片段非常相似：

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        SetContentView(Resource.Layout.activity_main);
    }
}
```

## <a name="run-the-app"></a>运行应用

至此，代码已完成，请在设备上运行应用，以查看实际演示效果。

[![在电话上运行应用程序的屏幕截图。](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

[本演练的第 2 部分](./walkthrough-landscape.md)将把此应用优化为，更适合在采用横向模式的设备上运行。
