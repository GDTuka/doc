MVVM (Elementary) - Библиотека предоставляющая возможность разделить экран на 3 слоя

Model - Слой который общается с domain

ViewModel - Слой который ответственен за всю логику экрана

View - Слой который содержит в себе только представление.

При создании экрана через Elementary используется определённых [[Elementary code style]]

Для более удобной работы с Elementary используются специальные виджеты. 
Про них написано здесь [[Elementary helper]]

---

## Model

Такой вид конструкции слоя Model позволяет в будущем слою WidgetModel зависеть от абстракцией а не конкретных реализаций

```dart
// IsomeClassScreenModel - Представляет собой абстрактрый интерфейс. 
// Наследуется от ElementaryModel который содержит в себе некоторые базовые методы
// Данные методы можно при необходимости переопределить
abstract interface class ISomeScreenModel extends ElementaryModel {
	// Одна из функций описанная в абстрактном интерфейсе.
	// Позже она будет использована в WidgetModel для загрузки данных
	Future<void> loadSomeData();
}

// SomeScreenModel - Реализация IsomeClassScreenModel
// В нём будет находиться реализация методов описанных в абстрактоном интерфейса
class SomeScreenModel extends ICheckoutScreenModel {
	SomeScreenModel(
	this._someRepository
	);
// Какой-то репозиторий который содержит в себе метод с помощью которого можно 
// обратиться к API и получить информацию
SomeRepository _someRepository;

@override()
Future<void> loadSomeData(){
	try{
		final res = await _someRepository.loadData();
		return res;
	} on Exception catch(e){
		throw Exception(e);
	}
}
```

---

## Widget model

[[Async Dependency]]

```dart
// Абстрактный интерфейс в котором будут описаны все методы и объекты.
// Которые необходимы для работы интерфейса.
abstract interface class ISomeScreenWidgetModel implements IWidgetModel {

}

// Конструктор WidgetModel. 
// Возвращает экземплеяр WidgetModel который будет использоваться View для 
// Rорректной работы
// Имеет доступ к полноценному контексту
// 
SomeScreenWidgetModel defaultSomeScreenWidgetModelFactory(BuildContext context) {
  return SomeScreenWidgetModel(
    SomeScreenModel(
    // В данном случае использует AsyncDependency
    context.dep.someRepository,
    ),
  );
}

// Класс в котором будет реализована вся логика
class SomeScreenWidgetModel extends WidgetModel<SomeScreen, ISomeScreenModel>
    implements ISomeScreenWidgetModel {
  CheckoutScreenWidgetModel(super.model);
  }


```

---
## View

```dart
// Сам виджет, не смотря на то что наследуется от ElementaryWidget 
// Может быть использован как обычный виджет в любом месте приложения
class CheckoutScreen extends ElementaryWidget<ICheckoutScreenWidgetModel> {
  const CheckoutScreen({
    super.key,
    // defaultCheckoutScreenWidgetModelFactory Возвращает созданный экземпляр
    // WidgetModel необходимый для работы ElementaryWidget
  }) : super(defaultCheckoutScreenWidgetModelFactory);
  
	// ICheckoutScreenWidgetModel wm в аргументах функции нужен потому что
	// Прямого доступа к контексту у экрана нет. Все методы необходимые
	// Для работы бизнесс логики можно использовать как wm.someMethod()
  @override
  Widget build(ICheckoutScreenWidgetModel wm) {
  return Container()
  }
}
```