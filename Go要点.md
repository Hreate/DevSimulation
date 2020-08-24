# Go要点

### Go方法接收者

#### 前言

Go中，除了slice、map、chan是引用传递，其他类型都是值传递

#### 大意

##### 接收者分为

* 值接收者 ：接收者的类型是一个值，传递的是一个副本，方法内部无法对其真正的接收者做更改。
* 指针接收者：接收者的类型是一个指针，传递的是接收者的引用，对这个引用的修改影响真正的接收者。

```go
type ErrSocket error
var resp = make(chan ErrSocket)

if err := f(); err != nil {
				resp <- err
			}
func DaemonListen(err <-chan ErrSocket) error {
	for {
		v, ok := <-err
		if ok {
			return v
		}
	}
}
func test() error {
	DaemonListen(resp)
	StartTimer(FindAllUsers)
}
```

