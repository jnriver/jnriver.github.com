---
layout: post
category : programer
title : 在NSTextField中实现超链接
tags : [programer, cocoa]
---
{% include JB/setup %}

工作上要实现一个效果，在app中提供一个超链接。
先按照了最近简单的思路做，填一个`NSMutableAttributedString`进去

	- (void)setHyperlink:(NSString *)text inField:(NSTextField *)field
	{
    	NSURL *link = [[NSURL alloc] initWithString:[NSString stringWithFormat:@"mailto:%@",text]];
    
    	NSMutableAttributedString* attrString = [[NSMutableAttributedString alloc] initWithString:text];
    	NSRange range = NSMakeRange(0, [attrString length]);
    
    	[attrString beginEditing];
    	[attrString addAttribute:NSLinkAttributeName value:link range:range];
    	[attrString addAttribute:NSForegroundColorAttributeName value:[NSColor blueColor] range:range];
    	[attrString addAttribute:NSUnderlineStyleAttributeName value:[NSNumber numberWithInt:NSSingleUnderlineStyle] range:range];
    	[attrString endEditing];
    
    	[field setAllowsEditingTextAttributes:YES];
    	[field setSelectable:YES];
    	[field setAttributedStringValue:attrString];
	}
结果表现效果不很理想。
然后在stack overflow中查到一个apple的官方[QA](https://developer.apple.com/library/mac/#qa/qa2006/qa1487.html)。
<br/>
<br/>
吃完饭后回来看上面提到的官方QA，结果。。就是我上面的实现方式。。好吧。。就这样吧。。