大家好，能听到我的声音吗？
好的，欢迎回到你们的计算机科学教育中缺失的学期。
今天我们的讲座主题是“杂项”。
这将是一些我们讲师认为有趣但又不足以成为单独讲座的主题的组合。
但是它们都是我们希望你们知道的一些特定主题，因为它们可能非常有用。
由于我们不会详细探讨这些主题，如果你对它们更感兴趣，可以随时在讲座结束后或讲座期间向我们提问。
所以我想先谈谈键盘重新映射。
到现在为止，你可能已经意识到我们鼓励你使用键盘作为主要的输入方法。
例如，当我们进入编辑器讲座时，Vim 的一个主要思想就是尽可能多地使用键盘。
这样你就不必依赖鼠标，因为使用鼠标是慢的。
事实上，你的键盘，就像电脑中的许多其他东西一样，都可以进行配置。
而且配置是值得的，因为许多默认设置可能不是最优的。
最简单的修改方法就是重新映射键。
例如，在编辑器讲座中我们提到，大写锁定键是一个非常好的键，因为它位于主键盘区，而且很大，但它并没有什么用处。
你可能会意识到你不常使用大写锁定键。
因此，你可以将大写锁定键重新映射为更有用的键，如 Escape（如果你是 Vim 用户）或 Control（如果你是 Emacs 用户）或其他实用的重新映射，如 F 功能键或 Print Screen。
例如，你可以将它们重新映射为媒体键，这样当你按下 Print Screen 时，你可能不需要那么频繁地截屏，但你可能需要播放或暂停音乐。
几乎每个操作系统都有一些工具可以用来配置这些功能。
我不会详细介绍，但在注释中列出了其中一些。
哦，是的，你可以使用键盘重新映射来执行更复杂的组合操作。
你可以将一些键的组合映射到某些操作上。
例如，我有一些键盘重新映射，每当我按下 Ctrl+Enter 时，就会打开一个新的终端窗口。

因为这是我经常做的事情，并且在Mac上默认情况下没有绑定按键来执行该操作，或者Ctrl + Shift + Enter将打开一个新的浏览器窗口，这是我每天都要做的另一个操作。
所以我不必拿起鼠标去Chrome执行它。
你也可以重新映射来执行操作。
如果你不想一直输入你的密码......不好意思，不是你的密码，是你的电子邮件或密码，或者例如你的MIT ID，你可能不记得它，那么你可以有一个键盘组合键来执行粘贴文本的操作。
最后，还有更多...现在看起来你只需要执行“这是你按下的键和这是发生的动作”的功能。
但实际上有更复杂的键盘组合键，当你不断学习时，你会发现你可以执行键盘序列。
所以例如当我们处理tmux时，在tmux中有这个概念，首先你按下Ctrl + A或Ctrl + B，就像你按下一些前缀然后一些其他的键，那意味着某些东西。
很多这些软件都允许这样做。
例如，在我的键盘上，因为我根本不使用Caps Lock，但偶尔我的本科生有一些依赖Caps Lock更改模式的软件。
然后我可以快速连续按五次Shift键，然后处于中间的解释这些命令并重新映射到其他命令的软件将为此发送一个单独的Caps Lock命令。
更多的例子是我提到你可以使用你的Caps Lock键映射到Escape或Control，但实际上你可以重新映射它们两个。
所以在我的电脑上，当我轻按Caps Lock键时，它被解释为Escape。
然而，如果我按住它，这个软件可以理解快速按下它和仅仅按住它以与其他键组合使用的区别，然后在这种情况下它被映射到Control。
很多这些更高级的配置都得到了许多这些工具的支持。
正如我所提到的，我们有一个关于Windows、Mac OS和Linux的这些程序的良好默认值的简短列表。
这个话题有什么问题吗？好的，现在我要讲解一个与键盘映射无关的话题[笑声]。
我们将在本讲座中看到许多这些不相关的过渡。
这就是“守护进程”的概念。
所以你可能......也许如果你不熟悉这个领域，这个概念看起来很陌生，但你对守护进程这个概念应该已经很熟悉了。
大多数计算机在运行时会启动和运行一些软件，就像我们一直在看到的那些命令一样，比如输入“ls”，就是调用“列出文件列表”命令。

