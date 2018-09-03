- 将OC代码转换成C/C++代码

```
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc OC源文件 -o 输出的CPP文件
//如果需要链接其他框架，使用-framework参数。比如-framework UIKit
```

