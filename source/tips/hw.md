
请在草稿纸上手写一个单例模式的实现代码，拍照提交作业
最省心的写法：在类加载的时候就初始化好了，所以性能好，但某些情况下也会出现内存浪费的情况，更别说被反射和序列化攻击了。



最麻烦写法：解决了某些情况下内存浪费的情况，采用双重检查的机制，相比直接用synchronized关键字也提供了一些性能，也实现了线程安全，但是代码的可读性比较差。跟上面那种一样都存在反射和序列化攻击的风险。

最优雅的写法：利用java内存类的特性，很简洁的生成了单例，不存在线程安全问题。Singleton$SingletonHolder,存在的问题也同样

最推荐的写法：直接上图，





请用组合设计模式编写程序，打印输出图 1 的窗口，窗口组件的树结构如图 2 所示，打印输出示例参考图




代码如下 ：



//打印结果
"C:\Program Files\Java\jdk1.8.0_202\bin\java.exe" -Dvisualvm.id=8377219460099 "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2019.3.3\lib\idea_rt.jar=52681:C:\Program Files\JetBrains\IntelliJ IDEA 2019.3.3\bin" -Dfile.encoding=UTF-8 -classpath "C:\Program Files\Java\jdk1.8.0_202\jre\lib\charsets.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\deploy.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\access-bridge-64.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\cldrdata.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\dnsns.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\jaccess.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\jfxrt.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\localedata.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\nashorn.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\sunec.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\sunjce_provider.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\sunmscapi.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\sunpkcs11.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\ext\zipfs.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\javaws.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\jce.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\jfr.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\jfxswt.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\jsse.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\management-agent.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\plugin.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\resources.jar;C:\Program Files\Java\jdk1.8.0_202\jre\lib\rt.jar;D:\mywk\das-multi\js-frame\target\classes;E:\maven\repo\commons-httpclient\commons-httpclient\3.1\commons-httpclient-3.1.jar;E:\maven\repo\commons-codec\commons-codec\1.2\commons-codec-1.2.jar;E:\maven\repo\org\nutz\nutz\1.r.68.v20191031\nutz-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-plugins-daocache\1.r.68.v20191031\nutz-plugins-daocache-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-plugins-cache\1.r.68.v20191031\nutz-plugins-cache-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-integration-quartz\1.r.68.v20191031\nutz-integration-quartz-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-integration-jedis\1.r.68.v20191031\nutz-integration-jedis-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-plugins-wkcache\1.r.68.v20191031\nutz-plugins-wkcache-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-plugins-validation\1.r.68.v20191031\nutz-plugins-validation-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-plugins-mongodb\1.r.68.v20191031\nutz-plugins-mongodb-1.r.68.v20191031.jar;E:\maven\repo\org\mongodb\mongo-java-driver\3.5.0\mongo-java-driver-3.5.0.jar;E:\maven\repo\org\nutz\nutzboot-starter-nutz-mvc\2.3.7.v20190731\nutzboot-starter-nutz-mvc-2.3.7.v20190731.jar;E:\maven\repo\org\nutz\nutzboot-core\2.3.7.v20190731\nutzboot-core-2.3.7.v20190731.jar;E:\maven\repo\org\nutz\nutzboot-starter-feign\2.3.7.v20190731\nutzboot-starter-feign-2.3.7.v20190731.jar;E:\maven\repo\io\github\openfeign\feign-core\9.5.1\feign-core-9.5.1.jar;E:\maven\repo\io\github\openfeign\feign-slf4j\9.5.1\feign-slf4j-9.5.1.jar;E:\maven\repo\io\github\openfeign\feign-hystrix\9.5.1\feign-hystrix-9.5.1.jar;E:\maven\repo\com\netflix\archaius\archaius-core\0.6.6\archaius-core-0.6.6.jar;E:\maven\repo\com\netflix\hystrix\hystrix-core\1.4.26\hystrix-core-1.4.26.jar;E:\maven\repo\io\github\openfeign\feign-ribbon\9.5.1\feign-ribbon-9.5.1.jar;E:\maven\repo\com\netflix\ribbon\ribbon-loadbalancer\2.2.4\ribbon-loadbalancer-2.2.4.jar;E:\maven\repo\com\netflix\netflix-commons\netflix-statistics\0.1.1\netflix-statistics-0.1.1.jar;E:\maven\repo\io\reactivex\rxjava\1.0.9\rxjava-1.0.9.jar;E:\maven\repo\com\netflix\servo\servo-core\0.10.1\servo-core-0.10.1.jar;E:\maven\repo\com\netflix\servo\servo-internal\0.10.1\servo-internal-0.10.1.jar;E:\maven\repo\com\google\guava\guava\16.0.1\guava-16.0.1.jar;E:\maven\repo\com\netflix\netflix-commons\netflix-commons-util\0.1.1\netflix-commons-util-0.1.1.jar;E:\maven\repo\com\netflix\ribbon\ribbon-core\2.2.4\ribbon-core-2.2.4.jar;E:\maven\repo\com\google\code\findbugs\annotations\2.0.0\annotations-2.0.0.jar;E:\maven\repo\commons-configuration\commons-configuration\1.8\commons-configuration-1.8.jar;E:\maven\repo\org\nutz\nutzwx\1.r.68.v20191031\nutzwx-1.r.68.v20191031.jar;E:\maven\repo\org\nutz\nutz-plugins-views\1.r.68.v20191031\nutz-plugins-views-1.r.68.v20191031.jar;E:\maven\repo\com\vdurmont\emoji-java\4.0.0\emoji-java-4.0.0.jar;E:\maven\repo\org\quartz-scheduler\quartz\2.2.3\quartz-2.2.3.jar;E:\maven\repo\c3p0\c3p0\0.9.1.1\c3p0-0.9.1.1.jar;E:\maven\repo\org\apache\shiro\shiro-web\1.4.0\shiro-web-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-core\1.4.0\shiro-core-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-lang\1.4.0\shiro-lang-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-cache\1.4.0\shiro-cache-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-crypto-hash\1.4.0\shiro-crypto-hash-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-crypto-core\1.4.0\shiro-crypto-core-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-crypto-cipher\1.4.0\shiro-crypto-cipher-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-config-core\1.4.0\shiro-config-core-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-config-ogdl\1.4.0\shiro-config-ogdl-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-event\1.4.0\shiro-event-1.4.0.jar;E:\maven\repo\org\apache\shiro\shiro-cas\1.4.0\shiro-cas-1.4.0.jar;E:\maven\repo\org\jasig\cas\client\cas-client-core\3.2.2\cas-client-core-3.2.2.jar;E:\maven\repo\org\apache\shiro\shiro-ehcache\1.4.0\shiro-ehcache-1.4.0.jar;E:\maven\repo\org\slf4j\slf4j-log4j12\1.7.25\slf4j-log4j12-1.7.25.jar;E:\maven\repo\org\slf4j\slf4j-api\1.7.25\slf4j-api-1.7.25.jar;E:\maven\repo\log4j\log4j\1.2.17\log4j-1.2.17.jar;E:\maven\repo\com\alibaba\druid\1.1.9\druid-1.1.9.jar;E:\maven\repo\xml-apis\xml-apis\1.4.01\xml-apis-1.4.01.jar;E:\maven\repo\org\apache\poi\poi-ooxml\3.11-beta1\poi-ooxml-3.11-beta1.jar;E:\maven\repo\org\apache\poi\poi\3.11-beta1\poi-3.11-beta1.jar;E:\maven\repo\org\apache\poi\poi-ooxml-schemas\3.11-beta1\poi-ooxml-schemas-3.11-beta1.jar;E:\maven\repo\org\apache\xmlbeans\xmlbeans\2.3.0\xmlbeans-2.3.0.jar;E:\maven\repo\stax\stax-api\1.0.1\stax-api-1.0.1.jar;E:\maven\repo\dom4j\dom4j\1.6.1\dom4j-1.6.1.jar;E:\maven\repo\com\microsoft\sqlserver\mssql-jdbc\6.5.0.jre8-preview\mssql-jdbc-6.5.0.jre8-preview.jar;E:\maven\repo\com\oracle\ojdbc6\11.2.0.3\ojdbc6-11.2.0.3.jar;E:\maven\repo\org\jsoup\jsoup\1.11.2\jsoup-1.11.2.jar;E:\maven\repo\com\belerweb\pinyin4j\2.5.1\pinyin4j-2.5.1.jar;E:\maven\repo\net\sourceforge\jtds\jtds\1.3.1\jtds-1.3.1.jar;E:\maven\repo\com\google\zxing\javase\3.2.1\javase-3.2.1.jar;E:\maven\repo\com\google\zxing\core\3.2.1\core-3.2.1.jar;E:\maven\repo\com\beust\jcommander\1.48\jcommander-1.48.jar;E:\maven\repo\net\sf\ehcache\ehcache\2.10.4\ehcache-2.10.4.jar;E:\maven\repo\redis\clients\jedis\2.9.0\jedis-2.9.0.jar;E:\maven\repo\org\apache\commons\commons-pool2\2.4.2\commons-pool2-2.4.2.jar;E:\maven\repo\org\apache\commons\commons-email\1.4\commons-email-1.4.jar;E:\maven\repo\com\sun\mail\javax.mail\1.5.5\javax.mail-1.5.5.jar;E:\maven\repo\javax\activation\activation\1.1.1\activation-1.1.1.jar;E:\maven\repo\org\apache\commons\commons-lang3\3.4\commons-lang3-3.4.jar;E:\maven\repo\commons-beanutils\commons-beanutils\1.9.3\commons-beanutils-1.9.3.jar;E:\maven\repo\com\ibeetl\beetl\2.7.27\beetl-2.7.27.jar;E:\maven\repo\org\antlr\antlr4-runtime\4.2\antlr4-runtime-4.2.jar;E:\maven\repo\org\abego\treelayout\org.abego.treelayout.core\1.0.1\org.abego.treelayout.core-1.0.1.jar;E:\maven\repo\org\antlr\antlr4-annotations\4.2\antlr4-annotations-4.2.jar;E:\maven\repo\commons-collections\commons-collections\3.2.1\commons-collections-3.2.1.jar;E:\maven\repo\commons-lang\commons-lang\2.6\commons-lang-2.6.jar;E:\maven\repo\commons-logging\commons-logging\1.1\commons-logging-1.1.jar;E:\maven\repo\org\brickred\socialauth\4.12\socialauth-4.12.jar;E:\maven\repo\org\json\json\20160212\json-20160212.jar;E:\maven\repo\io\jsonwebtoken\jjwt\0.6.0\jjwt-0.6.0.jar;E:\maven\repo\com\fasterxml\jackson\core\jackson-databind\2.8.1\jackson-databind-2.8.1.jar;E:\maven\repo\com\fasterxml\jackson\core\jackson-annotations\2.8.0\jackson-annotations-2.8.0.jar;E:\maven\repo\com\fasterxml\jackson\core\jackson-core\2.8.1\jackson-core-2.8.1.jar;E:\maven\repo\com\rabbitmq\amqp-client\4.1.0\amqp-client-4.1.0.jar;E:\maven\repo\org\nutz\nutz-integration-rabbitmq\1.r.68.v20191031\nutz-integration-rabbitmq-1.r.68.v20191031.jar;E:\maven\repo\bouncycastle\bcprov-jdk16\140\bcprov-jdk16-140.jar;E:\maven\repo\com\itextpdf\barcodes\7.0.7\barcodes-7.0.7.jar;E:\maven\repo\com\itextpdf\font-asian\7.0.7\font-asian-7.0.7.jar;E:\maven\repo\com\itextpdf\forms\7.0.7\forms-7.0.7.jar;E:\maven\repo\com\itextpdf\hyph\7.0.7\hyph-7.0.7.jar;E:\maven\repo\com\itextpdf\io\7.0.7\io-7.0.7.jar;E:\maven\repo\com\itextpdf\kernel\7.0.7\kernel-7.0.7.jar;E:\maven\repo\com\itextpdf\layout\7.0.7\layout-7.0.7.jar;E:\maven\repo\com\itextpdf\pdfa\7.0.7\pdfa-7.0.7.jar;E:\maven\repo\com\itextpdf\sign\7.0.7\sign-7.0.7.jar;E:\maven\repo\com\itextpdf\itextpdf\5.5.0\itextpdf-5.5.0.jar;E:\maven\repo\com\itextpdf\itext-asian\5.2.0\itext-asian-5.2.0.jar;E:\maven\repo\org\apache\httpcomponents\httpclient\4.2.1\httpclient-4.2.1.jar;E:\maven\repo\org\apache\httpcomponents\httpcore\4.2.1\httpcore-4.2.1.jar;E:\maven\repo\org\eclipse\jetty\jetty-util\9.3.7.v20160115\jetty-util-9.3.7.v20160115.jar;E:\maven\repo\org\projectlombok\lombok\1.16.10\lombok-1.16.10.jar" com.js.test.conponent.DrawTest
print WinForm(WINDOWS窗口)
print Picture(LOGO图片)
print Button(登录)
print Button(注册)
print Frame(FRAME1)
print Label(用户名)
print TextBox(文本框)
print Label(密码)
print PasswordBox(密码框)
print CheckBox(复选框)
print TextBox(记录用户名)
print LinkLabel(忘记密码)


