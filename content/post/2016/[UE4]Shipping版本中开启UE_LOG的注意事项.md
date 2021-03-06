---
title: "[UE4]Shipping版本中开启UE_LOG的注意事项"
date: "2016-10-16T01:40:40+08:00"
categories:
- UnrealEngine4
tags:
- UE4
---


比如UE4工程的C++代码中有如下log打印语句：

    UE_LOG(LogTemp, Display, TEXT("aaaaaaa"));

我们希望这句话在Android Device Monitor中能也能够打印出来，默认情况下，需要在将设备的Config设置为DebugGame或者Development，Shipping下则不会打印。

{{< figure src="/img/20161016-[UE4]Shipping版本中开启UE_LOG的注意事项/[UE4]Shipping版本中开启UE_LOG的注意事项-01.jpg">}}

如果想在Shipping版本中开启Log，则需要在Build.cs中添加：

	Definitions.Add("USE_LOGGING_IN_SHIPPING");
	
或者

	UEBuildConfiguration.bUseLoggingInShipping = true;

##### VisualStudio中查看UE4 log
{{< hl-text primary >}}
如果要查看VS命令行的UE4相关log，需要Debug模式（VS的Debug，即F5，不是UE4的DebugGame）
{{< /hl-text >}}