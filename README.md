# Отчет по Практической работе №6

## 🎯 Цель работы  
Провести тестирование разработанных программных модулей с использованием средств автоматизации **Microsoft Visual Studio** методом **"белого ящика"**.

---

## 🔨 Ход выполнения работы  

### 📌 1. Создание проекта для тестирования  
✅ Создан проект **Bank**.

![Консольный вывод](/5424934799321199303.jpg) 

✅ В файле `Program.cs` изменен код в соответствии с заданием.

![Консольный вывод](/5424934799321199304.jpg) 

✅ Файл **переименован** в `BankAccount.cs`.  

![Консольный вывод](/5424934799321199305.jpg) 

✅ Запуск показал корректную работу консольного приложения.  

**Консольный вывод:**  
![Консольный вывод](/5424934799321199306.jpg)  

---

## 🛠 Создание проекта модульного теста  
✅ Создан проект **BankTests**.  

![Консольный вывод](/5424934799321199307.jpg)  

✅ Добавлены тестовые методы.  

---

## ✅ Создание и запуск тестов  

### 📌 1. Первый тест (Debit)  
**Проверка:** списание со счета работает корректно, если сумма допустима (больше нуля и меньше баланса).  

**Результат:** тест **не пройден**, так как в коде ошибка (`m_balance += amount` вместо `m_balance -= amount`).  
![Консольный вывод](/5424657000836492101.jpg)  

**Исправленный тест:**  
![Консольный вывод](/5424657000836492102.jpg)   

---

## 🔄 Улучшение кода через тестирование  
✅ Теперь метод `Debit()` корректно выбрасывает исключение `ArgumentOutOfRangeException`, если сумма:  
- превышает баланс,  
- меньше нуля.  

### 📌 2. Тест на превышение баланса  
```csharp
[TestMethod]
public void Debit_WhenAmountIsMoreThanBalance_ShouldThrowArgumentOutOfRange()
{
    // Arrange
    double beginningBalance = 11.99;
    double debitAmount = 20.00;
    BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);

    // Act
    try
    {
        account.Debit(debitAmount);
    }
    catch (System.ArgumentOutOfRangeException e)
    {
        // Assert
        StringAssert.Contains(e.Message, BankAccount.DebitAmountExceedsBalanceMessage);
        return;
    }

    Assert.Fail("The expected exception was not thrown.");
}
```
✅ Все тесты успешно пройдены:  
![Консольный вывод](/5424657000836492103.jpg)    

---

## 🔎 Тестирование метода `Credit()`  
✅ Добавлены тесты для проверки зачисления средств.  

### 📌 3. Тест на отрицательное зачисление  
```csharp
[TestMethod]
public void Credit_WhenAmountIsLessThanZero_ShouldThrowArgumentOutOfRange()
{
    // Arrange
    double beginningBalance = 11.99;
    double creditAmount = -50.00;
    BankAccount account = new BankAccount("Mr. Roman Abramovich", beginningBalance);

    // Act
    try
    {
        account.Credit(creditAmount);
    }
    catch (System.ArgumentOutOfRangeException e)
    {
        // Assert
        StringAssert.Contains(e.Message, "Credit amount cannot be negative");
        return;
    }

    Assert.Fail("The expected exception was not thrown.");
}
```
### 📌 4. Тест на корректное пополнение баланса  
```csharp
[TestMethod]
public void Credit_WhenAmountIsPositive_ShouldIncreaseBalance()
{
    // Arrange
    double beginningBalance = 11.99;
    double creditAmount = 20.00;
    BankAccount account = new BankAccount("Mr. Roman Abramovich", beginningBalance);

    // Act
    account.Credit(creditAmount);

    // Assert
    Assert.AreEqual(beginningBalance + creditAmount, account.Balance, 0.001, "Account balance was not correctly updated after crediting.");
}
```
✅ Все тесты успешно пройдены:  
![Консольный вывод](/5424657000836492105.jpg)   

---
## 📌Обозреватель тестов:
![Консольный вывод](/5424934799321199313(1).jpg) 
## 📌 **Вывод**  
Я провел тестирование разработанных программных модулей с использованием средств автоматизации **Microsoft Visual Studio**, применяя метод **"белого ящика"**.  

В ходе тестирования проверена работа методов `Debit()` и `Credit()`, включая случаи:  
✅ сумма превышает баланс,  
✅ сумма отрицательная,  
✅ баланс корректно изменяется при пополнении.  

**Все тесты успешно пройдены**, что подтверждает правильность работы методов в рамках заданных ограничений. Исключения выбрасываются корректно, а логика операций с балансом соответствует требованиям.  

📌 **Заключение**: Реализованный функционал соответствует ожидаемым результатам. ✅  
