# Profile B — Interactive Prototype

Один автономный HTML-файл с единой точкой входа и двумя режимами:

- **Operator**
- **Verifier**

## Запуск

Откройте:

```text
index.html
```

Появится экран выбора режима.

Можно зайти напрямую:

```text
index.html#operator
index.html#verifier
```

Никакие внешние CSS, JS, изображения, CDN или backend не используются.

## Operator mode

Показывает сотруднику:

- simulated webcam;
- ключевые статусы;
- текущий challenge;
- итоговый `PASS / FAIL`.

Это презентационный интерфейс без технических деталей CV.

## Verifier mode

Использует тот же сценарий и состояние, но дополнительно показывает:

- detection overlays;
- confidence / evidence metrics;
- QR state;
- motion delta;
- cross-signal consistency;
- event trace.

Таким образом Operator и Verifier — два представления одной проверки, а не два отдельных прототипа.

## Сценарии

Встроены четыре сценария:

1. **Legit user**
   - phone detected;
   - QR valid;
   - face detected;
   - challenge passed;
   - `PASS`.

2. **Replay attack**
   - телефон/экран наблюдается;
   - QR выглядит присутствующим;
   - live challenge-response отсутствует;
   - `FAIL`.

3. **Wrong QR**
   - телефон и лицо обнаружены;
   - QR mismatch;
   - `FAIL`.

4. **Challenge failed**
   - QR valid;
   - лицо обнаружено;
   - движение телефона не соответствует challenge;
   - `FAIL`.

## Что это за тип прототипа

Это **interactive system / UX prototype**.

Сценарии анимированы и статусы синхронизированы логикой JavaScript, но визуальный поток не поступает из реальной камеры и CV-модель не выполняет реальную детекцию.

Поэтому это:

```text
не просто слайд
не видео
не технический CV PoC
```

а интерактивный прототип поведения будущей системы.

Позже simulated webcam можно заменить на реальный `getUserMedia()` и подключить настоящий QR / face / motion verifier, не меняя основную структуру интерфейса.
