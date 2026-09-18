# Profile B — Unified Interactive Prototype

Один автономный `index.html` с единой точкой входа и двумя представлениями одной проверки:

- **Operator** — минимальный интерфейс сотрудника;
- **Verifier** — тот же поток с техническими evidence / metrics / trace.

Прототип поддерживает два источника:

- **Live Camera** — реальный webcam-поток через `getUserMedia()`;
- **Presentation Mode** — явно маркированный simulated fallback, если доступ к камере не разрешён или Camera API недоступен.

Никакие внешние CSS, JS, изображения, CDN или backend не используются.

## Запуск

Отдайте `index.html` через обычный HTTP(S)-сервер и откройте страницу в браузере.

Для доступа к webcam рекомендуется HTTPS; `localhost` также обычно считается secure context браузером.

Точка входа одна:

```text
/index.html
```

На стартовом экране можно выбрать роль.

Прямые ссылки:

```text
/index.html#operator
/index.html#verifier
```

## Поток демонстрации

1. Открыть **Operator** или **Verifier**.
2. По умолчанию запускается **Presentation Mode**.
3. Нажать **Live Camera**, чтобы запросить доступ к webcam.
4. Если камера доступна, интерфейс переключается на реальный поток.
5. Если доступ запрещён или API недоступен, прототип автоматически возвращается в **Presentation Mode**.
6. Нажать **New challenge**, чтобы выбрать новый spatial challenge:
   - `MOVE RIGHT →`;
   - `← MOVE LEFT`;
   - `MOVE CLOSER`.

## Operator mode

Показывает только основные признаки проверки:

- Camera / feed;
- Phone screen detected;
- QR detected;
- Face detected;
- Challenge response;
- итоговый `PASS / FAIL`.

Технические evidence и trace скрыты.

## Verifier mode

Показывает тот же verification flow, но дополнительно выводит:

- cross-signal consistency;
- phone evidence;
- face evidence;
- QR state;
- motion delta;
- event trace.

Operator и Verifier — два UI-представления одной логики, а не два отдельных прототипа.

## Presentation Mode

Это fallback для презентации UX и state machine без реальной webcam/CV-зависимости.

На экране анимируется человек, который поднимает телефон; затем последовательно обновляются статусы:

```text
phone candidate
→ phone detected
→ QR valid
→ face detected
→ challenge passed
→ cross-signal consistency
→ PASS
```

Режим всегда явно помечен:

```text
PRESENTATION MODE · SIMULATED FEED
```

Его статусы являются сценарием демонстрации и не выдаются за результат реального CV.

## Live Camera

При наличии доступа используется:

```js
navigator.mediaDevices.getUserMedia(...)
```

Прототип пытается использовать browser-native APIs:

- `BarcodeDetector` для QR;
- `FaceDetector` для face presence.

Поддержка этих API зависит от браузера.

Ожидаемое demo-значение QR:

```text
PROFILE-B-DEMO|STATIC-QR|V1
```

После первого обнаружения QR его bounding box используется как baseline для spatial challenge. Проверяется горизонтальное смещение или увеличение площади QR при приближении телефона.

## Что именно доказывает прототип

Это **interactive system / UX prototype + browser CV spike** для Profile B.

Он демонстрирует:

```text
single entry point
+
Operator / Verifier views
+
real webcam path when available
+
explicit simulated fallback
+
QR / face browser detection attempts
+
spatial challenge state
+
evidence/status flow
```

Это ещё не production KYC verifier: browser-native detectors и статический demo QR используются только как минимальная реализация интерфейса и CV-потока.
