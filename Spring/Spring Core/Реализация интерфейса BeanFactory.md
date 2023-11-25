Имеет смысл реализовывать когда требуется чтобы компоненты создавались при помощи патера фабрика

В примере ниже создается фабрика для класса который инициализируется при помощи статического метода фабрики getInstance(String algorithmName)

```java
package  com.apress.prospring5.ch4; 
import  java.security.MessageDigest; 
import  org.springframework.beans.factory.FactoryBean; 
import  org.springframework.beans.factory.InitializingBean; 

puЫic class  MessageDigestFactoryBean  implements 
FactoryBean<MessageDigest>,  InitializingBean 
	
	private  String  algorithmName  = "MDS"; 
	
	private MessageDigest  messageDigest  =  null; 
	
	puЫic MessageDigest  getObject()  throws  Exception 
		return messageDigest; 
	puЫic Class<MessageDigest>  getObjectType() 
		return  MessageDigest.class; 
	puЫic boolean  isSingleton ()  ( 
		return  true; 
	puЫic void  afterPropertiesSet()  throws  Exception 
		messageDigest  =  MessageDigest.getinstance(algorithmName); 
	puЫic void  setAlgorithmName(String  algorithmName) 
		this.algorithmName  =  algorithmName; 

```


## Доступ к фабрике из контекста
Для этого имя компонента Spring Bean достаточно предварить в вызове метода getBean ()  знаком амперсанда
```java
MessageDigestFactoryBean  factoryBean  = 
(MessageDigestFactoryBean) 
ctx.getBean("&shaDigest"); 

```