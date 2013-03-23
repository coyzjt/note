#编译boost的方法

1. 下载boost1.4.6
2. 解压进入,然后执行

	./bootstrap.sh

3. 再执行
	./bjam

4. 最后把boost 子目录拷贝到`/usr/local/include`目录

#使用TR1 的方法

* shared_ptr 的头文件

在4.4 以上的gcc中 #include<tr1/memory>
vs2008 sp1 或者 vs2010 中 #include<memory>

