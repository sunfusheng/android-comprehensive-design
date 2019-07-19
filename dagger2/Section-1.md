## Dagger2是什么？

[Dagger2](https://github.com/google/dagger)是[Google](https://github.com/google)从[Square](https://github.com/square)的[Dagger1](https://github.com/square/dagger)Fork过来的一个基于[JSR 330](https://jcp.org/en/jsr/detail?id=330)标准的为Android and Java设计的依赖注入框架，它的主要作用就是在编译期间自动生成代码，完成对依赖对象的创建，达到类与类之间解耦的目的。

[Dagger2](https://github.com/google/dagger)同样和[Butter Knife](https://github.com/JakeWharton/butterknife)是一样的，都是一种依赖注入框架；  
Butter Knife被称为黄油刀，主要通过注解的标记完成对资源的获取与初始化，比如：

	//使用@BindView注解标记省去重复的findViewById初始化View
	@BindView(R.id.text_view) TextView vTextView;

	//使用@OnClick注解标记省去设置监听器的麻烦
	@OnClick(R.id.submit) void submit() {
		//do something...
	}

Dagger2同样是一件利器，被称为匕首，目的就是划出一个口子注入我们自己的东西。

#### 依赖注入是什么？

依赖注入(简称DI: Dependency Injection)是面向对象编程的一种设计模式。简单的说，使用DI技术可以让对象从别处获得依赖，而不是由它自己来构造，使用DI有很多好处，它能降低代码之间的耦合度，让代码更易于测试、更易读。

不用依赖注入的时候我们以前包括现在大部分情况下是如何解决依赖的呢？  
举个例子，比如每个GitHub仓库(Repo)对应一个用户或组织(User)，这时候Repo是对User有依赖的，我们怎么处理呢？

第一种方式，通过构造函数注入，解决依赖问题

	//User数据类
	data class User constructor(val userName: String)

	//Repo数据类
	data class Repo constructor(val repoName: String, val user: User)

	//通过构造函数注入对象
	val user = User("sunfusheng")
	val repo = Repo("GitHub", user)

第二种方式，通过接口方式注入，解决依赖问题

	interface IOwnerProvider {
	    fun setOwner(owner: User)
	}

	data class Repo constructor(val repoName: String) : IOwnerProvider {
	    lateinit var user: User

	    override fun setOwner(owner: User) {
	        this.user = owner
	    }
	}

	//通过接口方式注入对象
	val repo = Repo("GitHub")
	repo.setOwner(user)