"ls"命令执行是因为你要求它执行，然后它就完成了。
但是许多其他程序只是作为后台进程运行，它们只是在后台执行并等待事件发生或启用计算机的某些功能。
这些进程的示例可能包括像网络管理器这样的部分，或者像管理显示的计算机部分。
您会发现，守护进程启用的大多数内容通常是以'd'结尾的程序。
例如，当您通过SSH连接到计算机时，接收计算机必须具有SSH守护进程，该程序称为"sshd"，如果此程序未运行，则无法通过SSH连接到计算机。
如果该程序正在运行，则该程序将正在侦听，当您SSH到该服务器时，一些传入请求将进入计算机，计算机将将其发送到在后台运行的该守护进程，然后该守护进程将检查您是否有授权，如果有，则将启动一些登录Shell，以便您可以开始执行。
不同的操作系统处理此问题的方式有所不同，但它们都有某种响应这些较小守护进程的系统守护进程。
在我们为许多示例选择的Linux中，您正在使用的工具是"systemd"。
首先，系统守护进程将启动许多这些进程，如果您使用"systemctl"命令，则可以检查不同守护进程的状态，您可以检查哪些正在运行，可以要求它启动进程或停止它们。
这是一种一次性的操作。
您还可以启用它，然后禁用它们，这将告诉系统在启动时运行它们或停止启动它们。
更有趣的是，您可以配置自己的"systemd"单位。
到目前为止，所有示例都是计算机必须执行的许多内容，但是假设您想运行Web服务器。
一个解决方案是，您可以每次启动计算机时打开tmux会话，然后执行命令，但这不是计算机期望守护进程运行的方式。
计算机期望守护进程运行的方式是使用某种"systemd"单位。
这是一种告诉"systemd"如何执行此进程的配置。
这个的一个简单例子是：
这里发生的事情是我们正在向"systemd"描述需要执行此程序的步骤。
这个例子只是运行一个简单的Python应用程序，你可以将其视为使用某些Python Web服务器库实现的Web服务器。
我们在这里说的是这个描述，我们说在某个之后，像这样很重要，"Systemd"有一个必须启动的服务列表，像所有这些守护进程都必须启动，但是可能存在这些守护进程之间的依赖关系，所以我们在这里说"不，只有在网络设置完成后才能启动此服务"，因为否则，如果我无法侦听网络端口，你甚至如何尝试配置我们的Web服务器呢？然后我们定义了应该运行此服务的用户，因为你可能想要将其作为自己的用户运行，或者可能是其他用户，或者可能是root用户在运行此服务，然后定义了要运行的命令和在哪个目录下运行。
当你有这个时，可能会有一些小的角落情况需要调试，但这是核心思想，可以非常有用地自动化后台运行进程的过程。
对此的一个小侧面说明是，如果你只想定期运行一个命令，比如每天早上我都想在我的计算机上做一些事情，你可以编写一个守护进程，它只做一些事情然后睡觉一天，但实际上Linux和Mac OS已经有了一个这样做的守护进程，称为"crond"，"crond"将采用另一种类型的配置文件，你可以说哦，我想在每天上午8点运行一个命令，或者我想每5分钟运行一个命令，它将只检查此事件并执行它。
对于许多事情，你会发现已经配置了相应的守护进程。
有关守护进程的任何问题吗？[学生含糊不清地讲话]问题是是否有一个计算机上的文件夹，其中包含所有这些配置文件？是和不是，这些配置文件中的一些位于几个不同的文件夹中，具体取决于它们是系统守护进程还是用户守护进程。
在这里，您可以看到在第一行的位置是将此放置在系统守护进程中以便其被识别为已安装的位置，但是如果您只想列出正在运行的所有守护进程，则例如在Linux中，您只需执行“systemctl status”，它将打印出所有系统的树状结构，以及哪个守护进程是由哪个其他守护进程产生的，其中许多直接由“systemd”生成。
下一个话题将是用户空间的文件系统，所以，对此进行快速介绍的事实是，每当您使用现代操作系统时... 哦，对不起，还有一件事，每当您使用现代操作系统时...
你不必局限于特定的文件系统。
现代系统相当模块化，例如在Linux中有不同的文件系统可供选择。
这是因为内核是运行大部分操作系统的程序，有一些模块知道如何与文件系统进行交互。
通常，当你执行像"touch foobar"这样的命令时，这是在用户级别进行的，然后通过内核级别，并有一层检查在这个操作发生的位置，以确定它属于哪个文件系统。
例如，你将拥有多个磁盘，每个磁盘都有不同的文件系统，因此内核必须确定要使用哪个文件系统操作，并说这个文件可能在一个"Ext4"文件系统中，这是最常见的Linux文件系统之一。
那么，每当你执行"touch foobar"时，内核会听到并试图找出它属于Ext4文件系统，然后执行创建文件的相关指令。
然而，这种系统的缺点是现在我不能拥有定义如何创建文件的用户代码，在某些情况下这可能会很有用。
比如说，我想拥有一个文件系统，每次有人创建文件时，它会给我发送一封电子邮件，这样我就可以知道人们正在创建这些文件。
但是在这里我无法修改内核来添加这个功能。
解决这个问题的方法叫做"FUSE"，FUSE是一种在用户空间中拥有文件系统的方法。
因此，如果这个文件不是在Ext4中，而是在FUSE文件系统中，FUSE将把这个操作转发到用户调用的某个其他部分，该部分将说"哦，创建这个文件"。
在这里，我可以拥有发送电子邮件给我的代码部分，告诉我"哦，这个文件已经被创建了"。
如果你仍然想创建该文件，它可以把请求转发回来，做一些更多的内核操作。
这似乎并不是很实用，但这只是理论上的。
在实践中，这很有用，因为现在你可以在尝试执行文件系统操作时执行任意动作的用户级代码。
一个非常有趣的例子是"FUSE"
在一个SSHFS FUSE文件系统上，无论你尝试创建、打开、读取、写入一个文件，它都会通过SSH连接到远程服务器，而不是尝试在本地文件上执行操作。
因此，如果我在这里尝试创建一个文件，它将使用SSH连接将该操作转发到远程系统，然后在那里执行。
因此，在我的本地计算机上，在运行的其他程序中，有一个路径看起来好像它在这里，但是对路径执行的所有操作都被转发到远程文件系统。
基于这个想法，你会在笔记中找到一些例子，并在线上找到更多，说明人们如何利用这种能力来创建相当有趣的文件系统。
例如，如果你不关心SSH，因为你使用Dropbox或Google Drive，那么人们已经实现了FUSE文件系统，可以在本地挂载，每次你尝试在本地执行操作时，实际上它会去这些云存储提供商之一，所以你也可以使用像Amazon S3或Google Cloud Storage这样的服务，它们没有像Dropbox或Google Drive那样的同步UI系统。
这种方法的另一个应用与远程操作无关，就是加密文件系统。
你可能有一个文件系统，每次你尝试写入一个文件时，你都会尝试以明文形式写入它，但它会捕获该操作，动态地加密它，然后将其保存为常规文件在你的文件系统中，但实际上是加密的。
一旦你卸载文件系统，一旦你断开FUSE连接，你的计算机上只剩下一些加密的常规文件。
我想要讨论的最后一个主题是备份和一些关于它们的良好做法。
主要的想法是，对于你关心的每个文件，如果你没有备份它，如果你没有存储它的备份，你几乎可以在任何时候失去它。
有许多不同的故障场景。
其中之一就是硬件故障。
所以你的硬盘可能在任何时刻都会出现故障。
因此，如果你只是在同一个驱动器上复制你的文件，那是没有用的。
如果你的硬盘故障了，文件就丢失了。
如果你在外部驱动器上进行备份，但是如果你把所有东西都存储在你的家里，而你的家着火了......这虽然不太可能，但如果发生了，你就失去了所有的数据。

