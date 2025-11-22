# Sentiment Analysis Microservice

Микросервис для анализа тональности текста, написанный на **Java 17 + Spring Boot**.  
Использует кастомную JSON-модель (`sentiment-model.json`), содержащую веса слов и bias.

---

## Возможности

- Анализ тональности текста (positive / neutral / negative)
- Кастомная модель в `resources/model/sentiment-model.json`
- REST API
- Docker-образ
- Kubernetes-манифесты

---

## Структура проекта

```
src/
 └─ main/
     ├─ java/com/session_task/
     │   ├─ SentimentApplication.java
     │   ├─ SentimentController.java
     │   ├─ inference/
     │   │    ├─ SentimentModel.java
     │   │    └─ SentimentModelLoader.java
     │   └─ service/
     │        ├─ SentimentService.java
     │        └─ SentimentResult.java
     └─ resources/
          ├─ application.yml
          └─ model/sentiment-model.json
```

---

## Как работает анализ

1. Текст разбивается на слова.
2. Каждому слову ищется вес в модели.
3. Сумма весов + bias = итоговый score.
4. На основе score определяется label:
   - `>  0.2` → `positive`
   - `< -0.2` → `negative`
   - `иначе` → `neutral`

---

## REST API

### `POST /api/sentiment`

**Запрос:**
```json
{
  "text": "I love this product"
}
```

**Ответ:**
```json
{
  "score": 1.32,
  "label": "positive"
}
```

---

## Docker

### Сборка:
```bash
docker build -t sentiment-service .
```

### Запуск:
```bash
docker run -p 8080:8080 sentiment-service
```

---

## Kubernetes

Манифест находится в `k8s/k8s.yaml`.

Деплой:
```bash
kubectl apply -f k8s/k8s.yaml
```

---

## Кастомизация модели

Файл модели лежит здесь:

```
src/main/resources/model/sentiment-model.json
```

Можно менять:
- список слов,
- веса,
- bias.

Приложение автоматически подхватит модель при следующем запуске.