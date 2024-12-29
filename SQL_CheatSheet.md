
# SQL Шпаргалка: Основные команды

## 1. **SELECT**  
**Описание:** Используется для выборки данных из одной или нескольких таблиц.  

**Синтаксис:**  
```sql
SELECT столбец1, столбец2, ... 
FROM таблица;
```

**Примеры:**
- `SELECT * FROM Passenger;` — выбрать все столбцы из таблицы `Passenger`.
- `SELECT name FROM Company;` — выбрать только столбец `name` из таблицы `Company`.

---

## 2. **FROM**  
**Описание:** Указывает таблицу, из которой будут извлекаться данные.  

**Синтаксис:**  
```sql
SELECT столбец1, столбец2
FROM таблица;
```

**Примеры:**
- `SELECT * FROM Trip;` — выбрать все столбцы из таблицы `Trip`.
- `SELECT town_from, town_to FROM Trip;` — выбрать города отправления и прибытия из таблицы `Trip`.

---

## 3. **JOIN**  
**Описание:** Используется для объединения строк из двух или более таблиц на основе их связей.  

### Типы JOIN:
- **INNER JOIN** — выбирает только совпадающие строки между таблицами.
- **LEFT JOIN** — выбирает все строки из левой таблицы, даже если нет совпадений в правой.
- **RIGHT JOIN** — выбирает все строки из правой таблицы, даже если нет совпадений в левой.
- **FULL JOIN** — объединяет все строки из обеих таблиц, включая строки без совпадений (не поддерживается всеми СУБД).

**Синтаксис:**  
```sql
SELECT столбцы
FROM таблица1
JOIN таблица2
ON таблица1.поле = таблица2.поле;
```

**Примеры:**
- `SELECT Trip.town_from, Company.name FROM Trip JOIN Company ON Trip.company = Company.id;`  
  — соединить таблицы `Trip` и `Company`, выбрать город отправления и название компании.

- `SELECT Pass_in_trip.place, Passenger.name FROM Pass_in_trip JOIN Passenger ON Pass_in_trip.passenger = Passenger.id;`  
  — соединить таблицы `Pass_in_trip` и `Passenger`, выбрать место в рейсе и имя пассажира.

---

## 4. **ON**  
**Описание:** Указывает условие соединения для команд `JOIN`. Обычно связывает внешний ключ одной таблицы с первичным ключом другой.  

**Синтаксис:**  
```sql
JOIN таблица2
ON таблица1.поле = таблица2.поле;
```

**Пример:**
- `JOIN Passenger ON Pass_in_trip.passenger = Passenger.id`  
  — связывает таблицу `Pass_in_trip` с таблицей `Passenger` через поле `passenger`.

---

## 5. **WHERE**  
**Описание:** Используется для фильтрации строк на основе заданных условий.  

**Синтаксис:**  
```sql
SELECT столбцы
FROM таблица
WHERE условие;
```

**Примеры:**
- `SELECT * FROM Trip WHERE town_from = 'Москва';` — выбрать все рейсы, отправляющиеся из Москвы.
- `SELECT name FROM Passenger WHERE name LIKE 'А%';` — выбрать пассажиров, имена которых начинаются на "А".

---

## 6. **GROUP BY**  
**Описание:** Группирует строки с одинаковыми значениями в столбце и позволяет применять агрегатные функции (`COUNT`, `SUM`, `AVG`, и др.).  

**Синтаксис:**  
```sql
SELECT столбец, агрегатная_функция(столбец)
FROM таблица
GROUP BY столбец;
```

**Примеры:**
- `SELECT company, COUNT(*) FROM Trip GROUP BY company;` — подсчитать количество рейсов для каждой компании.
- `SELECT passenger, COUNT(trip) FROM Pass_in_trip GROUP BY passenger;` — подсчитать количество рейсов для каждого пассажира.

---

## 7. **ORDER BY**  
**Описание:** Сортирует строки по указанным столбцам.  

**Синтаксис:**  
```sql
SELECT столбцы
FROM таблица
ORDER BY столбец [ASC | DESC];
```

**Примеры:**
- `SELECT name FROM Passenger ORDER BY name ASC;` — отсортировать имена пассажиров в алфавитном порядке.
- `SELECT * FROM Trip ORDER BY time_out DESC;` — отсортировать рейсы по времени вылета в обратном порядке.

---

## 8. **LIMIT**  
**Описание:** Ограничивает количество возвращаемых строк.  

**Синтаксис:**  
```sql
SELECT столбцы
FROM таблица
LIMIT число;
```

**Примеры:**
- `SELECT * FROM Passenger LIMIT 5;` — выбрать только 5 первых пассажиров.
- `SELECT * FROM Trip ORDER BY time_out DESC LIMIT 1;` — выбрать рейс с самым поздним временем вылета.

---

## 9. **Агрегатные функции**

### **COUNT**  
Подсчитывает количество строк.  
```sql
SELECT COUNT(*) FROM Trip;
```

### **SUM**  
Суммирует значения столбца.  
```sql
SELECT SUM(цена) FROM Билеты;
```

### **AVG**  
Вычисляет среднее значение.  
```sql
SELECT AVG(вес) FROM Багаж;
```

### **MAX / MIN**  
Находит максимальное или минимальное значение.  
```sql
SELECT MAX(цена) FROM Trip;
SELECT MIN(вес) FROM Багаж;
```

---

## Пример комплексного запроса
```sql
SELECT Passenger.name, COUNT(Pass_in_trip.trip) AS total_trips
FROM Passenger
JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
GROUP BY Passenger.name
ORDER BY total_trips DESC
LIMIT 5;
```

**Что делает:**
1. Выбирает имя пассажира (`Passenger.name`).
2. Считает количество рейсов для каждого пассажира (`COUNT(Pass_in_trip.trip)`).
3. Группирует результаты по пассажирам (`GROUP BY Passenger.name`).
4. Сортирует пассажиров по количеству рейсов в порядке убывания (`ORDER BY total_trips DESC`).
5. Ограничивает результат до 5 строк (`LIMIT 5`).

---

Эта шпаргалка поможет легко вспомнить ключевые команды SQL и их применение.
