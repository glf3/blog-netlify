---
title: arts-week-01
date: 2018-11-18 16:58:57
---

# ARTS Week 1
积极响应[@耗子叔](https://weibo.com/haoel)的号召，加入了ARTS的大家庭。

　　**ARTS**，即：每周完成一个ARTS：每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）。

　　因为感觉工作两年多来自己的技术栈，接触到的技术、视野都有一点狭窄。所以我希望通过和大家一起参加ARTS，能够认识一些新的小伙伴，适当地跳出舒适区，扩展自己的技术视野，让自己的心态更加包容与开放，我觉得这是ARTS对我的意义所在。这是第一周，希望自己能够坚持下去咯... ^_^<br>

##   LeetCode
LeetCode 140 [Word Break II](https://leetcode.com/problems/word-break-ii/)<br>
**题意**：
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。<br>
说明：<br>
分隔时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。<br>
**示例1：**<br>
**输入：**<br>
s = "catsanddog"<br>
wordDict = ["cat", "cats", "and", "sand", "dog"]<br>
**输出：**<br>
[<br>
  "cats and dog",<br>
  "cat sand dog"<br>]
<br>

**分析：**<br>
我最先想到的解法是暴力dfs<br>
1、从(0~len（单词长度）开始枚举切割的位置，把单词分为前缀prefix和后缀postfix两部分，如果prefix在wordDict里，则开始递归下去，如果不在则继续往后遍历。)<br>
２、递归结束的条件：当后缀单词为""(空字符串)时，则代表找到了一种切割方法。有一种特殊的情况需要判断，即当前的整个单词就存在wordDict里面，那这个时候，我们也可以当做结束条件退出。<br>
　　当时提交上去的时候TLE(超时)了，因为每次计算的时候，会有很多重复的计算，比如string="aaabbbbc"和dict=["a","bb","bbbb"]这种情况，会存在重复计算了很多子串的情况。<br>
　　实际上，对于确定的字符串string="aaabbbbc"，dict＝["a","bb","bbbb"]。当前缀等于"aa",子串1为substring ="abbbb"，子串2为substring="abbbbc"这种情况而言，子串"abbbb"可以拼成单词的情况是确定的，即：["a","bb","bb"]和["a","bbbb"]两种。那么当递归的时候遇到相同子串时候，我们可以把这个保存下来，不需要重复计算，这样能节省很多时间，即记忆化搜索。

**具体代码：**
```
    class Solution {
        Set<String> mWordCache = new HashSet<String>();
        Map<String, List<String>> mAnswerCache = new HashMap<String, List<String>>();
 
        private void init(String s, List<String> wordDict) {
            mWordCache.clear();
            mAnswerCache.clear();
            for(String word : wordDict) {
                mWordCache.add(word);
            }
        }

        private List<String> dfs(final String word) {
            /** 如果已经计算过，则直接返回结果 */
            if (mAnswerCache.containsKey(word)) {
                return mAnswerCache.get(word);
            }
            List<String> sequences = new ArrayList<String>();
            /** 递归的退出条件 */
            if ("".equals(word)) {
                return sequences;
            }
            for(int i = 0; i <= word.length()-1; i++) {
                String prefixWord = getPrefixWord(i, word);
                String postfixWord =  getPostfixWord(i, word);
                if (mWordCache.contains(prefixWord)) {
                    List<String> backtrackingResult = dfs(postfixWord);
                    for (String singleWord : backtrackingResult) {
                        StringBuilder sb = new StringBuilder();
                        sb.append(prefixWord + " " + singleWord);
                        sequences.add(sb.toString());
                    }
                } else if(i == 0 && mWordCache.contains(postfixWord)) {
                    sequences.add(postfixWord);
                    dfs("");
                }
            }
            mAnswerCache.put(word, sequences);
            return sequences;
        }

        /** 获取前缀 */
        private String getPrefixWord(int end, String word) {
            String result = "";
            for(int i = 0; i < end; i++) {
                result += word.charAt(i);
            }
            return result;
        }

        /** 获取后缀 */
        private String getPostfixWord(int start, String word) {
            return word.substring(start);
        }

        public List<String> solution(String s, List<String> wordDict) {
            init(s, wordDict);
            List<String> sequences = dfs(s);
            return sequences;
        }

        public List<String> wordBreak(String s, List<String> wordDict) {
            return solution(s,  wordDict);
        }
    }
```

---
<br>
## Review
本周翻译一篇Medium.com上的文章[《How not to be afraid of Git anymore》](https://medium.freecodecamp.org/how-not-to-be-afraid-of-git-anymore-fe1da7415286)<br>
知其然，也要知其所以然。<br>
![avatar](https://cdn-images-1.medium.com/max/800/1*0o9GZUzXiNnI4poEvxvy8g.png)<br>
### 什么是Git？<br>
一个版本控制系统<br>
　　好了好了，首先说明，也许我在这方面给不了你们多少帮助。这是一些观点：当项目太庞大，有着太多的开发者的时候，几乎不可能去追踪每一个人什么时候做了什么事情，提交了什么代码。比如是否有人提交了一些代码导致整个系统不可用了？你怎么发现是哪一个人提交的代码导致了整个系统奔溃？怎么样才能将代码回滚？建立一个没有故障的桃花源。

　　让我们更进一步说话 — 但一个没有那么多开发者，仅仅是一个很小的项目，你是这个项目的创建者，维护者，还有贡献者：你为这个项目提交了一个新的功能，这个功能后来你发现引入了一些bug，但是你不记得是哪一个提交导致了这一个bug。这是一个问题吗？

　　解决这些问题的方法就是使用版本控制系统！用版本控制系统来提交你的代码的话，你就知道你做了哪些提交，这些提交是用来做什么的，当然，从项目一开始就使用。

　　现在，我邀请你停止把git当做一个不能够打开的潘多拉魔盒，而是打开这个它，你会发现有一大批财富正在等着你。弄明白Git是怎么工作的，当你工作时就不会再出现什么岔子。一旦你通过使用Git，我保证你能够避免上面所说的一些愚蠢的事情。这正是版本控制能够避免的。

### 怎么使用Git？<br>
　　我假设你知道一些Git的基本命令或者是至少用过Git一两次，如果没有的话，这里有一些关于Git基本的概念，它们可以帮助你开始学习使用Git。<br>

目录：一个存放东西的地方。在Git中，这就是存放代码的地方。<br>
head：指向你最近你提交代码的地方<br>
add：一个操作，这个操作能够使得Git去追踪最近修改的文件。
commit：一个操作，这个操作可以保存当前的状态。
remote：不是本地的仓库，可能是一个在云端上的远端仓库。不需要从你的电脑拷贝代码，从而可以帮助其他人可以简单的进行代码协作。在云端有一个代码备份，可以防止防止本地代码出现故障丢失代码的情况。<br>
pull：获取远端仓库的代码。<br>
push：将代码提交到远端仓库。<br>
merge：合并两个不同版本的代码。<br>
status：展示当前本地仓库的状态

### Git在哪？<br>
介绍一个神奇的目录:.git/<br>
在每一个使用Git的目录下，你会看到以下结构：
>$ tree .git/<br>
.git/<br>
├── HEAD<br>
├── config<br>
├── description<br>
├── hooks<br>
│   ├── applypatch-msg.sample<br>
│   ├── commit-msg.sample<br>
│   ├── post-update.sample<br>
│   ├── pre-applypatch.sample<br>
│   ├── pre-commit.sample<br>
│   ├── pre-push.sample<br>
│   ├── pre-rebase.sample<br>
│   ├── pre-receive.sample<br>
│   ├── prepare-commit-msg.sample<br>
│   └── update.sample<br>
├── info<br>
│   └── exclude<br>
├── objects<br>
│   ├── info<br>
│   └── pack<br>
└── refs<br>
    ├── heads<br>
    └── tags<br>
8 directories, 14 files<br>
<br>

　　这就是Git管理和控制你整个项目的方式。我们接下来会一个一个地分析一些重要的模块。<br>
Git由三个部分组成：对象存储区，索引，工作目录。

#### 对象存储区<br>
　　这其实是Git保存自己产生的对象的地方。你在项目中 **add** 的每一个文件，Git都会产生一个唯一的哈希值。举个例子，如果你创建了一个 **helloworld** 文件并且执行 **git add hello world** 操作(那么就相当于告诉Git把这个helloworld文件放入Git的对象存储区)，我得到了这样的结果。
>$ tree .git/<br>
.git/<br>
├── HEAD<br>
├── config<br>
├── description<br>
├── hooks<br>
│   ├── applypatch-msg.sample<br>
│   ├── commit-msg.sample<br>
│   ├── post-update.sample<br>
│   ├── pre-applypatch.sample<br>
│   ├── pre-commit.sample<br>
│   ├── pre-push.sample<br>
│   ├── pre-rebase.sample<br>
│   ├── pre-receive.sample<br>
│   ├── prepare-commit-msg.sample<br>
│   └── update.sample<br>
├── index<br>
├── info<br>
│   └── exclude<br>
├── objects<br>
│   ├── a0<br>
│   │   └── 423896973644771497bdc03eb99d5281615b51<br>
│   ├── info<br>
│   └── pack<br>
└── refs<br>
    ├── heads<br>
    └── tags<br>
<br>
9 directories, 16 files<br>

　　一个新的对象生成了！对这个Git操作的底层原理感兴趣的话，Git提供了对文件生成哈希值的命令。
>$ git hash-object helloworld<br>
a0423896973644771497bdc03eb99d5281615b51

　　对的，这个和在对象存储区中看到的一模一样。为什么一个哈希值需要拆分成两份？这是为了更快的查找。

　　然后，Git产生了一个对象，把一个文件压缩并且存储到这个对象里面，并把它对应着上面的哈希值一一对应。你看，你也可以看到储存的对象的内容。
>$ git cat-file a0423896973644771497bdc03eb99d5281615b51 -p<br>
hello world!

　　这就是Git底层的一些操作。你基本上不会用到 **cat-file**这个操作。你一般只会用 **add**，然后让Git去处理剩下的事情。<br>
这就是我们的第一个Git命令，已经介绍了怎么使用它和它工作的原理了。<br>
>git add creates a hash, compresses the file and adds the compressed object to the object store.

#### 工作目录
　　从名字上可以看出，这是我们工作的地方。所有你创建，修改代码都在是工作目录里面完成的。我创建了一个文件， **byeworld** ，然后执行 **git status**：

>$ git status<br>
On branch master<br>
No commits yet<br>
Changes to be committed:<br>
  (use "git rm --cached <file>..." to unstage)<br>
new file:   helloworld<br>
Untracked files:<br>
  (use "git add <file>..." to include in what will be committed)<br>
byeworld<br>

有一些在工作目录中没有跟踪的文件，我们也没有要求Git去管理它们。

当我们没有操作工作目录的时候，我们会得到下面的信息：
>$ git status<br>
On branch master<br>
nothing to commit, working tree clean

这个我十分肯定你已经理解了，把“分支信息”和“现在提交”的文案忽略掉，那么关键的就是：当前工作目录是干净的，没有需要提交的。
#### 索引
这是Git的核心。或者也叫作暂存区。索引存储的是文件与Git产生在对象存储区的对象之间的映射。这时候 **commit** 工作的地方。最好的方式就是索引是怎么工作的。<br>

让我们使用** git commit** 提交我们的这个文件 **hello world**
>$ git commit -m "Add helloworld"<br>
[master (root-commit) a39b9fd] Add helloworld<br>
 1 file changed, 1 insertion(+)<br>
 create mode 100644 helloworld<br>

返回到我们的Tree结构：
>$ tree .git/<br>
.git/<br>
├── COMMIT_EDITMSG<br>
├── HEAD<br>
├── config<br>
├── description<br>
├── hooks<br>
│   ├── applypatch-msg.sample<br>
│   ├── commit-msg.sample<br>
│   ├── post-update.sample<br>
│   ├── pre-applypatch.sample<br>
│   ├── pre-commit.sample<br>
│   ├── pre-push.sample<br>
│   ├── pre-rebase.sample<br>
│   ├── pre-receive.sample<br>
│   ├── prepare-commit-msg.sample<br>
│   └── update.sample<br>
├── index<br>
├── info<br>
│   └── exclude<br>
├── logs<br>
│   ├── HEAD<br>
│   └── refs<br>
│       └── heads<br>
│           └── master<br>
├── objects<br>
│   ├── a0<br>
│   │   └── 423896973644771497bdc03eb99d5281615b51<br>
│   ├── a3<br>
│   │   └── 9b9fdd624c35eee08a36077f411e009da68c2f<br>
│   ├── fb<br>
│   │   └── 26ca0289762a454db2ef783c322fedfc566d38<br>
│   ├── info<br>
│   └── pack<br>
└── refs<br>
    ├── heads<br>
    │   └── master<br>
    └── tags<br>
14 directories, 22 files<br>

有趣的事情发生了，我们有了在对象暂存区有了两个新的对象，还有一些我们不明白的logs和refs。回到我们的老朋友，**cat-file**：
>$git cat-file a39b9fdd624c35eee08a36077f411e009da68c2f -p<br>
tree fb26ca0289762a454db2ef783c322fedfc566d38<br>
author = <=> 1537700068 +0100<br>
committer = <=> 1537700068 +0100<br>
Add helloworld<br>

>git cat-file fb26ca0289762a454db2ef783c322fedfc566d38 -p<br>
100644 blob a0423896973644771497bdc03eb99d5281615b51 helloworld<br>

　　第一个对象代表的是这一次提交的元数据：谁做了什么，为什么，还有一个指向 **tree** 的指针(树型结构体)。第二个对象代表的是一颗真实的 **tree** 。如果你理解了Unix文件系统，你就会知道这是什么。

　　**tree** 在Git中就好比是Git文件系统。在任意一次提交里面，每一个东西都可以看做是一个tree(目录)或者是一个(blob)文件，Git保存了tree的信息，这就像是：这就是为什么工作目录需要查看这一个节点的原因。注意树节点指向一个每一个对象存储区里的对象都包含的唯一的哈希值。<br>
　　现在是时候讨论一下**branches**(分支)了！。我们的第一个提交加了一些东西去.git/目录下面。我们现在把精力放在 **.git/refs/heads/master**：
>$ cat .git/refs/heads/master<br>
a39b9fdd624c35eee08a36077f411e009da68c2f

这是你关于分支需要了解的东西：
>分支在Git中代表的是一个轻量级的可移动的指针，它指向了一个对应的commits提交。在Git中，默认的分支名就叫master。

　　啊，什么情况？我更倾向于把分支看作是代码的一份拷贝。你想添加一些新功能，但是不想破坏原有的代码结构与功能。当你想找一个比提交记录更加明显的区分边界，于是就有了分支这个概念。 **master**代表的是默认的分支，也被看做是实际上的生产分支。所以，它创造了上面这个文件。你也可以明确猜到的是这个文件的内容指向了我们的第一次提交。

让我们更进步的深究下去。我新建一个新的分支。
>$git branch the-ending<br>
git branch<br>
 *master<br>
 the-ending

我们可以看到，有了一个新分支。正如你所猜到的，这个新的分支得添加到 **.git/refs/heads/** 中去，当没有额外提交的时候。它也应该指向我们的第一次提交，就与 **master**分支中的一样。
>$ cat .git/refs/heads/the-ending<br>
a39b9fdd624c35eee08a36077f411e009da68c2f

没错！还记得 **byeworld** 吗？那个文件是没有追踪的，所以无论你切换到哪一个分支，那一个文件都永远在那里。所以我 **checkout**（切换）了分支。
>$ git checkout the-ending<br>
Switched to branch 'the-ending'<br>

>git branch<br>
  master<br>
*the-ending

　　现在，理解了原理，Git会把所有工作目录的内容变为我们指向的commit(提交)。对现在这种情况而言，它们看起来一样。

我现在对 **byeworld** 执行 **add** 和　**commit**操作。

你认为对象暂存区会有什么样的变化？

你认为refs/heads会有什么样的变化？

当你继续往下看之前，想一想这两个问题。

>$ tree .git/<br>
.git/<br>
├── COMMIT_EDITMSG<br>
├── HEAD<br>
├── config<br>
├── description<br>
├── hooks<br>
│   ├── applypatch-msg.sample<br>
│   ├── commit-msg.sample<br>
│   ├── post-update.sample<br>
│   ├── pre-applypatch.sample<br>
│   ├── pre-commit.sample<br>
│   ├── pre-push.sample<br>
│   ├── pre-rebase.sample<br>
│   ├── pre-receive.sample<br>
│   ├── prepare-commit-msg.sample<br>
│   └── update.sample<br>
├── index<br>
├── info<br>
│   └── exclude<br>
├── logs<br>
│   ├── HEAD<br>
│   └── refs<br>
│       └── heads<br>
│           ├── master<br>
│           └── the-ending<br>
├── objects<br>
│   ├── 0b<br>
│   │   └── 17be9dbc34c5a5fbb0b94d57680968efd035ca<br>
│   ├── a0<br>
│   │   └── 423896973644771497bdc03eb99d5281615b51<br>
│   ├── a3<br>
│   │   └── 9b9fdd624c35eee08a36077f411e009da68c2f<br>
│   ├── b3<br>
│   │   └── 00387d818adbbd6e7cc14945fdf4c895de6376<br>
│   ├── d1<br>
│   │   └── 8affe001488123b496ceb34d8b13b120ab4cb6<br>
│   ├── fb<br>
│   │   └── 26ca0289762a454db2ef783c322fedfc566d38<br>
│   ├── info<br>
│   └── pack<br>
└── refs<br>
    ├── heads<br>
    │   ├── master<br>
    │   └── the-ending<br>
    └── tags<br>
17 directories, 27 files<br>

３个新的对象，１个对象是add的文件，还有２个对象是commit产生的。弄明白了吗？你认为这几个文件都包含什么呢？

 >提交元数据<br>
 >添加的对象的内容<br>
 >树型结构的信息与描述

最后一个问题是，这个提交的元数据如何和前面的提交元数据联系起来。好了，让我们来使用 **cat-file** 操作。

>$ git cat-file 0b17be9dbc34c5a5fbb0b94d57680968efd035ca -p<br>
100644 blob d18affe001488123b496ceb34d8b13b120ab4cb6 byeworld<br>
100644 blob a0423896973644771497bdc03eb99d5281615b51 helloworld<br>

>git cat-file b300387d818adbbd6e7cc14945fdf4c895de6376 -p<br>
tree 0b17be9dbc34c5a5fbb0b94d57680968efd035ca<br>
(parent a39b9fdd624c35eee08a36077f411e009da68c2f)<br>
author = <=> 1537770989 +0100<br>
committer = <=> 1537770989 +0100<br>

>add byeworld<br>

>git cat-file d18affe001488123b496ceb34d8b13b120ab4cb6 -p<br>
Bye world!<br>

>cat .git/refs/heads/the-ending<br>
(b300387d818adbbd6e7cc14945fdf4c895de6376)

你看到了用括号括起来的东西了吗？有了指向父亲节点的指针！对，这就是你发现的，一个链表，这个链表把所有的提交联系起来了。

你看到分支的实现了吗？它指向一个提交，一个切分支之后的最近的提交！当然，master分支现在应该还指向着helloworld 这个提交，对吗？

>$ cat .git/refs/heads/master<br>
a39b9fdd624c35eee08a36077f411e009da68c2f

好了，我们分析了这么多，现在我来做一个总结。
### 总结
Git的功能是靠对象来实现的，一个你要求Git去跟踪的压缩了的文件。

每一个对象有一个唯一的哈希值(经过文件内容产生的唯一哈希值)

每一次你使用 **add** 添加文件的时候，Git都会产生一个新的对象放到对象存储区域。这就是为什么在Git中无法处理超大型文件的问题，因为每次使用 git add 的时候存储的都是整个文件而不是diff(差异)，这是与主流不符的设计。

每一次提交产生两个对象：<br>
１、The tree(树型结构)：一个ID对应一棵树，这个和Unix的文件系统很像：它指向的是其他的trees（目录）或者blob(文件)：tree描述了整个基于当时对象的目录的结构。blobs代表的基于当时由 **add** 这个命令产生的对象。<br>
２、(The commit metadata)提交的元数据：一个ID对应一个提交，谁做了这个提交。一个树代表了这个提交，提交信息，还有父提交。就像一个链表一样把整个提交串联起来了。

分支是指向提交元数据的指针，所有分支都存在.git/refs/heads里面。

这就是所有关于Git的解读！在下一章，我们会分析一些让人们非常疑惑的Git操作是怎么工作的。
如：
>reset, merge, pull, push, fetch and how they modify the internal structure in .git/.

---
<br>
## Tips:
本周工作中遇到的一个移动客户端上的问题。<br>
　　当时接入小米账号SDK的时候忘记了在AndroidManitest获取账户相关的权限。在一些机型上，当Android版本 >= M的时候，有一个线程发生了CRASH，当时我把获取账号信息的具体操作没有放在主线程上面，所以测试并没有发现这个问题，后来上线之后才发现的这一个问题。<br>
　　Review代码的时候发现现在的项目在权限管理这一方面写的有一点复杂，于是我把现在需要动态申请权限的地方，将Android原生的onRequestPermissionResult的回调去掉，改成一个开源的库[RxPermission](https://github.com/tbruyelle/RxPermissions)，RxPermission是RxJava实现的异步申请权限的工具，支持申请一个单独的权限或者是一组权限，用起来非常方便与快捷。整体使用之后来说，代码量变少，语义也变得清晰，之前加很多逻辑都可以去掉了。

---
<br>
## Share:
Java中的动态代理[链接](https://www.cnblogs.com/gonjan-blog/p/6685611.html)