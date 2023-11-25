https://youtu.be/cou_qomYLNU?list=TLPQMTEwOTIwMjM89koiUFjZwA&t=1835

```java
@Component
public abstract class ColorFrame extends JFrame
{
	public ColorFrame(){
			setSize(200,200);
			setVisible(true);
			setDefaultCloseOperation(WindowsConstants.EXIT_ON_CLOSE);
		}
	
	public void showOnRandomPlace(){
		Random random = new Random();
			setLocation(random.nextInt(1200), random.nextint(900));
			getContentPlane().setBackground(getColor());
			repaint();
		}

	protected Color getColor();
}
```
В месте объявления бина то есть в конфиге мы можем обратиться прямиком к протатайпу бина колор и он соответственно не будет наследовать синглтон бина ColorFrame
```java
@Configuration
@ComponentScan(basePackages = "screensaver")
public class Config{
	@Bean
	@Scope("prototype")
	public Color color(){
		Random random = new Random();
		return new Color(random.nextInt(255),random.nextInt(255),random.nextInt(255));
	}

	@Bean 
	public ColorFrame frame(){
		return new ColorFrame(){
			@Override 
			protected Color getColor{
				return color()
			}
		};
	}

	public static void main(){
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
		while (true){
			context.getBean(ColorFrame.class).showOnRandomPlace();
			
		}
 	}
}
```
Короче чтобы сделать Inject prototype в singleton так чтобы он постоянно менялся при каждом его вызове, мы делаем абстрактный метод, который реализуем во время задания бина, посредством вызова бина который мы хотим внедрить. То есть внедрение происходит в контексте где мы описываем бины к примеру класс конфигурации и уже вызывая этот метод куда мы внедрили бин мы получаем его в компоненте. 

### Также можно это реализовать через аннотацию @Lookup
```java
public abstract class StudentServices {
 
    private Map<String, SchoolNotification> notes = new HashMap<>();
 
    @Lookup
    protected abstract SchoolNotification getNotification(String name);

    public String appendMark(String name, Integer mark) {
        SchoolNotification notification
          = notes.computeIfAbsent(name, exists -> getNotification(name)));
        return notification.addMark(mark);
    }
}
```
Несмотря на универсальность @Lookup, существует несколько заметных ограничений:

- Методы с аннотацией @Lookup, такие как getNotification, должны быть конкретными, когда окружающий класс, например Student , сканируется с помощью компонентов. Это связано с тем, что сканирование компонентов пропускает абстрактные компоненты.
- Методы с аннотациями @Lookup вообще не будут работать, если окружающий класс управляется @Bean.
В этих обстоятельствах, если нам нужно внедрить прототип bean в синглтон, мы можем обратиться к Provider в качестве альтернативы.