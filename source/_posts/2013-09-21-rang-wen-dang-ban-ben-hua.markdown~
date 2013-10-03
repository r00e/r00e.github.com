---
layout: post
title: "Docbook, 让文档版本化"
date: 2013-09-21 20:36
comments: true
categories: 
---
由于项目中客户的一些原因，我们不能直接接触产品环境的服务器。导致我们每次部署产品代码前，需要给客户的IT团队准备一份部署文档，然后他们遵循这份文档来部署产品。

最开始的时候，我们是用word来写这份文档的。方便是方便，但是当时间长了以后，文件数越来越多，每次写这个文档也不是固定的人，有时候需要复查一下很久前写的东西，就不那么方便了。所以很期望能把这个部署文档也纳入版本控制当中，这样就可以像代码一样，不管是谁写完文档，check in到repository里，以后想要查找、对比都方便很多。

但是word文档本身并不能直接纳入到版本控制中，需要check in的是纯文本。我们还想提供给客户的文档有一定的格式，所以直接发送纯文本的方式也被否定了。

这个时候就是我们的主角Docbook登场的时候了！Docbook可以把符合自己格式的XML文件转变成pdf，我们可以把pdf作为发送给客户的最终文档。下面就让我们看看，如何使用Docbook来实现文档的版本化吧！

我们这里以**Windows**环境为例。

依据[链接1](http://www.crifan.com/files/doc/docbook/docbook_dev_note/release/htmls/pure_win_docbook_dev_env.html)的步骤，可以很方便的搭建起来Docbook环境。搭建的过程很简单，把那个链接当中提到的东西都下载安装后，就可以了。
之后，我们的重点就可以放在编辑XML文件了，Docbook本身有很多规则，可以参考[链接2](http://www.docbook.org/)。由于我们只是准备一份文档，不许要像论文、书籍那么复杂，所以可以只用几个简单有用的元素，就满足我们的需求了。

下面是一个简单的例子，创建一个叫做 docbook5-demo.xml 的文件，编辑如下：
``` xml docbook5-demo.xml
    <?xml version='1.0' encoding="utf-8"?>
    <article>
    <artheader>
	<title>Instruction Example</title>  
	<author><personname><firstname>Your Name</firstname></personname></author> 
    </artheader>

    <sect1>
	<title>Topological Diagram</title>
	<figure>
	    <graphic fileref="picture.png"></graphic>
	</figure>
    </sect1>

    <sect1><title>Pre</title>
	<sect2><title>Backup following things:</title>
	    <para>item 1</para>
	    <para>item 2</para>
	    <para>item 3</para>
	</sect2>

	<sect2><title>Update things</title>
	    <sect3><title>Update xxx file in yyy folder</title>
	        <para>Remove items below:
	            <itemizedlist>
	                <listitem>removed item1</listitem>
	                <listitem>removed item2</listitem>
	                </itemizedlist>
	        </para>
	    </sect3>
	</sect2>
    </sect1>
</article>
```

很好理解的文件，对吧。

第1行，是来规定文件的编码格式。

`<article>`定义一个article的开始，在最后有相应的`</article>`来关闭他。

`<artheader>`作为article的头，定义了文档的名称，以及作者，同样需要相应的`</article>`。

`<sect1>`就是章节的划分了，在示例文件中我们会看到多个`<sect1>`，这些章节的序号会自动按照1、2、3这样的顺序生成。示例中的第一个section是一个插图，可以讲指定的图片插入到我们的文档中。`<title>`指出了该章节的标题；`<graphic>`元素中指出了插图的路径和名称，这里XML文件跟png文件在同一路径下。

往下看，我们会发现`<sect2>`这样的元素，他是我们插入的子章节，他的页面效果是这样的。第一层的`<sect1>`，会被展现成 2. 这样子，可以认为是表示第二章。`<sect2>`则会被展现成 2.1 这样子的，表示是第二章第一小节。后面的`<sect2>`和`<sect3>`以此类推。17行中的`<para>`表示一个章节中普通的段落。

在25行中，我们可以看到`<itemizedlist>`这个元素，他是一个列表。

到这里，我们已经编辑完我们的XML文件了。然后我们就可以按照最开始提到的链接1中所描述的那样，用相应的工具把这个XML文件转换成pdf文件。  
这里分两步：  
1.  首先要把XML文件转换成fo文件，在命令行模式中，去刚才编辑好的XML文件所在路径，运行：`xsltproc -o ../output/fo/docbook5-demo.fo E:\DevRoot\docbook\config\docbook-xsl-ns-1.77.1\docbook_fo.xsl docbook5-demo.xml`  这里，-o 参数后面跟的是本次将要生成fo文件的路径和名称；E盘路径那个参数，表示生成fo文件时所要参考的格式文件；最后的参数，就是刚刚编辑好的XML文件.  
2.  然后我们会得到一个叫做docbook5-demo.fo的文件，再运行：`E:\DevRoot\docbook\tools\fop-1.0\fop.cmd -c E:\DevRoot\docbook\config\fop\fop.xconf -fo ../output/fo/docbook5_demo.fo -pdf ../output/pdf/docbook5_demo.pdf`  这里，第一个E盘路径表示我们此次要运行的命令，可以根据自己所设置的位置来调整；-c 跟的参数，表示此次转换时要是用的配置文件； -fo 就是刚才生成的fo文件路径； 最后-pdf表示此次要生成的pdf的路径和名称。

运行完上面两步，我们就可以得到一个pdf文件了！

最后，我们可以在本地自己搭建一个repository（git或者hg，随你自己喜欢），把编辑好的XML文件check in上去了。以后每次写完，可以运行上面两条命令来得到交付的pdf文件；编辑完的XML文件则完全纳入版本管理当中了，可以集中、方便地管理，查询以前的提交，两次之间的diff，都是方便得很～～
