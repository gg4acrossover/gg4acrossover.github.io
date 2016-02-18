---
layout: post
title: ưu tiên toán tử
date: 2016-02-18T01:40:10+07:00
categories: c++
---
Hôm nay mình sẽ nói về một số cái hay nhầm lẫn khi lập trình, tập trung vào những vấn đề như trên tựa đã ghi smiley (làm cái tựa for fun tý)

{% highlight cpp %}
mọi người thấy            mọi người hay nghĩ               thực tế
*p.f                      (*p).f                           *(p.f)
int *ap[]                 int (*ap)[]                     int *(ap[])
a & b != 0                (a & b) != 0                    a & (b != 0)
a << 4 + b                (a << 4) + b                    a << (4 + b)
{% endhighlight %}

!= và == sẽ được ưu tiên hơn các toán tử bitwise.
Các phép tính toán học sẽ ưu tiên hơn phép dịch bit.
Con trỏ tới mảng, và mảng con trỏ chắc gặp nhiều.

{% highlight cpp %}
int main(int argc, const char * argv[])
{
    int a[] = { 2, 4, 6};
    
    // 1, con tro toi mang
    int (*pta)[] = &a;
    printf("%i \n", *(*pta +2)); // phan tu thu 3
    
    // 2, mang con tro
    int *pa[3];
    
    pa[0] = &a[0];
    pa[1] = &a[1];
    pa[2] = &a[2];
    
    printf("%i \n", **(pa+2)); // phan tu thu 3
    
    return 0;
}
{% endhighlight %}

P/S: :d vừa rồi cty mình nhận 1 dự án maintain (code c) toàn bitwise, khoai lòi mắt T_T.
