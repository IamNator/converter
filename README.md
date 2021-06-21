a lib for golang , generate mysql table schema to golang struct  
-----
mysql table structure automatically generates golang struct

## github address
[https://github.com/gohouse/converter](https://github.com/gohouse/converter)

## installation
1. Download the executable file directly: [download address](https://github.com/gohouse/converter/releases)  
2. golang source package: `go get github.com/gohouse/converter`

## Example table structure
```sql
CREATE TABLE `prefix_user` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `Email` varchar(32) NOT NULL DEFAULT '' COMMENT '邮箱',
  `Password` varchar(32) NOT NULL DEFAULT '' COMMENT '密码',
  `Status` tinyint NOTNULL DEFAULT 0 
  `CreatedAt` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT= ' User table '
```

## Command line usage
1. Download the executable file of the corresponding platform, [download address](https://github.com/gohouse/converter/releases)

2. Command line execution
    ```sh
    # 文件名: table2struct-[$platform].[$version].[$suffix]
    ./table2struct-linux.v0.0.3.bin -file model.go -dsn xxx -table user
    ```

3. Parameter Description
```sh
-dsn string database dsn configuration
-enableJsonTag bool Whether to add json tag
-file string save path
-packageName    string 包名
-prefix string table prefix
-realNameMethod string table name corresponding to the structure
-table string the table to be migrated
-tagKey         string tag的key
```

## golang代码简单用法
```go
package main
import (
	"fmt"
	"github.com/gohouse/converter"
)
func main() {
	err := converter.NewTable2Struct().
		SavePath("/home/go/project/model/model.go").
		Dsn("root:root@tcp(localhost:3306)/test?charset=utf8").
		Run()
	fmt.Println(err)
}
```

## golang代码详细用法示例
```go
package main

import (
	"fmt"
	"github.com/gohouse/converter"
)

package main

import (
	"fmt"
	"github.com/gohouse/converter"
)

func  main () {
	 // Initialize 
	t2t  :=  converter . NewTable2Struct ()
	 // Personalized configuration 
	t2t . Config ( & converter. T2tConfig {
		 // If the first letter of the field is originally uppercase, no tag is added, default false is added, true Do not add 
		RmTagIfUcFirsted : false ,
		 // Whether the field name of the tag is converted to lowercase, if it has uppercase letters, the default is false. 
		TagToLower : false ,
		 // While the first letter of the field is capitalized, whether to convert other letters to lowercase, The default false does not convert 
		UcFirstOnly : false ,
		 //// Each struct is placed in a separate file, the default is false, placed in the same file (not provided yet) 
		//SeperatFile: false,
	})
	// Start the migration conversion 
	err  :=  t2t .
		 // Specify a table, if not specified, all tables are migrated by default 
		Table ( "user" ).
		 // Table prefix 
		Prefix ( "prefix_" ).
		 // Whether to add json tag 
		EnableJsonTag ( true ).
		 // Generate the package name of the struct (if it is empty by default, the name is: package model) 
		PackageName ( "model" ).
		 // The key value of the tag field, the default is orm 
		TagKey ( "orm" ).
		 // Whether to add a structure method to obtain the table name 
		RealNameMethod ( "TableName" ).
		 // The generated structure save path 
		SavePath ("/Users/fizz/go/src/github.com/gohouse/gupiao/model/model.go" ).
		 // Database dsn, here you can use t2t.DB() instead, the parameter is *sql.DB object 
		Dsn ( "root:root@tcp(localhost:3306)/test?charset=utf8" ).
		 // Execute 
		Run ()
	
	fmt.Println(err)
}
```

result 
```go
package model

import "time"

type User struct {
	Id         int     `json:"Id" orm:"Id"`
	Email      string  `json:"Email" orm:"Email"`           // 邮箱
	Password   string  `json:"Password" orm:"Password"`     // 密码
	Status     bool     `json:"Status" orm:"Status"`
	CreatedAt  string  `json:"CreatedAt" orm:"CreatedAt"`
}

func (*User) TableName() string {
	return "user"
}
```