所以问题是电脑中是否有一个文件夹包含了这些备份，因此你需要有一个离线备份的解决方案。
还要注意的一件事是同步或镜像选项不是备份。
所以像我之前提到的 Google Drive 和 Dropbox 只会传播你电脑上发生的任何事情。
这也适用于硬件镜像，如 RAID。
它们只是在制作副本。
如果你不小心删除了一个文件，或者有人恶意删除了你的文件，或者使用某些勒索软件对它们进行加密，那么你可能会有一个副本，但你只有一份无用的数据副本。
你必须拥有备份运行的解决方案。
你应该问自己，有什么人需要知道或拥有你的信息才能删除你所有的数据。
我们在笔记中链接了不同的软件，介绍了如何备份。
最后我要提到的是备份时，很多时候你只会考虑本地文件，比如所有的照片和报税申报表，怎么备份？但在现代，越来越多的网络应用程序和很多数据只可能存在于一些云服务提供商，比如如果你使用 Webmail，而且你没有将其同步到电脑上，它只存在于提供商的服务器上。
如果你没有这个数据的备份，并且由于某些原因无法访问该帐户，因为你忘记了密码，被黑客攻击，他们认为你违反了服务条款...所有的数据都会消失。
因此，你应该寻找一些工具来制作所有这些数据的离线副本，这样你就可以定期备份。
这个短小的备份部分就到这里了，有什么问题吗？（学生）当你说硬盘随时会出故障时，它会出故障的原因是什么？[不清楚听到的话]比如说我把外接硬盘放在我父母家里或者其他地方，我的电脑在这里，这样就足够了吗？还是任何硬盘都可能随时失效？（Jose）任何硬盘都可能随时失效，我们没有一个完美的解决方案。
不同的媒体有不同的故障率，在网上有非常好的统计数据，例如，旋转硬盘的故障率比固态硬盘高。
如果你将硬盘掉落，故障率会更高，但总体上我们没有一个完美的解决方案。

关于“这个媒体不会失败”的说法，实际上，像SD卡、固态硬盘、硬盘、CD等都会随着时间而逐渐退化，基本上每种数据都会因此而有可能在任何时刻丢失。
此外，你还需要知道数据也有可能会损坏，你的硬盘看起来可能没问题，但是有些文件可能已经损坏了，而像Google Drive或Dropbox这样的同步技术会传播这种损坏。
等到你意识到出现问题时，可能已经太晚了。
（Jon）好的，我们将继续跳跃到不同的主题并讨论API。
到目前为止，我们一直在讨论如何在本地计算机上更有效地完成任务。
例如，我想更有效地完成此任务。
我该如何配置我的编辑器？我该如何使用我的shell？但你应该意识到，通常你也可以与外界集成。
你日常互动的大多数服务都提供某种API，供你与其存储的数据或提供的服务进行交互。
而且通常这些API都有很好的文档。
如果你查看Facebook、Twitter、Google Drive或Gmail等服务的API，你会发现许多接口都可以供你从本地机器上使用这些服务。
真正有趣的是，你通常可以将此与我们迄今为止在课堂上讨论的一些内容相结合。
例如，在数据整理的讲座中，我们介绍了如何创建这些管道以从某些具有不同格式的源中提取数据。
例如，美国政府提供了一个免费服务，你可以请求任何美国地点的天气预报。
你所要做的就是请求一个URL，并在其中设置正确的参数，然后获取数据，你将得到JSON，这是一种定义良好的数据格式，你可以解析它，并提取像未来14天的天气预报之类的信息，然后你可以将其导入到shell中，并生成一些方便的别名，以便在终端中打印出任何位置的未来14天天气的方便参考。

这些事情你可以相对容易地构建，关于如何构建的说明在笔记中。
一般来说，当你与这些API交互时，你会使用某种形式的URL，而确切的格式因服务而异。
但总的来说，URL将包含一些参数集。
最终，你只需向它们发出一个Web请求，然后以某种格式获取数据。
你应该知道的一个命令是“curl”。
所以curl是一个你调用的程序，你给它一个URL。
它只是获取该URL并把响应返回给你。
你对响应做什么，完全取决于你。
也许你会通过像“jq”这样的程序进行管道处理。
所以jq是一种JSON查询工具，可以接收JSON格式的数据，然后编写查询以提取你感兴趣的数据。
这是你可以使用这些工具提取你感兴趣的数据的一种方式。
 这些服务中的一些还需要你以某种方式进行身份验证。
