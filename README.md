# Poslednaa-praktika
# Бикентаева Сауле ПИ-430Б
# Цель работы
Изучение дополнительных возможностей Docker для управления жизненным циклом контейнеров, мониторинга их состояния и оптимизации использования системных ресурсов. Данная практика направлена на глубокое понимание операционных аспектов контейнеризации, что критически важно для построения надежных и масштабируемых приложений в production-среде.
# Подготовка окружения
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/1.png)
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/1.1.png)
# Задание 1: Вывод логов в файл
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/2.1.png)
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/2.2.png)
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/2.3.png)
Dockerfile
```
# Используется базовый образ на основе Alpine Linux для минимизации размера
FROM alpine:3.18

# Установка необходимых утилит для работы практики
RUN apk add --no-cache \
    bash \
    curl \
    htop \
    procps

# Создание пользователя без привилегий администратора для повышения безопасности
RUN addgroup -g 1000 appuser && \
    adduser -D -u 1000 -G appuser appuser

# Установка рабочей директории
WORKDIR /home/appuser

# Копирование скрипта логирования в контейнер
COPY --chown=appuser:appuser entrypoint.sh /home/appuser/

# Предоставление прав на выполнение скрипта
RUN chmod +x /home/appuser/entrypoint.sh

# Переключение на непривилегированного пользователя
USER appuser

# Точка входа контейнера - выполняет логирование и мониторинг
ENTRYPOINT ["/home/appuser/entrypoint.sh"]

# Значение по умолчанию - время работы в секундах
CMD ["60"]
```
Скрипт entrypoint.sh
```
#!/bin/bash

DURATION=${1:-60}

echo "====== Docker Practice Container ======"
echo "Start time: $(date)"
echo "Duration: ${DURATION} seconds"
echo "========================================"

# Вывод информации о системе
echo "System Information:"
uname -a
echo "Available Memory: $(free -h | grep Mem)"
echo "CPU Info:"
nproc

# Генерируем логи и нагрузку на ресурсы
counter=0
while [ $counter -lt $DURATION ]; do
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] Iteration $((counter + 1))/$DURATION"
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] Memory usage: $(ps aux | awk '{sum+=$6} END {print sum/1024 " MB"}')"
    
    # Имитация работы приложения
    sleep 5
    counter=$((counter + 5))
done

echo "====== Container Shutdown ======"
echo "End time: $(date)"
echo "Container completed successfully"
```
# Задание 2: Проверка docker-stats
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/3.1.png)
# Задание 3: Ограничение ресурсов
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/4.1.png)
# Задание 4: Экспорт в tar
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/5.1.png)
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/5.2.png)
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/5.3.png)
#Задание 5: Импорт из ta
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/Снимок%20экрана%202025-12-25%20181657.png)
![photo](https://github.com/SauleBikentaevaPI330B/Poslednaa-praktika/blob/main/6.1.png)
