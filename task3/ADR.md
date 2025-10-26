### **Название задачи:**
Разработать концептуальную архитектуру MVP, позволяющую:

- Подавать заявку на депозит через сайт (для новых клиентов) и интернет-банк (для существующих).
- Обеспечить обработку заявок бэк-офисом без автоматизации полного цикла (в рамках MVP).
- Соблюсти требования надёжности, безопасности, производительности и ограничения интеграции с АБС.

### **Автор:**
Папян Артем Мушегович

### **Дата:**
25.10.2025

---
### **Функциональные требования**

| КОД | ТРЕБОВАНИЕ |
|:---:|------------|
| F1,F2 | Подача заявки через сайт и интернет-банк |
| F3 | Отображение актуальных ставок |
| F6 | Единая заявка со сквозным ID |

| № | Действующие лица и системы | Use Case | Описание |
| :-: | :- | :- | :- |
| 1 | Клиент, сайт | Подача заявки через сайт | Клиент вводит свои данные и отправляет заявку на депозит через сайт |
| 2 | Клиент, интернет-банк | Подача заявки через интернет-банк | Клиент вводит свои данные и отправляет заявку на депозит через интернет-банк |
| 3 | Клиент, сайт, сервис ставок | Просмотр ставок на депозиты через сайт | Клиент просматривает актуальные ставки на депозиты через сайт |
| 4 | Клиент, интернет-банк, сервис ставок | Просмотр ставок на депозиты через интернет-банк | Клиент просматривает актуальные ставки на депозиты через интернет-банк |

---

### **Нефункциональные требования**
Опишите здесь нефункциональные требования и архитектурно значимые требования.

| КОД | ТРЕБОВАНИЕ |
|:---:|------------|
| R4, DC1, P2 | Запрет прямой интеграции интернет-банка с АБС в реальном времени |
| DC2 | Отсутствие поддержки Kafka в текущем интернет-банке |
| I2 | Централизованный справочник ставок |
| IF2 | Интеграция с АБС, СМС-шлюзом, кол-центром |
| PH1 | Защита персональных данных |

---

### **Решение**

[C4 context](https://www.plantuml.com/plantuml/png/TP71QiCm38RlVWejfnOsUkbnZB9sXnqAPM7RCRYsqeavSh3KjNtxnN4B8x2JmfzFqYUy4hL9ZrqmTyGk73t2utgvM2-RTMJ5ipuPewG1eRw8OWVN63Pa3ybsqCO4suyTd4Y_m6CVXLICsM6IoBEZOBg7wdqerL1VMJ5PfrBmlULla75kDJUcR9YLOA1hjUnjKdZEN3tXcfUppdgRb36XRvmDeN78EYxStNB8-KF9g_dVLkcaELZ5axtaNTiQ-XH_o8-SUSDGvAz-vGZ9HtpY7cfIN8pNC1Q_eIdttEZ_n3RKjfYG5SGbwbN0O_xQ4n_01KBMxE5GP4WcGoVAo5uWYtDKQibPfSYIxLV3h_oclm00)

[C4 container](https://www.plantuml.com/plantuml/png/dLF1ZjCm4BtdAuQzK2GaBdj4Q3TTG9L0AkJ0qNBTQMlLiOiztgL2_3lZE9kaJf5MtEpnUyzldcRk0abFiJNGaBHRFaCjxH6QSo1iOUaiIMtHmuEMUo_RwfMsoiGjmllbxRlbhHWG7wfgZJuukuW1_LkuMHgDbcxuTAyKXg3j7ZP9EsHjEdWNzmTumWq4JGZEWxQ4tY9rm4J8s9itLPKTYTh5PFEyI0YHd_cBqdrcmUyA0COk5lAanLuIFibpyhLUmUTyunzAvCbe9KaxAmBwHwqmCSOgzDMI-IrYn4N_A3RU7Riqcbo0kYlClZEub-eiv2OmzajtWVkwcU6KnwqciSKJDzkYfHHqLuE7eSQiFqDW7t_HWYDct8ySD3kUlh4iYHGc8rRFyTSv_ajaQCCalEPhCuOtXIbfZ51i8qy1ts3PtHXMVKVJpmTxojvP8m0tvDWRJ6AV3GZpH_fW-uKYvMHTY1MMNo2N6A2cRUMGYm4R1h-O-HxIcnXw-K_-n7wfiFQkboZQA7KweGlkzGrOsKTtBEMKntmuvUzwNrSLPfjHnzqEVIdfyMV4G5bfkXvpWKz8wl0FFkoyQvyfa2bND6ThDPLLHdFbzJWGNX9U4Ids0-qsiTEUM4TwrpjBMT_nMspDNm00)

### Почему буферный сервис вместо прямых вызовов АБС?

- Соответствует ограничению DC1 (избежать прямых вызовов АБС)
- Позволяет обрабатывать пиковые нагрузки
- Обеспечивает сохранность данных при сбоях АБС

### Почему Java для сервиса депозитов?

- Существующая экспертиза в команде АБС
- Совместимость с текущими системами (Oracle, REST)
- Поддержка микросервисной архитектуры в будущем


### Почему Service Broker (MS SQL) вместо Kafka?

- Ограничение DC2 (несовместимость с текущим интернет-банком)
- Минимальные изменения в инфраструктуре (уже используется MS SQL)
- Достаточная функциональность для MVP

---

### **Альтернативы**
- JMS (Artemis и т.п.) вместо Service Broker

---

### **Недостатки, ограничения, риски**

- **Зависимость от подрядчика интернет-банка** при обновлении ядра — но MVP не требует изменений в ядре.
- **Задержка обработки заявок** — в MVP бэк-офис обрабатывает вручную, что не нарушает SLA.
- **Дублирование данных** между сервисом заявок и АБС — временно допустимо для MVP при наличии сквозного ID (F6).

---