例如，如果你想与Facebook API交互，你需要拥有一些认证令牌，证明你是Facebook关心的那个人。
否则，他们不能说你是否被允许以某个用户身份创建帖子。
很多时候，这些东西将使用OAuth，虽然并不总是这样。
你应该查看你关心的每个服务的文档。
但总的来说，你将从服务中获得一些秘密令牌，你必须在你发给它们的请求中包含它们。
无论是在URL中还是在附加的Web头中，你也可以通过curl发送它们。
但要记住，这些令牌是秘密的。
它们是你的用户的另一种表现形式，任何拿到它们的人都可以假装是你。
他们可以用那个令牌做你能做的一切。
所以要记住这一点，不要把它们放在你的“dotfiles”中，然后把它们推到GitHub上。
这会让你陷入麻烦。
你应该把它们当作密码来考虑。
在线上也有一些非常好的工具，可以将服务集成在一起。
有一个叫做“If This Then That”的服务，它基本上提供了对一堆不同的服务的集成，并允许你将它们链接在一起，并且如果你愿意，也可以部分地在本地访问它们。
这是值得研究的东西。

如果你想以更高效的方式与特定服务进行交互，有关API的问题请问？好的，我们完全切换话题，讨论命令行参数。
命令行工具有很多，大多数都需要不同的参数，因为它们执行不同的操作。
我们已经谈到过查看命令的man页面，这会告诉你如何使用这个特定的命令，你可以给它什么样的标志和选项，以及当你调用它时它实际执行了什么。
但是有一些常见的主题是有用的，无论是许多程序使用的参数，还是许多程序都适用的概念。
第一个我们在命令行环境讲座中已经提到了一些，那就是"--help"标志。
通常你可以将这个标志传递给程序，而不是运行它，它就会打印有关如何运行这个程序的信息，通常是一种非常简短、压缩的方式。
类似的一个是"--version"标志，它只打印你正在使用的软件版本。
如果你正在进行诸如提交错误报告之类的操作，并且想要报告你正在运行的版本，以防该错误已经修复，那么这非常方便。
通常你也可以使用"-V"，它与版本相同。
但是要检查一下man页面。
还有一个经常用到的是"--verbose"或"-v"标志，它允许你增加程序的输出，使程序打印更多有关它正在做什么的信息。
通常你可以重复使用这个标志，比如使用"-vvvvv"来从该工具获取更多的信息。
如果你正在尝试调试问题，这可能特别有用。
如果你正在运行"rsync"，并且想要知道它为什么决定复制这个文件，或者为什么决定不复制这个文件，那么这种调试输出就很有用了。
通常还有一个相反的标志叫做"quiet"或"silent"，它意味着工具不会打印任何东西，除非出现错误。
很多工具，特别是那些执行破坏性操作或你无法撤销的操作，提供了所谓的"模拟运行标志"。
这在命令行中的表示方式因工具而异，但基本上，这个模拟运行模式会运行该工具，但实际上不会进行任何更改，而是只会告诉你如果你没有使用模拟运行，它将会做什么。

许多这些工具也有交互模式。
例如，“rm”和“mv”工具都有交互模式，通常只需要“-i”，但不总是这样。
当您在交互模式下运行工具时，通常会在即将执行无法撤消的操作时提示您，并提示您确认是否应该执行。
当我们谈论破坏性工具时，其中许多默认情况下都不递归。
如果您尝试删除一个目录或对一个完整的目录进行操作，则它们不会继续进入该目录内部的文件。
原因是您可能会意外地删除整个硬盘，这似乎很糟糕。
因此，对于许多这些工具，它们都有一个“recurse”标志，通常为“-r”，但并非总是如此，它让它们向下遍历树以深入了解，但您需要选择使用此行为。
例如，“rm”的情况就是如此，这也是“cp”的情况。
在许多工具中，当它们要求您提供文件名或路径时，我们在数据整理讲座中也稍微谈到了这一点，您通常可以只给出一个破折号“-”，这意味着标准输入或标准输出，具体取决于该参数是输入文件还是输出文件。
如果您正在尝试构建我们之前谈到的数据整理流水线，那么了解这一点非常有用。
如果您想要传递看起来像是一个标志或选项的东西给一个命令，但您实际上不想将其解释为标志或选项，则有时需要考虑一些问题。
例如，如果您想删除名为“-i”的文件，该怎么办？是的，如果您编写以下命令...“rm -i”，那么“-i”是一个“rm”的标志，因此当您运行此命令时，“rm”会说“告诉我要删除哪个文件，您没有给我一个文件”，因为它将其解释为一个标志。
同样，如果您做这样的事情...“ssh某台机器，某个命令，例如，dash r”，这就是说-在SSH上运行命令“foo”，并且我想向“foo”传递“-r”，那么这两个命令将被解释为标志。
但对于这个命令来说，这可能不是您预期的。
实际上，在SSH的情况下，它对某些命令有一些奇怪的特殊行为。
但是，如果您想要某些东西不被解释为标志，则有一种非常简单的方法可以选择退出，那就是使用双破折号。

