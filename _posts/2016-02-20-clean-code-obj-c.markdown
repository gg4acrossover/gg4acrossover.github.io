---
layout: post
title: code sạch, code khô thoáng
date: 2016-02-20T01:54:53+05:30
categories: objc
---

Đây là một số phương pháp khi tớ làm việc với obj c. Với mỗi người, tổ chức có thể khác, thế nên cái này chỉ mang tính tham khảo. :smile: 

# Sử dụng immutable

Đối với array hay dictionary nếu không cần sử dụng mutable thì ta nên viết như sau:

{% highlight cpp %}
	NSArray *pFruit = @[@"orange", @"apple", @"lemon", @"strawberry"];
	NSDictionary *pDict = @{ @"name" : @"Tom",  @"age" : @18};
{% endhighlight %}

Thay vì viết:

{% highlight cpp %}
	NSArray *pFruit = [NSArray arrayWithObjects:@"orange", @"apple", @"lemon", @"strawberry", nil];
	NSDictionary *pDict2 = [NSDictionary dictionaryWithObjectsAndKeys:@"value1",@"key1",@"value2", @"key2", nil];
{% endhighlight %}

Như thế code nhìn trông gọn và dễ đọc hơn, đối với các biến dạng số hoặc bool được đưa vào trong dictionary để gửi lên server ta cũng nên làm như vậy.

{% highlight cpp %}
	NSNumber *pIsNew = @YES;
{% endhighlight %}

Thay vì

{% highlight cpp %}
	NSNumber *pIsNew = [NSNumber numberWithBool:YES];
{% endhighlight %}

Sử dụng category

Frame được sử dụng nhiều khi viết code iOS, ta có thể tạo ra một file chứa các c function chuyên làm việc với frame, hay tạo ra một macro. Ví dụ như muốn lấy originX của 1 uiview:

{% highlight cpp %}
	CGFloat ORIGIN_X( CGRect rec)
	{
            return rec.origin.x;
	}
{% endhighlight %}

Hoặc ngắn hơn với macro

{% highlight cpp %}
#define posX(v)                  v.frame.origin.x
{% endhighlight %}

Tuy nhiên sử dụng category cũng là một lựa chọn thời thượng. Giả sử bạn có thể lấy originX bằng cách sau:

{% highlight cpp %}
CGFloat originX = [PopUpView getX];
{% endhighlight %}

Khá ngắn gọn, clear, dễ chỉnh sửa. Category còn đặc biệt với những ai hay gửi nhận dữ liệu lên server, đôi khi dictionary sẽ trả về một cái object null. Việc check null ở nhiều chỗ khác nhau khá là tốn thời gian nếu cứ phải viết đi viết lại như thế này

{% highlight cpp %}
if(dict[@"key"] && ![dict[@"key"] isEqual: [NSNull null]])
{% endhighlight %}

Ta có thể đưa vào category theo cách sau

{% highlight cpp %}
- (id)safeValueForKey:(NSString *)key {
    id value = [self valueForKey:key];
     
    if (value && ![value isEqual: [NSNull null]]) {
        return value;
    }
     
    return nil;
}
{% endhighlight %}

Sau đó ở tất cả mọi nơi chỉ cần gọi

{% highlight cpp %}
NSString *pValue = [dict safeValueForKey: @"key"];
{% endhighlight %}

# Sử dụng block

{% highlight ruby %}
UIView *pCircle2 = ({
	UIView *pView = [[UIView alloc]
					initWithFrame:CGRectMake(80.0f, 80.0f, 90.0f, 90.0f)];
	pView.backgroundColor = [UIColor redColor];
	CAShapeLayer *shape = [CAShapeLayer layer];
	CGFloat r = CGRectGetWidth(pView.frame)*0.5f;
	CGPoint center = CGPointMake( r, r);
	UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center
							    radius:r
							startAngle:0
							  endAngle:2*M_PI
							 clockwise:YES];
	shape.path = path.CGPath;
	pView.layer.mask = shape;
	pView;
});
{% endhighlight %}

Gộp các đoạn code sử dụng chung cho một mục đích vào trong 1 block, với cách này thì giá trị return tương ứng với dòng cuối trong block.

# cách đặt tên

Nhiều tài liệu của apple cũng có nói về cách đặt tên. Mọi người hãy chú ý delegate của uitableview hay uicollectionview sẽ thấy có nhiều điểm tương đồng trong cách đặt tên hàm.
Ví dụ guide của apple [datten](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)

Từ cách đặt tên trên ta có thể tạo ra enum như sau

{% highlight cpp %}
typedef NS_ENUM(NSUInteger, GGAnimationViewType) {
    GGAnimationViewTypeFadeIn,
    GGAnimationViewTypeFadeOut,
    GGAnimationViewTypeEaseIn,
    GGAnimationViewTypeEaseOut
};
{% endhighlight %}

Công thức chung là [tên class][tên enum], cụ thể như ví dụ trên [GGAnimationView][Type].
Các giá trị enum [tên class][tên enum][các giá trị]. Các giá trị FadeIn, FadeOut,....

Tóm lại là code càng ngắn gọn càng dễ hiểu, trừ cách đặt tên biến ra :v.