## EntityState

EntityState - объект который содержит в себе 3 функции. Все они нужны для работы с EntityStateNotifierBuilder(). Каждая из них вызывает отдельный билдер этого виджета.

- loading() - Вызывает билдер загрузки
- error() - Вызывает билдер ошибки
- content() - Вызывает основной билдер когда вся нужная информация есть

Пример использования

```dart
Future<void> _loadSomeData()async{
// Вызывает билдер загрузки у EntityStateNotifierBuilder
_someEntity.loading();

try{
	final res = model.loadData();
	// Вызывает основной билдер у EntityStateNotifier
	_someEntity.content(res);
} on Exception catch(e){
	// Вызывает билдер ошибки у EntityStateNotifier
	_someEntity.error(e);
}
}
```

Стоит отметить что loading() можно передать некую информацию которую можно использовать при постройке виджета. Точно также как в error() можно передать не только ошибку. Подробнее посмотреть в самом классе EntityState.

Используется данный вариант конструкции редко но может когда-то и пригодиться.

Примеры использования

```dart
_someEntity.loading(oldData);

_someEntity.error(e,oldData);
```