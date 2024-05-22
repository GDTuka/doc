Общие требования к кодстайлу, чтобы можно было писать код одинаково.

# Документация

## Объекты и методы в абстрактном интерфейса

Документация объектов происходит в интерфейсах. В них через "///" нужно описать зачем нужна какая-то функция / объект или что бы это ни было.

```dart
absctract interface class ISomeWidgetModel extends IWidgetModel {
/// Документация к тому зачем он нужен
ValueNotifier<String> get someNotifier;
}
```

**Документировать объекты которые переопределяются из интерфейса в реализации не нужно**

---

## Инкапсулированные методы и объекты в реализации

Если какой-то метод или объект является инкапсулированным в реализации WidgetModel к нему применяются те же правила документации что и к объектам в абстрактном интерфейсе.

```dart
/// Здесь документация к тому зачем нужен этот метод
Future<void> _loadData()async{
try{
	final res = await model.getData();
	someEntity.content(res);
} on Exception catch(e) {
	someEntity.content(e);
}
}

```

---

## Комментарии

Комментарии к коду оставляются через "//".

Комментарий нужен чтобы как конкретно работает код и что он делает. 

**Комментарии != документация.**

### Когда нужно комментировать код

- Когда требует пояснений по поводу того почему оно работает именно так.
- Комментарий стоит писать в случае если реализовывается конченная бизнес логика. 
- Когда ниже написан сложный длинный метод который занимается чем-то стандартным но он большой

Пример комментирования кода.

Ниже пример большого и сложного метода который является таковым из-за особенностей бэкенда. Комментариями по пунктам помечено что и зачем нужно, что делает то или иное условие.

```dart
  void _initToppigns() {
    // toppings that user has picked
    final selectedToppings = widget.selectedToppings;
    // List of topping that will be displayed on screen
    final toppingGroupsLocal = <ToppingGroupsLocal>[];

    if (widget.toppingPrice != null && !Flavors.isBelarus) {
      // double cycle to search in list of toppings price and all topping groups
      for (int j = 0; j < widget.toppingsGroups.length; j++) {
        final updateToppings = <ToppingGroupsItemLocal>[];

        // provide all topping from group price
        for (int k = 0; k < toppingGroups[j].toppings.length; k++) {
          ToppingGroupsItemLocal topping = toppingGroups[j].toppings[k].copyWith(price: widget.toppingPrice!);

          // check if topping was added before to update quantity to correct one
          for (final selectedTopping in selectedToppings) {
            if (selectedTopping.id == toppingGroups[j].toppings[k].id) {
              // privde corerct quantity
              topping = topping.copyWith(quantity: selectedTopping.quantity);
            }
          }
          // add topping with price and quantity to topping list that will be displayed in UI
          updateToppings.add(topping);
        }
        // add topping group with new price to local topping group that will be used to display toppings
        toppingGroupsLocal.add(widget.toppingsGroups[j].copyWith(toppings: updateToppings));
      }

      // set new toppings value
      _toppingEntity.content(toppingGroupsLocal);
    } else {
      // Price for toppings this pizza this picked size
      final toppingGroupPrice = widget.product.selectedSize.toppingGroups;
      // double cycle to search in list of toppings price and all topping groups
      for (int i = 0; i < toppingGroupPrice!.length; i++) {
        for (int j = 0; j < widget.toppingsGroups.length; j++) {
          // check if same group
          if (toppingGroupPrice[i].toppingGroup == widget.toppingsGroups[j].id) {
            final updateToppings = <ToppingGroupsItemLocal>[];

            // provide all topping from group price
            for (int k = 0; k < toppingGroups[j].toppings.length; k++) {
              ToppingGroupsItemLocal topping = toppingGroups[j].toppings[k].copyWith(price: toppingGroupPrice[i].price);

              // check if topping was added before to update quantity to correct one
              for (final selectedTopping in selectedToppings) {
                if (selectedTopping.id == toppingGroups[j].toppings[k].id) {
                  // privde corerct quantity
                  topping = topping.copyWith(quantity: selectedTopping.quantity);
                }
              }
              // add topping with price and quantity to topping list that will be displayed in UI
              updateToppings.add(topping);
            }
            // add topping group with new price to local topping group that will be used to display toppings
            toppingGroupsLocal.add(widget.toppingsGroups[j].copyWith(toppings: updateToppings));
          }
        }
      }

      // set new toppings value
      _toppingEntity.content(toppingGroupsLocal);
    }
  }
```



# Расположение методов и объектов в классах

Располагать объекты следует следующим образом 

**В интерфейсе**

1.  Нотифаеры
2.  Контроллеры
3. Синхронные функции
4. Асинхронные функции
5. Различные геттеры

**В реализации**

1.  Геттеры
2.  Инкапсулированные объекты для нотифаеров
3. Нотифаеры в которых им присваиваются инкапсулированные нотифаеры 
4. Инкапсулированные контроллеры
5. Переопределённые контроллеры и присваение им значений
6. Функции необходимые для работы контроллера, к примеру валидаторы
7. Функции которые срабатывает при прослушивании какого-либо нотифаера
8. initWidgetModel
9. Инкапсулированные функции
10. Переопределённые функции

Желательно группировать синхронные и асинхронные функции друг с другом.

# Разное

Каждый запрос который может вернуть Exception обязан быть обработан через try catch

Функции которые срабатывает при нажатии на какой-то объект должны начинаться с on. К примеру  onButtonTap, onNavigateBackTap, onRemoveFromFirendsTap.

