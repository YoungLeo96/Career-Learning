# Variable

    Var a int

    var b string

    var c []float32 - 声明一个32 位浮点切片类型的变量，浮点切片表示由多个浮点类型组成的数据结构

    var d func() bool - 声明一个返回值为布尔类型的函数变量，这种形式一般用于回调函数，即将函数以变量的形式保存下来，在需要的时候重新调用这个函数

    var e struct{
        x int
    }
        - 声明一个结构体类型的变量，这个结构体拥有一个整型的 x 字段
    
## Batch variables defining

    var (
        a int
        b string
        c []float32
        d func() bool
        e struct{
            x int
        }
    )

# Variable-Initialized

    Varibales automaticlly get value when define them, eg:
        int and float defalut 0
        string default ''
        bool default bool
        slice,func,pointer(指针) default nil

# Variable type by derivation(推导)

    var hp = 100
        hp type is same as 100 

# Short variables defined and initialized

    hp := 100
        cannot exist 'var hp int' before this statement

    eg:
        conn,err := net.Dial('tcp','127.0.0.1:8080')
        conn,err2 := net.Dial('tcp','127.0.0.1:8080')
            left-variables must exist one new at least

# Multiple Assignment(赋值)

    eg-1:
        var a int = 100
        var b int = 200
        b,a = a,b

    eg-2: (排序)
        type IntSlice []int

        func (p IntSlice) Len () int         {return len(p)}
        func (p IntSlice) Less(i,j int) bool {return p[i] < p[j]}
        func (p IntSlice) Swap(i,j int)      {p[i],p[j] = p[j],p[i]}

# Anonymous Variable (匿名变量)

    _  --> do not accept value
    eg:
        func GetData() (int,int) {
            return 100,200
        } 

        a,_ := GetData()
        _,b := GetData()

        fmt.PrintIn(a,b)

        $ 100 200

    匿名变量不占用命名空间，不会分配内存。匿名变量与匿名变量之间也不会因为多次声明而无法使用

# Type of Int

## According to size

    int8,int16,int32,int64

## According to sign(符号)

    uint8,uint16,uint32,uint63
        -- no sign int

    uint8 -- is the 'byte' we knew
    int16 -- is the 'short' in C-language
    int63 -- is the 'long' in C-language

## Automaticlly map with platform

    int and uint can do 

        逻辑对整型范围没有特殊需求。例如，对象的长度使用内建 len() 函数返回，这个长度可以根据不同平台的字节长度进行变化。实际使用中，切片或 map 的元素数量等都可以用 int 来表示。

    sometime we cannot use int or uint:

        反之，在二进制传输、读写文件的结构描述时，为了保持文件的结构不会受到不同编译目标平台字节长度的影响，不要使用 int 和 uint。  

# Type of Float

    Go-Language supports two types of float
        float32 and float63 which follow IEEE754

    float32:
        Max-Size: 3.4e38
        Can be defined by a constant(常量): math.MaxFloat32

    float64:
        Max-Size: 1.8e308
        Can be defined by a constant: math.MaxFloat64

    eg:
        fmt.Printf('%.2f\n',math.Pi)
        $ 3.14

# Output images of Sin function

## Function of Sin

    Function Sin supplied by package of math
        -- math.Sin

    Parameters of Sin:
        -- float64

    Return of Sin:
        -- float64

    According to actual, can turnover them

## Image

    Standard Liberary can access to images and output multiple types of image file such as JPEG,PNG,GIF and so on

    eg:
        package main

        import (
            "image"
            "image/color"
            "image/png"
            "log"
            "math"
            "os"
        )

        func main() {

            // 图片大小
            const size = 300
            // 根据给定大小创建灰度图
            pic := image.NewGray(image.Rect(0, 0, size, size))

            // 遍历每个像素
            for x := 0; x < size; x++ {
                for y := 0; y < size; y++ {
                    // 填充为白色
                    pic.SetGray(x, y, color.Gray{255})
                }
            }

            // 从0到最大像素生成x坐标
            for x := 0; x < size; x++ {

                // 让sin的值的范围在0~2Pi之间
                s := float64(x) * 2 * math.Pi / size

                // sin的幅度为一半的像素。向下偏移一半像素并翻转
                y := size/2 - math.Sin(s)*size/2

                // 用黑色绘制sin轨迹
                pic.SetGray(x, int(y), color.Gray{0})
            }

            // 创建文件
            file, err := os.Create("sin.png")

            if err != nil {
                log.Fatal(err)
            }
            // 使用png格式将数据写入文件
            png.Encode(file, pic) //将image信息写入文件中

            // 关闭文件
            file.Close()
        }