如果你使用双破折号，那么你告诉命令的是，紧跟在其后的内容不应被解释为标志或选项。
在“rm”命令的情况下，你可以这样做，现在你会看到第一个参数是“--”，然后它会继续读取参数，但不会将它们解释为标志。
因此，当它遇到“-i”时，它不会将其解释为破折号i标志，而只是作为参数i。
同样，对于SSH，你也可以这样做，以指示这些都是位置参数，它们不是标志或选项，你不应解释以破折号开头的内容（学生）。
但是，如果你使用“--version”，它不会触发这个吗？（Jon）不会，这是一个“--”，两侧有空格。
你有任何关于命令行约定的问题吗？好的，那么让我们谈谈窗口管理器。
所以你们大多数人习惯于某种拖放窗口管理器，如果你正在运行Windows、Mac OS或Ubuntu，那么这些机器自带的就是窗口，它们在屏幕上部分重叠，你可以拖放和移动它们，调整它们的大小等等。
这种方式可以正常工作，但它并不是管理计算机窗口的唯一方式。
实际上，你习惯的是一种浮动窗口管理器，但并不是所有的窗口管理器都是浮动的。
通常你可以选择其他类型的窗口管理器，它们有不同的行为方式来布置你的桌面。
一种常见的替代方案是平铺窗口管理器。
在平铺窗口管理器中，不再有浮动窗口，一切都设置成平铺布局。
当你启动一个程序时，它的窗口被最大化。
如果你启动另一个程序，原来的窗口会缩小，新的窗口会占用总桌面空间的一部分。
除非没有打开任何程序，否则在任何给定的桌面上打开的所有程序都将共享这个空间。
这看起来有点像之前我们谈到的tmux面板，你可以在不同的方向上将它们分割。
这种方式的一个好处是你基本上不需要使用鼠标，在不同的窗口之间移动时，有键盘快捷键可以使用。
有键盘快捷键可以调整窗口的大小或在屏幕上交换它们的位置，这被证明是一种相当高效的管理计算机窗口的方式。
我不会详细介绍你可能会使用哪种窗口管理器，只要知道这些存在并值得一看就好了。

它们可以更加高效地工作。
关于窗口管理器有什么问题吗？好的，虚拟私人网络（VPN）。
与之前的主题有完全关联。
所以虚拟私人网络现在非常流行，这让我感到非常难过。
目前并不清楚使用虚拟私人网络是否有很好的理由。
因为你应该意识到虚拟私人网络能够做什么，以及不能做什么。
在最好的情况下，虚拟私人网络只是一种更改你的互联网服务提供商的方法。
这是一种让互联网上的流量看起来来自于一个比你实际所在的地方更远的地方的方法。
虽然这在某些情况下可能看起来很有吸引力，但它对于安全性能有什么帮助并不十分明确，因为你只是转换了你信任的对象。
你不再信任你当前的互联网服务提供商，而是信任提供你虚拟私人网络服务的企业……你首先要相信他们是否已正确设置了这个虚拟私人网络业务，还要相信他们不会追踪你正在做什么。
这种信任的改变是否真的值得呢？如果你正在使用一些不安全的公共Wi-Fi网络，那么也许值得考虑。
但如果你在麻省理工学院，这并不明显。
你是否比信任麻省理工学院的IS&T更信任你的VPN提供商？或许你是这样认为的，但这是你需要考虑的决定，关于你信任什么、信任谁以及为什么？你还应该知道，你的很多流量，尤其是敏感性数据在互联网上，已经被加密了。
无论是HTTPS还是其他使用类似TLS的协议的数据，大部分敏感数据都已经通过加密通道发送。
如果你在一个不安全的Wi-Fi网络上，你的网络提供商是谁并不重要，重要的是你的流量是否加密。
那些重要的流量可能已经被加密了，也可能没有，如果没有，你的VPN提供商也能像托管这个不安全的Wi-Fi网络的人一样看到它的明文。
请注意，我在上面说的是“在最好的情况下”。
有些VPN提供商被证明是恶意的，会记录你的所有流量，将流量出售给第三方。
有些VPN提供商忘记在VPN上启用加密，所有这些都是真实存在的问题。
因此，您应该非常仔细地考虑VPN是否真正为您提供了任何好处。
有关VPN的问题？是的？（学生）所以我有一个关于公共Wi-Fi网络的问题，因为电脑和路由器之间的流量在计算机和路由器之间没有加密，对吧？除了通常通过HTTPS和[不可理解的]进行的内容。
那么这是否意味着人们可以通过DNS请求嗅探出我正在访问哪些域名？（乔恩）这是一个非常好的问题。
如果您在公共Wi-Fi网络上，则您与无线接入点之间的流量没有加密。
至少在外层上没有加密，但它可能在像HTTPS这样的加密层中加密。

这是完全正确的，观察Wi-Fi网络的人将能够看到未加密的任何内容。
但解决方法是加密所有流量，而不一定要通过VPN。
例如，可以使用DNS over TLS或DNS over HTTPS的方式来实现这一点，这可以使本来可能以明文泄露的信息得到加密。
而不是试图相信某个提供商为您做到这一点。
当然，在某些情况下，您可能有一个可信赖的机构为您提供VPN网络。
例如，MIT为所有MIT学生和工作人员提供VPN网络，您可以注册使用。
在这种情况下，您可能更信任MIT而不是其他可能连接的网络。
所以这可能值得一试，但这是您需要考虑的事情。
(学生)当你说你可以用DNS加密它，是什么意思？(Jon)DNS是人们将域名转换成IP地址的方式，或者说是您的计算机将域名转换成IP地址以了解要连接到哪台计算机的协议。
默认情况下，该协议是明文的，没有任何加密。
有各种各样的方法可以加密DNS流量。
其中一些是标准化的，而一些不是。
我不会在这里详细介绍机制，但您应该搜索一下并查看一些方法。
好的，我想谈论的最后一件事是Markdown。
因此，很有可能你们中的一些人会在余生中写文本，并且你们会想以各种简单的方式标记这些文本。
你可以启动Word或使用LaTeX等来标记你的文档，但这是一种相当笨重的方法。
相反，如果我们能够以自然的方式写东西，那将是很好的。
我不知道如何更好地描述它，但是它是自然的方式，如果你想强调一个词，你只需要在它周围放上星号或其他东西，然后它就起作用了。
Markdown本质上就是这样。
它是一种尝试将我们通常自然地编写文本的方式编码到一种标记语言中，使您可以编写粗体文本、链接、列表等等。
事实上，本课程的所有讲义都是使用Markdown编写的。
Markdown非常简单明了，基本规则在讲义中，但您需要知道的基本事项是，在Markdown中，如果您在一个词的周围放上星号，那么这个词就被强调了。
或者是一些词序列。
如果您在一个词的周围放上双星号，那么这个词就会被强调为加粗。
您还可以做各种其他的事情，例如如果你在一行前加上破折号，那么这一行就成为一个列表了。

如果在一行前加上一个破折号，它就成为一个列表，然后你就可以添加列表项了。
如果在列表项前加上“1.”或其他数字，它就成为了一个有序列表。
如果在内容前面加上井号，它就会变成标题，就像是某种标题。
如果加多个井号，它们就变成了子标题，你可以继续添加更多。
如果你想编写代码，可以在单引号之间输入代码，然后它将以等宽字体呈现。
如果要多行代码，可以使用三个反引号，然后是代码和更多的代码，最后再使用三个反引号。
在许多情况下，如果你在GitHub上，你甚至可以在反引号之后输入一个语言名称，而不需要空格，它会以你选择的语言进行语法高亮显示。
这是一件非常方便的事情，在如此多的网站上都支持它，你甚至可能没有意识到。
就像在Facebook Messenger中，你可以使用许多这样的功能。
它们没有正式说明它们在任何地方都支持Markdown，但许多这样的事情只是发生了，值得学习至少基础知识并开始使用它们。
你也可以创建链接，但这已经在笔记中提到了。
有关Markdown的问题吗？好的，Anish，轮到你了。
（Anish）我的麦克风工作吗？这个工作吗？你们能在后面听到我吗？灯亮了。
哦，我想我能听到。
好的，继续我们随机话题的主题，这些主题与我们之前讨论的主题都没有关系。
接下来我们要讨论的是一个叫做“Hammerspoon”的程序，它是Mac OS上进行桌面自动化的工具，我认为Windows和Linux也有类似的工具，很多想法可以继承下来。
如果你想知道如何在其他平台上实现这些功能，可以谷歌一下。
基本上，Hammerspoon是一个程序，它让你编写用Lua编写的脚本，这些脚本与各种操作系统功能进行交互。
因此，你可以编写代码来与键盘和鼠标交互，并将其连接到窗口管理、显示管理、文件系统、电池和电源管理、Wi-Fi等等。
基本上，所有操作系统管理的东西，这个工具都可以让你钩入这些东西。
因此，你可以通过编写几行代码做所有的很酷的事情。
这个工具可以做的一些很酷的事情的例子是：你可以绑定热键来移动窗口。

特定位置。
这里有一个演示。
我有这个窗口打开。
我按下，按照我的特定设置，"Option+Command+Right"，这个窗口就会向右移动。
"Option+Command+Left"，这个窗口就会向左移动。
我还有一些其他的快捷键可以将窗口移动到不同的位置。
这样我就可以达到类似于Jon之前谈到的平铺窗口管理器的效果，将窗口移动到屏幕的不同部分，以特定的方式设置东西，而不必使用鼠标将东西放在我想要的位置，然后点击和拖动来调整窗口的大小。
只需要一个键盘快捷键就可以搞定。
但是这个工具不仅限于仅仅移动窗口并将其绑定到特定的键盘快捷键上。
你还可以做其他的事情，比如创建一个菜单栏按钮，里面有很多不同的选项，你可以将这些不同的选项绑定到不同的操作。
所以在我的特定情况下，我创建了这个小菜单，然后我有很多不同的事情，我相当频繁地做这些事情，点击这些事情会调用我编写的一个特定的Lua函数，与这个库进行交互。
例如，这个“Rescue windows”是一个我经常使用多个显示器的特定问题，有时我的操作系统会混淆，我有一些窗口出现在我的显示器之外，我该如何将这个东西拿回来呢？那就是这个“Rescue windows”所做的事情。
它将那些超出屏幕的窗口重新放回屏幕上。
我在这里设置的另一个很酷的东西是我有特定的布局，我已经命名了它们。
比如一个宿舍、一个实验室和一个笔记本电脑布局。
所以在我的实验室里，我有这个屏幕和另一个屏幕和另一个屏幕，方向不同。
我有这个特定的设置，我想在这里可能我的终端全屏显示，在这里显示我的聊天程序，这个屏幕分成五个不同的部分，有不同的程序放在不同的地方。
在我到实验室的时候，我可以点击“布局实验室”，它将调用一些代码，描述了一个特定的布局，这并不是很复杂，只有10行代码，它会实例化这个布局，并把所有东西放到需要放的地方。
我甚至可以在理论上自动化其中的一些过程，让我的电脑可以自己识别，比如我插上一个显示器，我的电脑就知道：“哦，这是你实验室里的显示器。
让我自动创建这个布局给你。

