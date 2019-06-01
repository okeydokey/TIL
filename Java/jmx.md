# JMX(Java Management eXtensions, JSR-160)
Java Management Extensions (JMX) API는 Application, Deivce, 서비스 및 JVM과 같은 자원의 관리 및 모니터링을 위한 표준 API이다.

### JMX Architecture
1. Instrumentation layer : 자원을 관리하는 JMX 에이전트에 등록 된 MBean
2. JMX agent layer : MBean의 레지스트리를 관리해, 액세스하는 인터페이스를 제공하는 코어 컴퍼넌트(MbeanServer)
3. Remote management layer : 일반적으로 JConsole과 같은 클라이언트 도구

### MBeans (managed beans)
MBean을 만들기 위한 몇가지 규칙이 필요함
- 'MBean'을 접미사로 하는 인터페이스 필요 ex) GameMbean
```java
public interface GameMBean {
 
    public void playFootball(String clubName);
 
    public String getPlayerName();
 
    public void setPlayerName(String playerName);
 
}
```
- 인터페이스 구현, 인터페이스명에서 'MBean' 제외 ex) Game
- Getter, Setter 필요
```java
public class Game implements GameMBean {
 
    private String playerName;
 
    @Override
    public void playFootball(String clubName) {
        System.out.println(
          this.playerName + " playing football for " + clubName);
    }
 
    @Override
    public String getPlayerName() {
        System.out.println("Return playerName " + this.playerName);
        return playerName;
    }
 
    @Override
    public void setPlayerName(String playerName) {
        System.out.println("Set playerName to value " + playerName);
        this.playerName = playerName;
    }
}
```

### JMX Agent
- PlatformMbeanServer : JMX 에이전트의 핵심 컴포넌트이며 MBean을 등록
- ObjectName : MBean 등록시 필요한 문자열
  - domain : 임의의 문자열을 사용할 수 있지만 MBean 네이밍 컨벤션에 따라 Java package name을 사용(네이밍 충돌 회피)
  - key : 'key=value'
```java
    MBeanServer server = ManagementFactory.getPlatformMBeanServer();
    ObjectName objectName = new ObjectName("com.okeydokey.study.jxm:type=basic,name=game");
    Game game = new Game();
    server.registerMBean(game, objectName);
```
- JMX agent 활성화 : 어플리케이션에 -Dcom.sun.management.jmxremote 설정 추가

### Jconsole
JDK_HOME/bin/jconsole 실행
ex) /Library/Java/JavaVirtualMachines/jdk1.8.0_211.jdk/Contents/Home

참고
https://docs.oracle.com/javase/6/docs/technotes/guides/jmx/index.html
https://www.baeldung.com/java-management-extensions
https://docs.spring.io/spring/docs/5.1.7.RELEASE/spring-framework-reference/integration.html#jmx
https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-metrics-export-jmx
https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-jmx.html
https://micrometer.io/docs/registry/jmx
https://dzone.com/articles/an-overview-of-jmx
https://sysdig.com/blog/jmx-monitoring-custom-metrics/

https://docs.oracle.com/javase/8/docs/technotes/guides/management/jconsole.html
