Интерфейс редакторов свойств служат тому чтобы строковые значения преобразовать в сложные типы Java и наоборот.

Стандартные редакторы Spring
![[Pasted image 20231124122229.png]]

# Создание специального редактора свойств 
Давайте сначала создадим класс модели CreditCard , определяющий поля rawCardNumber, идентификационный номер банка (первые 6 цифр), номер счета (цифры от 7 до 15) и контрольный код (последняя цифра):
```java
public class CreditCard {

    private String rawCardNumber;
    private Integer bankIdNo;
    private Integer accountNo;
    private Integer checkCode;

    // standard constructor, getters, setters
}
```

Далее мы создадим класс CreditCardEditor . Это реализует бизнес-логику для преобразования номера кредитной карты, заданного в виде строки , в объект CreditCard .

Класс редактора свойств должен расширять PropertyEditorSupport и реализовывать методы getAsText() и setAsText() :

public class CreditCardEditor extends PropertyEditorSupport {
```java
    @Override
    public String getAsText() {
        CreditCard creditCard = (CreditCard) getValue();
        
        return creditCard == null ? "" : creditCard.getRawCardNumber();
    }
    
    @Override
    public void setAsText(String text) throws IllegalArgumentException {
        if (StringUtils.isEmpty(text)) {
            setValue(null);
        } else {
            CreditCard creditCard = new CreditCard();
            creditCard.setRawCardNumber(text);
            
            String cardNo = text.replaceAll("-", "");
            if (cardNo.length() != 16)
                throw new IllegalArgumentException(
                  "Credit card format should be xxxx-xxxx-xxxx-xxxx");
            
            try {
                creditCard.setBankIdNo(Integer.valueOf(cardNo.substring(0,6)));
                creditCard.setAccountNo( Integer.valueOf(
                  cardNo.substring(6, cardNo.length() - 1)) );
                creditCard.setCheckCode( Integer.valueOf(
                  cardNo.substring(cardNo.length() - 1)) );
            } catch (NumberFormatException nfe) {
                throw new IllegalArgumentException(nfe);
            }
            
            setValue(creditCard);
        }
    }
}
```

Метод getAsText() вызывается при сериализации объекта в строку, а setAsText() используется для преобразования строки в другой объект.