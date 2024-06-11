
try catch конструкция - конструкция для отлавливания ошибок.

Если в какой-то функции ошибка не должна быть обработана следует использовать rethow;

## Rethow

```dart
Future<void> someAsyncRepositoryFunction()async{

	try{
	return _someRDS.getSomeData();
	} on Exception{
	rethrow;
	}
}
```

В месте в котором будет обрабатываться ошибка следует использовать немного другую конструкцию а также выводить stackTrace вместе с тем чтобы обработать эту ошибку

## Валидация ошибок

```dart
Future<void> _loadData()async{

	try{
	final res = await model.getData();
	} on SomeSpecificExeption catch(e,stackTrace){
	
	// Отобразить конкретную ошибку
	} on AnyOtherExeption catch(e,stackTrace){
	// Отобразить любую другую конкретную ошибку
	} finally {
	// Сделать какие-то общие действия которые будут делаться при всех ошибках
	// К примеру отобразить попап или что-то такое
	}
}
```

## Собственные ошибки

Собственные ошибки желательно создавать редко и хорошо документировать как где и при каких обстоятельствах они будут использоваться.

Пример когда следует использовать собственную ошибку это когда сервер возвращает какую-то конкретную инфу при ошибке.

### Не генерируемый код

В случае когда запросы описаны не через генерируемый код можно использовать такую логику.

```dart
@JsonSerializable()
class MyExcepction implements Exeption{
MyException({
required this.errorName,
required this.someOtherDataAboutError,
});

String errorName;
String someOtherDataAboutError;

factory MyException.fromJson(Map<String,dynamic> json) => _$FromJson(json);

}

Future<void> makeNewtworkRequest()async{
	final res = await http.post(url);
	if(res.statusCode == 400) throw MyExeption();
}
```

### Генерируемый код

Когда запросы в сеть делаются через генератор кода следует использовать такую логику.

```dart


Future<void> someRepositoryFunction()async{

try{
final res = await _rds.makeNewtworkRequest();
} on Exception catch(e,stackTrace){
i
}

}
```