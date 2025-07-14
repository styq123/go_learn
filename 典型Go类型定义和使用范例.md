# 典型Go类型定义和使用范例

 
```
package main

import (
	"errors"
	"fmt"
)

// 1.基础类型别名和自定义类型
type UserID int
type Rate float64

// 2.结构体定义-业务实体
type User struct {
	ID     UserID
	Name   string
	Email  string
	Active bool
	Rate   Rate
}

//3.接口定义-抽象行为

type Notifier interface {
	Notify(message string) error
}

//4.自定义结构体实现接口

type EmailNotifier struct {
	SendEmail string
}

func (en EmailNotifier) Notify(message string) error {

	fmt.Printf("Sending email:%s\n", message)
	return nil
}

// 5.方法绑定-给结构体绑定业务方法
func (u *User) Activate() {
	u.Active = true
}
func (u *User) Deactivate() {
	u.Active = false
}

// 6.安全类型别名,避免混淆
type ProductID int

// 7函数类型定义
type HandlerFunc func(u *User) error

//8. 自定义错误类型

type UserError struct {
	Code    int
	Message string
}

func (e *UserError) Error() string {
	return fmt.Sprintf("Error %d %s", e.Code, e.Message)
}

func main() {
	var u User
	u.ID = 1001
	u.Name = "Alice"
	u.Email = "1.@qq.com"
	u.Rate = 9.8
	u.Activate()
	fmt.Printf("User:%+v\n", u)

	var notifier Notifier = EmailNotifier{SendEmail: "2.qq.com"}
	notifier.Notify("Welcome !")

	var handler HandlerFunc = func(u *User) error {
		if !u.Active {
			return errors.New("user not active")
		}
		fmt.Println("Handing user:", u.Name)
		return nil
	}
	if err := handler(&u); err != nil {
		fmt.Println("Hnadler error:", err)
	}

	err := &UserError{Code: 404, Message: "User not found"}
	fmt.Println(err.Error())

}




```

