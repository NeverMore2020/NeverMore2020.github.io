教务系统只提供了全科加权平均分，而想要在出国成绩单上展现一个"更高"的成绩，我们可能需要去除"恼人"的思政类课程之后再计算主要课程成绩平均分。
博主虽然也是js新手但是发现好像从教务系统页面偷偷计算一下还是挺容易的。


```javascript
var x = document.getElementById("dataList").getElementsByTagName("tr");
var sum = 0;
var score = 0;
for(var i=0; i<x.length;i++){
    if(i===2||i===6||i===7||i===9||i===10||i===14||i===15||i===16||i===17||i===18||i===21||i===22||i===23||i===24||i===27||i===28||i===29||i===34||i===35||i===36||i===40||i===41||i===42||i===43||i===44||i===50||i===52||i===53||i===57||i===61||i===62||i===63||i===64) {//add more i according to your need. The i is relevent to sequence number of the grade table in BUPT educational administration system
        var xChild = x[i].children;
        var mul = xChild[5].firstChild.nodeValue * xChild[7].firstChild.nodeValue;
        sum = sum + mul;
        sc = parseFloat(xChild[7].firstChild.nodeValue);
        score = score + sc;
    }
}
sum/score

```



![Uploading 2021_03_01.jpg…]()







然后把上述代码输入，回车即可得到加权平均分了！


