# 用于机器学习项目的 GitOps

> 原文：<https://winder.ai/gitops-for-machine-learning-projects/>

不久前，开发人员使用笨重的控制台来提供基础设施和应用程序。不久之后，有人意识到通过脚本和 API 来自动化这样的过程更好。但是直到 [Hashicorp](https://www.hashicorp.com/) 表明 API 是不够的。他们的见解是声明基础设施的规范表示。然后，您可以将该声明与基础设施的实时视图进行协调。

2015-16 年，我们帮助 [WeaveWorks](https://www.weave.works/) 开发了他们的云监控平台。我们的目标是 Kubernetes，但对我们基于脚本的部署管道的粗糙感到遗憾。当时常见的模式是运行一个反复运行`kubectl apply`的脚本。

有人意识到这种模式，使用一个集中的真相来源来应用清单，与 Terraform 的模式是一样的。这不是基础设施。它甚至不是特定于应用程序的。但是它倾向于存在于版本控制中，并被部署到生产中。因此吉托普斯。

然后我们开始开发 [Flux](https://fluxcd.io/) ，这是出于对驻留在 Git 中的 Kubernetes 清单进行协调的需要。但是现在有其他同样有能力的持续部署工具，如 [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) 。

## GitOps 的定义是什么？

GitOps 有很多东西，但我将其归纳为两个关键原则。

### 1.宣布你整个企业的规范状态

通过声明，我的意思是写下并在适当的可见性级别公开。

是的，这可能是定义您的 web 应用程序的 Kubernetes 清单，您将向您公司的开发人员公开该清单。但也可能是代表你的招聘政策的降价文件，你会将这些文件公之于众。

规范状态意味着我们正在谈论的任何事物的最近达成一致的目标状态。

同样，这可能是最近构建的二进制文件或当前的公司战略。

您的部署管道的执行，或者实际上您的业务策略，应该将系统推向声明的规范状态。

### 2.版本控制

版本控制是额外的，但是它非常强大。想象一下穿越时空去寻找和重新创建一个以前的商业策略或应用程序。

但这更多的是血统和出处的特征，我想把重点放在声明上。

## 现代云原生应用中的 GitOps 是什么样子的？

任何具有部署阶段的应用程序都依赖于一系列复杂的事件。要了解为什么 GitOps 是有益的，如果您将这些依赖关系显式化会有所帮助。

下图展示了一个现代应用程序的简化依赖图。盒子代表应用程序部署中的一个阶段。带有齿轮的方框表示您应该自动化的过程的一部分，就像构建过程一样。带有软盘的盒子代表生成和存储工件的阶段。粉红色的方框代表需要声明的流程部分。

![A dependency graph depicting GitOps in a modern web application](img/2b6ed96c01fcb31d58fa2b2ea22ba7c6.png)

您可以看到，在这个过程中有三种主要的声明类型。应用程序代码声明了业务逻辑。环境声明了应用程序将在其中运行的上下文和运行时。部署清单声明应用程序应该如何以及何时运行。

这个例子代表了一个现代的 web 应用程序。但是您可以对业务的任何其他部分进行依赖分析。

## GitOps 有什么好处？

GitOps 对企业和工程师都有一系列好处。GitOps 提高了生产率，改善了开发人员的体验，增强了稳定性，并促进了标准化。

### 提高生产率

如果您的应用程序(或业务)是声明的和自动化的，那么业务中的任何人都很容易做出小的改变。通过自动化过程执行的测试确保这些变化不会损害应用程序。自动化部署将变更部署到生产环境中(或任何地方)。

这使得以最小的风险部署改进、修复或废弃变得容易和快速。这本身就提高了生产率，因为它消除了障碍。

### 更好的开发者体验

你付钱给开发者去开发。他们不喜欢上下文切换。

一个现代的云原生堆栈是如此之高，以至于从顶部到底部的切换远不止是一个上下文切换。这就像在电梯中自由落体，并试图在正确的微秒走出电梯，通过 13 楼的开口逃离。[机器学习](https://winder.ai/services/machine-learning/machine-learning-consulting/)一般在此之上叠加，让问题变得更糟。

GitOps 有助于向栈中一部分的开发人员保证其他部分是健壮的。因为开发人员可以看到应用程序的预期状态。自动化测试和部署那些最小化对栈的另一部分的影响的变更。

开发人员可以自由地在他们的堆栈上进行开发，而没有上下文切换的负担。

### 提高稳定性和可靠性

建筑工人按照建筑师的计划工作。建筑师遵守土木工程师制定的规则。土木工程师依赖于钢铁制造商的公差。与建筑业一样，GitOps 允许开发人员专注于他们擅长的挑战。

应用工程师不需要关注部署。DevOps 工程师不需要关注安全性。诸如此类。这导致全面的改进，并产生安全、稳定、可靠的应用程序。

代码带来的可观察性也有助于可靠性。代码审查和自动化测试可以在部署之前发现错误。

### 增强一致性和标准化

让内容可见可以促进开放性、反馈和最佳实践。没有什么比存储库中的代码更明显的了。开源软件是这一理想的证明。

开源软件的一个被低估的特性是开发者通过查看代码进行自我调试的能力。我曾无数次不得不自己做这件事。通过提供对代码库的自由访问，您可以将这一原则应用到您的组织中。当然，你要考虑是否要限制访问。但是您应该从公共访问开始，然后往回工作，而不是反过来。

开发人员更愿意找到并修复他们自己的 bug，而不是提交{在此插入票务系统}罚单并等待 2-5 天。

排班是终极阻击者；通过开放你的代码库使它变得没有必要。

GitOps 的另一个常见副作用是它促进了模式和共享。如果开发人员可以看到其他人的代码，当他们需要做类似的事情时，他们可以复制和粘贴。复制和粘贴会引起注意，并转化为新的共享项目，使整个公司受益。

有时它们会变成产品。我最近采用了[树状](https://www.dendron.so/)。这始于 AWS，当时开发人员认识到共享知识库的必要性。

## 如何将 GitOps 用于机器学习应用

但是在人工智能项目中使用它时，GitOps 有什么不同呢？

首先，我想远离人工智能这个术语，尽管我(不情愿地)使用它。我所说的人工智能是指任何试图自动化决策或策略的项目。我不是特指机器学习，虽然我会用[机器学习](https://winder.ai/services/machine-learning/)作为例子。我指的是任何东西，像[强化学习](https://winder.ai/services/reinforcement-learning/)，符号方法，数据分析，分析学，[数据科学](https://winder.ai/services/data-science/)，任何东西。

无论你在开发什么应用，GitOps 都能帮上忙。只有声明发生了变化。

> 请记住，您的声明可能会根据项目的类型而变化。查看我在本文末尾的提示，了解如何确定您需要申报的内容。

### 用于机器学习的 GitOps

对于一个机器学习项目，我遵循与[软件工程 GitOps 示例](#gitops-software-engineering)中相同的工作流程，通过依赖分析来分解声明性元素。

下图显示了一个示例机器学习应用程序的依赖关系图。和以前一样，有齿轮的盒子是自动装置，软盘存储文物，粉红色的盒子是你需要申报的东西。

![A dependency graph depicting GitOps in a machine learning application](img/349f29f462d3ef0647e1cae99b3426b5.png)

如您所见，代码、清单和环境是相同的。但是在机器学习项目中，你还需要声明数据和超参数。另一个区别是清单现在依赖于代码和模型工件。

#### 模型工件部署模式

事实上，这里有两种可能的模式。在这个例子中，我将代码工件从模型工件中分离出来；这种模式在塞尔顿和 T2 的民间广为流传。

我并不热衷于此，因为它并没有定义一个众所周知的工件。很难知道在任何时候运行的代码模型版本的组合，因为唯一的链接在清单中。是的，有可能回头看代码。但是这并不能保证代码模型组合实际上正在运行。有人可能已经手动应用了模型的新版本。或者清单可能没有成功应用。

相反，我更喜欢将模型烘焙到代码工件中，这会改变依赖图。在这种配置中，您可以保证出处。无论何时何地，代码工件都拥有相同的代码模型组合。你可以在下图中看到这种结构。

![A dependency graph depicting GitOps in a machine learning application with the model artifacts baked in](img/0b34a7779d044b465148de426a0b3f51.png)

正如你所看到的，它使低级代码工件变得复杂，因为两个独立的过程依赖于它。这意味着可能会意外地生成与训练运行不完全相同的代码。

为了防止这种情况发生，我建议添加另一个代码工件构建步骤。首先构建一个用于实验的工件，比如一个基本容器。然后构建另一个工件，在上面添加一个包含训练模型的层。这样就可以保证实验代码和最终容器中的代码是相同的。

#### 公共机器学习代码

对于机器学习示例，依赖图中还有最后一个有趣的地方。公共代码通常同时存在于训练代码和服务代码中。例如，您需要确保以完全相同的方式预处理数据。

在传统的软件工程中，发布不带任何辅助代码的代码是很常见的。例如，您不会打包测试或 CI 代码。但是在机器学习中，血统和可重复性是最重要的，所有代码都很重要。尤其是训练过程中使用的代码。捆绑整个回购的好处大于不捆绑的风险和缺点。

#### 声明性数据

声明数据超出了本文的范围。但是常见的方法包括:

*   在版本控制系统中声明数据，如 Pachyderm 和 Databricks Delta Lake
*   在数据湖中存储唯一可识别的、压缩的、不可变的数据集，如 S3
*   数据仓库中的不可变查询，如 BigQuery。

#### 声明性超参数

声明超参数并不难。它们往往是明文，很容易和代码一起存储在 Git 中。

## GitOps 对 AI 开发有什么好处？

GitOps 已经在帮助 AI 开发了，因为它有助于软件开发。AI 是 80%工程，所以之前的好处继续适用。

但是使用 GitOps 还有一些额外的好处，这些好处更多的是针对人工智能的。

### 管理

治理是使用控制、流程和审计来证明应用程序遵循业务策略。当然，除了 GitOps 之外还有很多其他的东西。但是让你的整个人工智能开发成为声明性的，是让你的应用变得可观察和明显的一步。如果工具可以看到应用程序的声明，就很容易审核它是否符合。

当审计人员执行调查时，提供应用程序在某个时间点的状态的确定的事实来源是非常有价值的。

### 防止开发人员锁定

人工智能技术栈，从代码，通过 DevOps 工具，到人工智能框架，再到部署，非常高。这使得你公司的每个人都不太可能是所有方面的专家。当有人离开公司时，这可能是一个真正的问题，因为知识的严重流失。

最坏的情况是开发者什么都没声明。在这种情况下，应用程序就像死了一样。你不能复制任何东西，更不用说继续发展了。我最喜欢的例子之一是 Andrew Yurisich 的《如何编写不可维护的代码》。快看，太搞笑了！

但是如果应用程序的整个状态都被声明了，那么尽管不容易，但它使得控制应用程序、了解它如何工作并继续前进成为可能。

### 可重复的实验

工业界(尤其是学术界)面临的一个关键挑战是实验的可重复性。很难让机器学习实验返回可重复的结果。如果项目不是声明性的，那就更难了。想象一下，如果作者提供的都是`Python 3.7`或者类似的东西，那么不得不尝试拼凑一个环境。

GitOps 在这方面很有帮助，因为根据定义，应用程序的状态以及实际上的实验都是声明的。复制应该像点击按钮一样简单。

### 终极实验平台

可重复的实验简化了对超参数或代码的小改动。在推动变更后，自动化流程开始运作并运行新的实验。

重复这个过程几次，以测试一些不同的东西，然后把你的脚。午饭后回来，比较你的实验跟踪解决方案中的实验。终极机器学习生产力。

### 重新测试以前的模型

回溯测试是在历史数据上证明模型性能的常用策略。但是用新数据来检验旧模型也是有用的，尤其是当数据表现出季节性的时候。

比如，想象一下，你需要为今年的疯狂重新开发一个黑色星期五模型。您可以:

a)参考去年的声明状态，并将其与当前模型进行比较，b)找出旧的工件，并根据该时期的数据对其进行测试。

本质上，你可以对去年的模型进行回溯测试和前瞻测试，看看它的表现到底有多好。

### 在基础设施之间移动

从本地开发环境转移到生产或大规模培训平台是很困难的。以至于一些框架积极推动远程开发，比如 Spark。但是在本地工作时，开发人员的体验和反馈回路要好得多。

如果您以声明方式工作，那么在运行时注入特定于环境的配置选项是很容易的。例如，您可以在本地使用包含一小部分数据的本地目录，而在远程使用完整数据集的 s3 路径。

## 从哪里开始？

希望我已经让你相信，人工智能的 GitOps 是一件值得为之奋斗的事情。所以现在你在问“下一步是什么？”。

首先，很可能你已经有了。很可能你的代码库在 Git 中。有人可能已经在贵公司流程的某个地方使用 GitOps 了。看看是否有人已经捷足先登了！

之后，请遵循以下步骤:

1.  绘制并遵循应用程序的依赖关系图。让它具有宣示性，一路向下。
2.  自动化执行阶段。
3.  开发程序和工具来加速开发和实验。
4.  开发可观察性工具来帮助治理和可观察性。

## GitOps 的未来如何？

吉托普斯会留在这里。

声明性的好处超过了极少的反对意见。这几乎没有任何额外的工作，并促进整个企业的清洁。有些人可能对血统有不同的看法。如果没有可用的代码工具，有些人甚至会质疑声明。

但是未来会保留 GitOps。

这是因为如果你透过一个足够小的窗口看，状态是静止的。如果没有一致同意的真相来源，团队就无法合作。

然而，未来会带来变化。无代码和点击式界面很有用。所以我认为无代码解决方案和 GitOps 的融合是不可避免的。至少在面向开发人员的应用程序中是这样。

这就产生了 [SlackOps](https://cto.ai/blog/cto-ai-launches-slackops-remote-first-devops/) 或 [ClickOps](https://www.lastweekinaws.com/blog/clickops/) 的想法。这些是 GitOps 后端的用户界面。

我甚至认为 MLOps 最终将汇聚成为 GitOps 的用户界面，尽管它侧重于人工智能应用。

重点是会有很多面向用户的界面，但是所有的时尚都会导致 GitOps。

## 常见问题

[GitOps 的定义是什么？](#what-is-the-definition-of-gitops)

GitOps is the act of being so declarative that you can operate your application via Git.

[GitOps 在机器学习方面有何不同？](#how-is-gitops-different-in-machine-learning)

Machine learning includes extra dependencies like training data and model weights. You must declare these as well as standard software engineering assets like deployment manifests.

[GitOps vs. DevOps？](#gitops-vs.-devops)

DevOps practices empower teams to take responsibility of their applications, end-to-end. GitOps is a methodology that helps make operational deployments more resilient.

有哪些 GitOps 工具可用？

Popular tools for Kubernetes deployments include ArgoCD and Flux.

[gitop 有什么好处？](#what-are-the-benefits-of-gitops)

GitOps improves productivity, resiliency, the developer experience, unification, experimentation, testing, and governance maturity.