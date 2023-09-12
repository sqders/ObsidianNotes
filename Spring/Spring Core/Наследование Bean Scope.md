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
В месте объявления бина то есть в конфиге мы можем обратиться прямиком к протатайпу мина колор и он соответственно не будет наследовать синглтон бина ColorFrame
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