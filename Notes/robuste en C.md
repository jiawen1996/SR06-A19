strcpy()在输入随机密码的时候并没有提前

# Robuste en C

SEI sert -> langage C

### Méfiance extrême



### Tout peut arriver

* 错误的来源可能来自于用户也可能其他的地方 - 尽早处理

## Bonnes pratiques

### Organisation

####  Organisation du projet

* 模块化结构
* style du code, convention
* Gestion des versions

#### S'adapter aux compétences des développeurs

### Mémoire

#### Gestion de la mémoire

* 加载必要的空间
* 经常释放空间

* 用const empêcher toute modification qui n'a pas lieu d'être

#### off by one

strncpy 有bufferOverFlow风险，会破坏内存
 声明：char *strncpy(char *strDest,const char *strSource,size_t count);
 功能：将strSource的count个字符复制到strDest中，如果count小于或者等于 strSource的长度，
NULL字符就不会自动的加到已经复制的字符串后面；如果count大于strSource的长 度，会用NULL字符来填充到复制的字符串后面使其长度达到count

## Envisager l'imprévu

* 尽可能地使用const

  * 在编译的时候可以检测到错误
  * 存在read-only的zone中

* const char* XXXX ="XX"

  char* XXXX ="XX"



str = (char*)malloc() == null





* printf - format string

  具体原理：当printf在输出格式化字符串的时候，会维护一个内部指针，当printf逐步将格式化字符串的字符打印到屏幕，当遇到%的时候，printf会期望它后面跟着一个格式字符串，因此会递增内部字符串以抓取格式控制符的输入值。这就是问题所在，printf无法知道栈上是否放置了正确数量的变量供它操作，如果没有足够的变量可供操作，而指针按正常情况下递增，就会产生越界访问。甚至由于%n的问题，可导致任意地址读写。

* 不要讲文件名以_开头命名，会和bibilo的文件冲突

* ++n 在判断时执行一次，在进condition之后再执行一次
* unsigned char i



ex-for-bad

根据用户输入的字符串长度输出，如果输入太多，256

在unsign中256是0

ex-test-bad

如果没有成功地test，所以没有tab这个表

ex-table-bad



extern 定义全局变量

#### Problème d'initialisation

需要定义一个专门初始化数组的函数

如果输入的字符串相同，生成的

srandom - 

junk - une variable non initialisé

所以不能使用一个没有初始化的变量来生成随机序列



尽量避免使用 ++i

foo(void) {

return

}







* 缺少<stdlib.h> malloc

ordre

a和b的调用顺序不明



errno ->已经在bilio中

需要把errno初始化为0

需要检验fopen返回的指针 fileptr == NULL

#### system()

* 如果改变了$HOME 就能改变~，在运行system的时候会出现问题

* injection de commande

  happy ; useradd "XXX"

  

## Langage Rust

```rust
let i = 0 
//定义一个const
//如果想定义一个modifiable
let mut j = 0
//我们可以定义一个临时变量通过{}

//match = switch
//我们可以提出interval，rust如果没有加入default cas，会报错
//如果要return的话，就不要加;

//我们需要一个函数来test子函数是否正常运行
assert_eq!(fonction, res)
```

```bash
rustc xxx.rs
rustc xx.rs --test
```

welcome()复制了tempon中的字符串，但是并没有检验他的长度

welcome()输入字符串的长度，和字符串，返回指针的地址，

他把字符串保存在了tempon中，默认的tempon定义是八个字符串

débourtement du buffer