这是使用 Hammerspoon 工具的另一种方法。
还有其他一些有趣的事情，比如可以检测您所在的 Wi-Fi 网络，这样就可以知道您所在的位置。
也许我在家里和在实验室的 Wi-Fi 网络名称不同，那么我可以做一些事情，比如当我到实验室时自动将扬声器静音，这样我就不会在实验室里尴尬地播放音乐了。
另一个很酷的例子是，我有一台 Mac 电脑，它有一个高级电源适配器，我的很多朋友的电脑看起来都和我的一样，他们的电源适配器看起来也和我的一样，有时候我会用他们的电源适配器，因为我把我的忘在家里了或者其他原因。
这个工具实际上可以使用三到四行代码做一些好玩的事情，比如在您意外拿了朋友的电源适配器并插到电脑上时，会弹出一个警告。
所以，从高层次上讲，这个工具允许您运行任意 Lua 代码，并将其绑定到菜单按钮或按键上，它与操作系统的大部分交互，以执行各种有趣的事情。
这就是 Hammerspoon，对此还有什么问题吗？好的，接下来讲另一个话题，和之前没有任何关系，就是启动和使用 Live USB。
您的计算机上的操作系统，比如 Windows 或 Mac OS 或其他您习惯的操作系统，实际上不是在开机时第一件运行的东西。
在操作系统加载之前，启动过程中会发生其他一些事情。
有些有趣的东西可以在这里配置。
您可能已经看到，当您打开计算机时，它会显示类似“按 F9 键配置 BIOS”或“按 F12 进入启动菜单”的消息。
特定的按键序列可能会因您的机器和具体配置而有所不同，但这是一个通用的模式，您可以在这里配置各种有趣的硬件相关的内容。
因此，这值得一试。
在这个启动菜单中，您可以让计算机从备用启动设备开始启动。
例如，默认情况下，我的笔记本电脑有一个固态硬盘，当它开机时会启动 Mac OS，但是我也可以插入一个安装有操作系统的 USB 闪存驱动器，然后在开机时告诉计算机从那个闪存驱动器而不是内置的固态硬盘开始启动。
这对于例如我破坏了操作系统安装，需要做一些事情，比如获取计算机上的数据，或者修复操作系统。
比如可能有些关键文件被删除了，或者我忘记了密码，需要去调整一些文件以重置它。
从一个Live USB启动计算机，从安装在闪存驱动器上的独立操作系统引导，可以让我做到这一点，就像启动我的操作系统，挂载在我当前机器上的硬盘一样。

我正在处理的任务,然后进行一些调整或者将数据复制出来。
因此，live USB非常有用，在课堂笔记中，我们提供了一个工具，可以帮助您轻松创建live USB。
有关启动过程或live USB的任何问题吗？好的，下一个话题是虚拟机、Vagrant、Docker、云和OpenStack。
去年我们整整开了一个讲座来探讨这个话题，今年我们将把它压缩到一分钟内。
在高层次上，虚拟机和类似的工具，如容器，可以让您在当前计算机系统内模拟整个计算机系统，例如，我正在运行Mac OS，但在我的Mac OS环境内，我可以模拟运行Ubuntu或其他操作系统的机器。
这是创建用于测试、开发或探索的隔离环境的好方法，例如运行潜在的恶意代码应该与我的当前环境隔离。
我认为程序员最常见的用例是使用虚拟机或容器创建开发环境。
我使用Mac OS，并在我的当前计算机上安装了一些服务、库等，但我可能想要在Debian机器上运行某些Web编程项目，并需要安装Postgres等数据库服务器，而不是在我的Mac OS机器上安装所有这些。
我可以为开发目的实例化这个新机器。
虚拟机是一个普遍的概念。
有许多程序可以被称为虚拟机超级监视程序，支持您在计算机上实现这个功能。
还有一些工具，让您可以脚本化这些超级监视程序，以便指定机器配置，如操作系统，您想要安装哪些软件包以及您想要安装哪些服务。
这是屏幕上的一个示例。
这是使用名为Vagrant的系统完成的。
在课程笔记中有链接，如果您感兴趣可以了解一下。
因此，在简短的纯文本文件中，我可以指定一个正在运行Debian的机器，它应该安装了Postgres、Redis和Python等软件。
然后，一旦我有了这个配置，
我只需要输入"vagrant up"，它会读取这个文件并根据这个配置实例化一个新的虚拟机。
然后，在我这样做后，我可以通过"vagrant ssh"来SSH进入这个虚拟机。
所以它不是在其他硬件上运行的远程机器，它只是在我的机器上模拟，但现在我在这里有一个像我现在拥有的Ubuntu盒子一样的盒子，让我们真正地惊奇一下，这里没有一堆的debian和我想要安装的所有东西，我可以在这个隔离的环境中进行开发，而不是在我的MacOS机器上安装所有这些垃圾。
现在有类似的工具，如Docker，它们在概念上类似，但是使用容器而不是虚拟机。
这是一个区别，我们现在不会详细讨论。
因此，你可以在自己的计算机上运行虚拟机，但你也可以在云上租用虚拟机，这是一个很好的获得即时访问的方式，比如你可能想要一个始终开着、始终连接到互联网并具有公共IP地址的计算机。
比如你想运行一个始终可用的Web服务器，或者你想运行一些其他的服务，比如一个slack机器人之类的东西。
在云上租用虚拟机是一个不错的选择，这些虚拟机对于低容量的机器和小型CPU和少量的磁盘空间来说都是很便宜的。
你可能想做的是获得一个非常强大的机器，比如有很多CPU核心或者有很多内存或者有大量的GPU，用于某些特定的目的，比如说你正在进行深度学习或者其他一些敏感的计算。
那么，这也是使用云上的虚拟机可以做的事情。
最后，你可以获得比你实际可以访问的机器更多的机器。
比如，如果我需要一千台机器，但只需要两分钟来完成一些非常并行的任务，那么我可以很容易地使用虚拟机来完成。
用于执行此操作的流行服务包括Amazon AWS或Google Cloud。
如果你是MIT CSAIL的成员，你也可以使用CSAIL OpenStack获得免费的虚拟机进行研究。
所以，关于虚拟机、Vagrant、Docker或其他类似的东西，有任何问题吗？问题是当我说我在运行Ubuntu时，实际上在这种情况下，我运行的是Debian，那么我是否像运行Ubuntu一样在我的电脑上安装了Ubuntu，还是发生了什么情况？基本上，当我键入“vagrant up”时，Vagrant为我做的是，因为我指定我想要Debian，它从互联网上下载了Debian，为这台新机器设置了一个磁盘映像，将Debian安装到该磁盘映像中，然后安装了这些程序。
所以，是的，这在我的电脑上。

