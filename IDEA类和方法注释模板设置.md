# IDEA 类和方法注释模板设置

## 1.类注释

> 首先我们来设置 IDEA 中类的模板：（IDEA 中在创建类时会自动给添加注释)

打开：file->setting->Editor->Filr and Code Templates->Includes->File Header

![](https://s1.ax1x.com/2018/08/26/PbZwB4.png)

~~~ java
 /**
  * @Auther: ${USER}
  * @version 1.0
  * @Date: ${DATE} ${HOUR}:${MINUTE}
  * @Description: 
  */
~~~

保存即可

## 2.方法注释

> 1.打开 file->setting->Editor->LiveTemplates 点击右边上面那个绿色的 + 号，选择 Template Group 双击，然后弹出一个窗口，随便添加一个名字，我这里添加的是 MyGroup 然后点击 OK 
>
> 2.再次点击上面那个绿色的+号，选择Live Templates设置注释名字为 *

![](https://s1.ax1x.com/2018/08/26/PbZ5Ed.png)



~~~java
*
 * Description: 
 * @auther: $user$
 * @param: $param$
 * @return: $return$
 * @date: $date$ $time$
 */
~~~

> 下一步

![](https://s1.ax1x.com/2018/08/26/PbeivT.png)

> 下一步 选择java

![](https://s1.ax1x.com/2018/08/26/PbeP2V.png)

> 下一步-->点击右下角的 Edit variables 按钮，然后弹出一个窗口

![](https://s1.ax1x.com/2018/08/26/PbeC80.png)

> 下一步 选择相应的方法

![](https://s1.ax1x.com/2018/08/26/Pbe9Cq.png)

保存即可

## 3.测试

> 新建一个类进行测试 结果如下

![](https://s1.ax1x.com/2018/08/26/PbeZVJ.png)

完成。
