---
layout: post
category : Program
tagline: 
tags : [JAVA]
---
{% include JB/setup %}

确保Class对象既表示枚举又表示Operation子类型


.
private static <T extends Enum<T> & Opreation> void test(Class<T> opSet,double x,double y){

}



