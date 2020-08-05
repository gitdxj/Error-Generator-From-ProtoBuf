# 从Proto文件中生成Error
在proto文件中定义错误信息，我们经常用enum来实现，而此种方法并不能产生golang中的error，实际使用当中并不方便。  
.proto 文件的解析 使用的是 [pbparser](https://github.com/tallstoat/pbparser)，这个包不依赖任何protobuf的官方包  

## 安装
```
go get github.com/gitdxj/errgen
```
## 如何使用
使用 error generator 例子如下：  


demo/student.proto中定义如下的Error类型：
```
enum ErrStudent{
    Err_Name_Cannot_Print = 0;
    Err_Transgender = 1;
    Err_Grade_Zero = 2;
}
```
可以生成如下的go代码：
```go
package student	
import (
	"errors"
)

type ErrStudent uint32

const (
	Err_Name_Cannot_Print ErrStudent = 1
	Err_Transgender ErrStudent = 2
	Err_Grade_Zero ErrStudent = 2

)


func (e ErrStudent) Error() error {
	switch e {
	case Err_Name_Cannot_Print:
		return errors.New("0: Err_Name_Cannot_Print")
	case Err_Transgender:
		return errors.New("1: Err_Transgender")
	case Err_Grade_Zero:
		return errors.New("2: Err_Grade_Zero")

	}
	return nil
}
```
其中返回的错误描述，如果在proto文件中使用注释进行标明，就直接使用注释中的描述，如：
```
enum ErrStudent{
    // 注释1
    Err_Name_Cannot_Print = 0;
    // 注释2
    Err_Transgender = 1;
    // 注释3
    Err_Grade_Zero = 2;
}
```
```go
func (e ErrStudent) Error() error {
	switch e {
	case Err_Name_Cannot_Print:
		return errors.New("注释1")
	case Err_Transgender:
		return errors.New("注释2")
	case Err_Grade_Zero:
		return errors.New("注释3")

	}
	return nil
}
```