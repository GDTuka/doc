
Async Dependency - специальный класс для асинхронной инициализации зависимостей.
Его главная задача предоставить асинхронную инъекцию зависимостей. Работает за счёт Provider.

[[Async dependency source code]] - Здесь находится сурс код

Для создания класса асинхронной зависимости необходимо создать объект и унаследовать его от AppAsyncDependency

## Создание скопа

Обязательным условием создания объекта будет переопределение initAsync в которой происходит инициализация зависимостей

```dart
class SomeScope extends AppAsyncDependency{

@override
Future<void> initAsync(Buildcontext context)async{

}
}
```

Для использования следует использовать AppDependencyWidget в коде программы. Все виджеты которые будут являться дочерними определённому скопу получат доступ ко всем его данным

```dart
  return AsyncDependencyWidget<GlobalDependency>(
      create: SomeScope.new,
      // Представляет собой виджет который будет отображаться пока не выполнится метод initAsyn c
      loaderBuilder: (context) => Container(),
      // Отображает виджет который появится в случае ошибки при выполнение метоыд initAsync
      errorBuilder: (context) => Container(),
      child: const App(),
    ),
```

## Инъекция зависимостей

Для создания зависимости необходимо объявить переменную внутри скопа и присвоить ей значение в initAsync.

При этом не обязательно присваивать значение именно там, но для соблюдения единого код стайла желательно

```dart
class SomeScope extends AppAsyncDependency{

late final SomeRDS someRDS;

late final SomeRepository someRepository;

@override
Future<void> initAsync(Buildcontext context)async{

	someRDS = SomeRDS();
	
	someRepository = SomeRepository(someRDS);
}

}
```

## Примеры использования

### Функции

В Виджете асинхронной зависимости можно использовать функции также которые будут выполнять определённую логику также как и везде

```dart
@override
Future<void>initAsync(BuildContext context)async{
	_makeSomeThing();
}

Future<void> _makeSomeThing()async{
// Функция что-то делает
}

```

Также можно прослушивать тот же bloc чтобы была возможность как-то реагировать на его события

```dart
@override
Future<void>initAsync(BuildContext context)async{
	someBloc = SomeBloc();

	someBloc.stream.listen(_onBlocEvent);
}

Future<void> _onBlocEvent(SomeBlocEvent event)async{
// Функция что-то делает при определённом событии
}
```
### Ошибки

В случае выполнения асинхронных запросов необходимо которые могут вызвать ошибки их необходимо оборачивать в блок [[try catch]]. Если в catch вызывать ошибку методом throw Exception то виджет асинхронной зависимости вернёт ошибку а не продолжит выполнение кода.

```dart
@override
Future<void>initAsync(BuildContext context)async{
	someRepository = SomeRepository();

	try{
		await someRepository.initUser();
	} on Object catch (e, stackTrace) {
      Error.throwWithStackTrace(e, stackTrace);
    }
}
```

### Получение данных из скопа

Чтобы данные из скопа было комфортно получать необходимо дописать extension

```dart
extension DepContextExtension on BuildContext {
  SomeScope get global => read<SomeScope>();
}
```

Таким образом получить данные из скопа можно будет следующим образом

```dart
context.global.someData
```





