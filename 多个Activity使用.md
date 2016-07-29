#Bundle的使用
##Intent通常被称为两个Activity之间的信使那么我们可以将要保存的数据存放在bundle之中，然后利用Intent提供的putExtras方法将数据保存到intent中
####Android中Intent如果要传递类对象，可以通过两种方式实现。

* 方式一：Serializable，要传递的类实现Serializable接口传递对象，
* 方式二：Parcelable，要传递的类实现Parcelable接口传递对象。
Serializable（Java自带）：
Serializable是序列化的意思，表示将一个对象转换成可存储或可传输的状态。序列化后的对象可以在网络上进行传输，也可以存储到本地。

Parcelable（android 专用）：<br>
除了Serializable之外，使用Parcelable也可以实现相同的效果，
不过不同于将对象进行序列化，Parcelable方式的实现原理是将一个完整的对象进行分解，
而分解后的每一部分都是Intent所支持的数据类型，这样也就实现传递对象的功能了。

####实现Parcelable的作用

1）永久性保存对象，保存对象的字节序列到本地文件中；

2）通过序列化对象在网络中传递对象；

3）通过序列化在进程间传递对象。

####选择序列化方法的原则

1）在使用内存的时候，Parcelable比Serializable性能高，所以推荐使用Parcelable。

2）Serializable在序列化的时候会产生大量的临时变量，从而引起频繁的GC。

3）Parcelable不能使用在要将数据存储在磁盘上的情况，因为Parcelable不能很好的保证数据的持续性在外界有变化的情况下。尽管Serializable效率低点，但此时还是建议使用Serializable 。
##利用java自带的Serializable 进行序列化的例子
```java
package com.amqr.serializabletest.entity;
import java.io.Serializable;

public class Person implements Serializable{
    private static final long serialVersionUID = 7382351359868556980L;
    private String name;
    private int age;
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```
##使用，MainActivity和SecondActivity结合使用
###MainActivity
```java
package com.amqr.serializabletest;
import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;
import com.amqr.serializabletest.entity.Person;
/**
 * 进行Android开发的时候，我们都知道不能将对象的引用传给Activities或者Fragments，
 * 我们需要将这些对象放到一个Intent或者Bundle里面，然后再传递。
 *
 *
 * Android中Intent如果要传递类对象，可以通过两种方式实现。
 * 方式一：Serializable，要传递的类实现Serializable接口传递对象，
 * 方式二：Parcelable，要传递的类实现Parcelable接口传递对象。
 *
 * Serializable（Java自带）：
 * Serializable是序列化的意思，表示将一个对象转换成可存储或可传输的状态。序列化后的对象可以在网络上进行传输，也可以存储到本地。
 *
 * Parcelable（android 专用）：
 * 除了Serializable之外，使用Parcelable也可以实现相同的效果，
 * 不过不同于将对象进行序列化，Parcelable方式的实现原理是将一个完整的对象进行分解，
 * 而分解后的每一部分都是Intent所支持的数据类型，这样也就实现传递对象的功能了。
 要求被传递的对象必须实现上述2种接口中的一种才能通过Intent直接传递。
 */
public class MainActivity extends Activity {
    private TextView mTvOpenNew;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        findViewById(R.id.mTvOpenNew).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent open = new Intent(MainActivity.this,SecondActivity.class);
                Person person = new Person();
                person.setName("一去二三里");
                person.setAge(18);
                // 传输方式一，intent直接调用putExtra
                // public Intent putExtra(String name, Serializable value)
                open.putExtra("put_ser_test", person);
                // 传输方式二，intent利用putExtras（注意s）传入bundle
                /**
                Bundle bundle = new Bundle();
                bundle.putSerializable("bundle_ser",person);
                open.putExtras(bundle);
                 */
                startActivity(open);
            }
        });
    }
}
```
###SecondActivity
```java
package com.amqr.serializabletest;
import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.widget.TextView;
import com.amqr.serializabletest.entity.Person;
/**
 * User: LJM
 * Date&Time: 2016-02-22 & 11:56
 * Describe: Describe Text
 */
public class SecondActivity extends Activity{
    private TextView mTvDate;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        mTvDate = (TextView) findViewById(R.id.mTvDate);
        Intent intent = getIntent();
        // 关键方法：getSerializableExtra ，我们的类是实现了Serializable接口的，所以写这个方法获得对象
        // public class Person implements Serializable
        Person per = (Person)intent.getSerializableExtra("put_ser_test");
        //Person per = (Person)intent.getSerializableExtra("bundle_ser");
        mTvDate.setText("名字："+per.getName()+"\n"
                +"年龄："+per.getAge());
    }
}
```
##android专用的Parcelable的序列化的例子
我们写一个实体类，实现Parcelable接口，马上就被要求

1、复写describeContents方法和writeToParcel方法

2、实例化静态内部对象CREATOR，实现接口Parcelable.Creator 。

也就是，随便一个类实现了Parcelable接口就一开始就会变成这样子

Parcelable方式的实现原理是将一个完整的对象进行分解，而分解后的每一部分都是Intent所支持的数据类型，这样也就实现传递对象的功能了。
```JAVA
package com.amqr.serializabletest.entity;
import android.os.Parcel;
import android.os.Parcelable;
/**
 * User: LJM
 * Date&Time: 2016-02-22 & 14:52
 * Describe: Describe Text
 */
public class Pen implements Parcelable{
    private String color;
    private int size;

    // 系统自动添加，给createFromParcel里面用
    protected Pen(Parcel in) {
        color = in.readString();
        size = in.readInt();
    }
    public static final Creator<Pen> CREATOR = new Creator<Pen>() {
        /**
         *
         * @param in
         * @return
         * createFromParcel()方法中我们要去读取刚才写出的name和age字段，
         * 并创建一个Person对象进行返回，其中color和size都是调用Parcel的readXxx()方法读取到的，
         * 注意这里读取的顺序一定要和刚才写出的顺序完全相同。
         * 读取的工作我们利用一个构造函数帮我们完成了
         */
        @Override
        public Pen createFromParcel(Parcel in) {
            return new Pen(in); // 在构造函数里面完成了 读取 的工作
        }
        //供反序列化本类数组时调用的
        @Override
        public Pen[] newArray(int size) {
            return new Pen[size];
        }
    };

    @Override
    public int describeContents() {
        return 0;  // 内容接口描述，默认返回0即可。
    }
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(color);  // 写出 color
        dest.writeInt(size);  // 写出 size
    }
    // ======分割线，写写get和set
    //个人自己添加
    public Pen() {
    }
    //个人自己添加
    public Pen(String color, int size) {
        this.color = color;
        this.size = size;
    }

    public String getColor() {
        return color;
    }
    public void setColor(String color) {
        this.color = color;
    }
    public int getSize() {
        return size;
    }
    public void setSize(int size) {
        this.size = size;
    }
}
```
其实说起来Parcelable写起来也不是很麻烦，在as里面，我们的一个实体类写好私有变量之后，让这个类继承自Parcelable，接下的步骤是：

1、复写两个方法，分别是describeContents和writeToParcel

2、实例化静态内部对象CREATOR，实现接口Parcelable.Creator 。 以上这两步系统都已经帮我们自动做好了

3、自己写写我们所需要的构造方法，变量的get和set