public class DrawTest {
    public static void main(String[] args) {
        winForm().print();
    }
    public static Draw winForm() {
        WinForm winForm=new WinForm("WINDOWS窗口");
        winForm.addDraw(new Picture("LOGO图片"));
        winForm.addDraw(new Button("登录"));
        winForm.addDraw(new Button("注册"));
        Frame frame1=new Frame("FRAME1");
        frame1.addDraw(new Label("用户名"));
        frame1.addDraw(new TextBox("文本框"));
        frame1.addDraw(new Label("密码"));
        frame1.addDraw(new PasswordBox("密码框"));
        frame1.addDraw(new CheckBox("复选框"));
        frame1.addDraw(new TextBox("记录用户名"));
        frame1.addDraw(new LinkLabel("忘记密码"));
        winForm.addDraw(frame1);
        return winForm;
    }
}


public interface Draw {
    public abstract void print();
}
​




public class WinForm implements Draw{
​
​
    private String name;
​
    private Vector<Draw> drawVectors=new Vector<>();
​
    public WinForm(String name){
        this.name=name;
    }
​
    public void addDraw(Draw draw){
        drawVectors.add(draw);
    }
​
    @Override
    public void print() {
        System.out.println("print "+this.getClass().getSimpleName()+"("+name+")");
        for(Draw each:drawVectors){
            each.print();
        }
    }
}


public class Frame extends WinForm{
​
public Frame(String name) {
        super(name);
    }
}


//其它类，都类似
public class Button implements Draw{
​
    private String name;
    public Button(String name){
        this.name=name;
    }
​
    @Override
    public void print() {
        System.out.println("print "+this.getClass().getSimpleName()+"("+name+")");
    }
}












请大家批评指正！