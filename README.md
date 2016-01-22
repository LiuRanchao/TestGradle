##官网文档：
http://tools.android.com/tech-docs/new-build-system/user-guide

##命令：

gradle assemble<Variant Name>    
allows directly building a single variant. For instance assembleFlavor1Debug.

gradle assemble<Build Type Name>
allows building all APKs for a given Build Type. For instance assembleDebug will build both Flavor1Debug and Flavor2Debug variants.
   
gradle assemble<Product Flavor Name>    
allows building all APKs for a given flavor. For instance assembleFlavor1 will build both Flavor1Debug and Flavor1Release variants.


##问题：
gradle生成目录
美团打包方式，是否可以修改包名，修改应用名，修改mainfest.xml中第三方参数
build.gradle 拆分
gradle 能做什么插件？ 