但是，所有这些都只是在一个特定的文件中，这是一个磁盘映像。
然后，我正在模拟一台与我的电脑完全隔离的机器。
这是在我的电脑上作为一个进程运行的。
这回答了你的问题吗？还有其他关于虚拟机的问题吗？好的，下一个话题也会是一个简短的提及。
很多人是程序员，你们习惯使用像Vim这样的工具编写程序，或者使用其他你们熟悉的编辑器。
但是，另一件非常实用的东西是，可以为特定任务使用笔记本编程环境。
这是一种更加交互式的编程方式。
在屏幕上，我有一个演示。
这是一个叫做Jupyter笔记本，它可以用于编写Python程序。
我认为它们也支持其他一些语言。
基本上，这是一个很好的交互式编程方式。
通常，你们习惯在一个文件或一组文件中编写一个大程序，一旦完成，就可以运行整个程序。
但是，这让你更加灵活，并逐段运行代码。
例如，我可以将我的程序分成这些小段，这只是我写的一些随机代码。
然后，我可以说，“执行这个单元格”，然后按下特定的组合键来执行该单元格。
但是，我可以回去稍微修改我的程序，例如，我想要将它变成小写。
然后，我可以执行这个单元格，然后去评估这个东西，这样我就可以在Python环境中运行一些小段的代码。
这是一个很好的逐步构建程序的方式，而不是一次性编写所有代码。
这对特定的研究目的非常有用，例如，我认为很多人用它来进行机器学习工作。
对于笔记本编程环境的概念有任何问题吗？值得一看。
哦，问题是，“这看起来像是在线的，有没有Jupyter笔记本的离线版本？”实际上，这是在浏览器中运行的东西，但它是本地运行的……所以，我不知道你能否在屏幕上看到，因为它有点小，但在这里，我在自己的本地机器上运行一个Jupyter笔记本。
他们只是建立了它，让它在Web浏览器中运行。
话虽如此，也有在线Jupyter笔记本可供使用，其中Python内核实际上在一些远程机器上运行。
你可能会想这样做，例如，在我的笔记本电脑上，我没有一个高级的GPU，
在我的房间里，我有一台装有高级GPU的机器。
所以，当我做机器学习的工作时，我经常通过SSH登录到那台机器，运行Jupiter笔记本，然后在本地的Web浏览器中打开界面，以便访问那台运行在另一台机器上的强大GPU。
还有其他问题吗？好的。
今天我们要讨论的最后一件事是Github。
我们在版本控制讲座中已经提到了一点。
但是，Github是最受欢迎的开源软件开发平台之一。
它托管源代码和git存储库。
但是，他们还有其他工具来管理项目。
而且，像我们在这门课程中谈论过的许多工具一样，都托管在Github上。
例如，像我们刚刚谈论的Hammerspoon就是在Github上开发的。
在Github上开始为开源项目做贡献以帮助改进您每天使用的工具非常容易。
您可以通过两种主要方式为Github上的项目做出贡献。
让我们打开一些存储库。
我们实际上可以进入课程网站的Github存储库。
这是一个开源软件项目。
让我们放大一点。
因此，您可以通过问题和拉取请求这两种主要方式为Github上的项目做出贡献。
实际上，对于开发人员来说，一件非常有用的事情，对用户来说也相对轻松和简单，就是报告软件项目的问题。
比如说，您正在使用某个程序，遇到了一些错误...编写高质量的问题对开发人员非常有帮助，希望不会花费太多时间。
因此，您可以前往Github上找到项目，进入问题页面，然后单击新问题，并编写一些高质量的错误报告。
然后，希望开发人员会回复并为您修复问题。
例如，在这门课程中，一个学生指出了我们的讲座笔记中的问题，我说，好的，看起来是一个合理的问题。
让我们来解决它。
在这种情况下，我实际上询问了这个人，“他们只是想为我解决问题吗？”
因此，我想谈论另一件事：问题和拉取请求。
拉取请求是在Github上为项目做出贡献的第二种方法。
这涉及实际向项目贡献代码。
如果我们查看此项目的拉取请求，您会看到许多人提交了代码更改。
创建拉取请求的过程比提交问题要更复杂一些。
您不只是提交文本：您实际上要修改他们的源代码。
我们提供了一些解释该过程的指南链接。
但总体来说，您需要在Github上“分叉”存储库，然后将其下载到本地，这样就有了自己的本地副本。
然后，您可以开始工作，进行一些开发工作，修复错误或添加功能，最终将所做的更改发送回原始开发人员，称为“拉取请求”。
然后，通常情况下，项目维护者将与您反复回复，给您反馈您所提出的更改。
最终，一旦大家都满意，他们会合并您的更改，这些更改将对使用该项目的所有人都可用。
这就是如何在Github上为项目做出贡献，使软件对每个人都更好。
对于Github有任何问题吗？明白了，好的。
所以，今天的话题就到这里。
对于整个讲座有任何问题吗？很好，那么在结束之前，让我简单介绍一下明天的讲座：今天是我们认为有趣的所有主题，我们应该讨论的所有主题。
明天的讲座将是一个问答环节。
在今天的讲座之后，我们将通过电子邮件发送一个链接，您可以通过该链接提交问题供我们回答。
请务必填写，否则，我们明天将没有太多可谈的内容。
好的，希望明天在我们的问答讲座中见到您。
