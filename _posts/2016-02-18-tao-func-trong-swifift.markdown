---
layout: post
title: tạo func trong swift
date: 2016-02-18T16:31:54+05:30
categories: swift
---

Có nhiều cách để tạo func hơn là làm bánh hay làm tình ( chế từ câu nói của Nguyễn Miền Biên Thùy).

# Tạo function 1 param

{% highlight ruby %}
func sayHello(name: String) -> String 
{
    return "Hello " + name + "!"    
}

print( sayHello("Peter")) // Hello Perter!
{% endhighlight %}

# Func có nhiều hơn 1 param

{% highlight ruby %}
func sayHelloAgain(name : String, anotherName: String) -> String
{
    return "Hello " + name + ", " + anotherName
}

print( sayHelloAgain("Peter", anotherName: "Tom")) // Hello Peter, Tom
{% endhighlight %}

ở param thứ 2 ta phải viết thêm cái tên biến ở đằng trước. Cái này để làm rõ nghĩa của hàm, nếu không viết thì sẽ báo lỗi.
Ta có thể làm rõ nghĩa của cả param đầu tiên bằng cách cho thêm external name. Mỗi khi thêm external name thì ta bắt buộc phải sử dụng nó khi gọi hàm.
Ở ví dụ dưới đây external name là *to* và *and*

{% highlight python %}
func sayHello(to person: String, and anotherPerson: String) -> String 
{
    return "Hello \(person) and \(anotherPerson)!"
}
print(sayHello(to: "Bill", and: "Ted")) // Hello Bill and Ted!
{% endhighlight %}

Nếu ta muốn viết gọn như C thì có thể thêm _ để loại bỏ việc phải viết thêm tên biến nếu có hơn 2 param.

{% highlight python %}
func someFunction(firstParameterName: Int, _ secondParameterName: Int) 
{
    // function body goes here
    // firstParameterName and secondParameterName refer to
    // the argument values for the first and second parameters
}
someFunction(1, 2)
{% endhighlight %}

Ở ví dụ trên _ được viết trước secondParameterName nên ta không phải viết
> someFunction(1, secondParameterName:2)

# Func với param dạng ...

{% highlight python %}
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5) // 3.0
{% endhighlight %}

Với kiểu này các biến sẽ được đưa vào một array tên là numbers, Ai làm objective c rồi chắc quen thuộc với hàm kiểu này. Để triển khai nó trên objective c thì phức tạp (liên quan đến con trỏ) nhưng trên swift thì đơn giản hơn rất nhiều.
Lưu ý: tất cả những param truyền vào phải cùng một kiểu dữ liệu.

# Thay đổi giá trị param trong func
Theo mặc định, param truyền vào sẽ là const, tức là ta không thể thay đổi giá trị của nó. Để thay đổi có hai cách, cách đầu tiên là sử dụng keyword var như ở ví dụ sau

var minMaxValue = minMax([2,1,5,9,8])!;
print(minMaxValue.1); // max

let minAndMax = minMax([]);
print(minAndMax?.min);

{% highlight python %}
func alignRight(var string: String, totalLength: Int, pad: Character) -> String {
    let amountToPad = totalLength - string.characters.count
    if amountToPad < 1 {
        return string
    }
    let padString = String(pad)
    for _ in 1...amountToPad {
        string = padString + string
    }
    return string
}
var originalString = "hello"
var paddedString = alignRight(originalString, totalLength: 10, pad: "-")
print(originalString, terminator:"") // hello
print(paddedString, terminator:"") // -----hello
{% endhighlight %}

Ở đây ta thay đổi giá trị của originalString trong thân func alignRight.
Tuy nhiên có lưu ý nho nhỏ, giá trị của originalString ở ngoài func alignRight sẽ không bị thay đổi theo. Vậy để thay đổi giá trị của originalString ta phải làm sao? Ta sử dụng cách thứ hai là In-Out param.

# In-Out param

{% highlight python %}
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// prints "someInt is now 107, and anotherInt is now 3
{% endhighlight %}

Khi gọi những hàm có keyword inout ta phải thêm & đằng trước tên biến.

Đây là những điều cơ bản nhất khi triển khai 1 func trong swift. Để đi sâu hơn, bạn nên tham khảo tài liệu của apple.

P/S: những ví dụ trên tham khảo trong swift 2.0 programming của apple. Tài liệu này mới cập nhật, hình như chưa có bản pdf. :stuck_out_tongue: