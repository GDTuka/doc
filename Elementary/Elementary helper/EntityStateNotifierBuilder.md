## EntityStateNotifierBuilder

Виджет для отображение объектов [[EntityState]].

Стоит отметить что data в Builder может быть null. Причина этому следующая. EntityStateNotifier не требует сразу при создании вызвать одно из возможных состояний. Поэтому может быть ситуация в которой нам уже нужно отрендерить виджеты экрана а никакого состояния у EntityStateNotifier не вызвано. В таких случаях сработает builder: с null значением.

```dart 

EntityStateNotifierBuilder<String>(
	// Видет ошибки
    errorBuilder: (context, e, data) => const SomeError(),
    // Виджет загрузки
    loadingBuilder: (context, data) => const SomeLoading(),
    // Основной виджет
    builder: (context, String? data) {
    return SomeWidget();
    }
)
```