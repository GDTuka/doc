## DoubleEntityStateNotifierBuilder

Представляет собой такой же виджет как и [[EntityStateNotifierBuilder]] только он умеет прослушивать сразу несколько объектов.

При этом в случае вызова .loading или .error у одного из объектов виджет сразу перейдёт в состояние загрузки или ошибки.

Чтобы вызвать builder необходимо вызвать сразу у обоих объектов .content

Пример:

```dart
DoubleEntityStateNotifierBuiler<List<Data>, Data>(
	// Первый прослушиваемый объект
    firstListenable: wm.firstListenable,
    // Второй прослушиваемый объект
    secondListenable: wm.secondListenable,
    // Билдер виджета ошибки
    errorBuilder: (context, firstData, secondData, firstE, secondE) => Error(),
    // Билдер виджета загрузки
    multiLoadingBuilder: (context, firstData, secondData) => Loading()
    // Просто билдер
    builder: (context, firstData, secondData) {}
    ),
```