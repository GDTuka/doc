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