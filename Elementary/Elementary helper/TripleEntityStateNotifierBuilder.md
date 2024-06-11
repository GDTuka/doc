## TripleEntityStateNotifierBuilder

Представляет собой такой же виджет как и [[EntityStateNotifierBuilder]] только он умеет прослушивать сразу несколько объектов.

При этом в случае вызова .loading или .error у одного из объектов виджет сразу перейдёт в состояние загрузки или ошибки.

Чтобы вызвать builder необходимо вызвать сразу у троих объектов .content

Пример:


```dart
DoubleEntityStateNotifierBuiler<List<Data>, Data>(
	// Первый прослушиваемый объект
    firstListenable: wm.firstLiestenable,
    // Второй прослушиваемый объект
    secondListenable: wm.secondListenable,
    // Билдер виджета ошибки
    thirdListenable: wm.thirdListenable
    errorBuilder: (context, firstData, secondData, thirdData, firstE, secondE, thirdE) => Error(),
    // Билдер виджета загрузки
    multiLoadingBuilder: (context, firstData, secondData,thirdData) => Loading()
    // Просто билдер
    builder: (context, firstData, secondData,thirdData) {}
    ),
```