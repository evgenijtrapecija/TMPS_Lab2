## Лабораторная №2

*Подготовил - Евгений Кифа , студент группы TI-164*

*Целью данной лабораторной работы было имплементировать 5 структурных (можно было использовать и поведенческие) шабловнов*

Были выбраны 5 шаблонов:

## 1. Decorator
![Decorator](https://refactoring.guru/images/patterns/cards/decorator-mini-2x.png)

Декоратор — это структурный паттерн проектирования, который позволяет динамически добавлять объектам новую функциональность, оборачивая их в полезные «обёртки».
Декоратор имеет альтернативное название — обёртка. Оно более точно описывает суть паттерна: вы помещаете целевой объект в другой объект-обёртку, который запускает базовое поведение объекта, а затем добавляет к результату что-то своё.

Оба объекта имеют общий интерфейс, поэтому для пользователя нет никакой разницы, с каким объектом работать — чистым или обёрнутым. Вы можете использовать несколько разных обёрток одновременно — результат будет иметь объединённое поведение всех обёрток сразу.

С помощью декоратор мы оборачиваем функцию в другую с дополнительным функционалом 

Функция Draw включает в себя вызов нескольних внутренних методов
```
Draw(){
        this.Context.beginPath();
        this.Context.arc(this.X,this.Y,this.D,0,2*Math.PI);
        this.Context.stroke();
        this.Context.fillStyle = new ColorAdapter(this.Color).Hex;
        this.Context.fill();
    }
```
## 2. Bridge
![Bridge](https://refactoring.guru/images/patterns/cards/bridge-mini-2x.png)

Мост — это структурный паттерн проектирования, который разделяет один или несколько классов на две отдельные иерархии — абстракцию и реализацию, позволяя изменять их независимо друг от друга.

Пример проблемы при которой можно использовать этот паттерн :

У вас есть класс геометрических Фигур, который имеет подклассы Круг и  Квадрат. Вы хотите расширить иерархию фигур по цвету, то есть иметь Красные и Синие фигуры. Но чтобы всё это объединить, вам придётся создать 4 комбинации подклассов, вроде СиниеКруги и КрасныеКвадраты.

![Bridge](https://refactoring.guru/images/patterns/diagrams/bridge/problem-2x.png)

При добавлении новых видов фигур и цветов количество комбинаций будет расти в геометрической прогрессии. Например, чтобы ввести в программу фигуры треугольников, придётся создать сразу два новых подкласса треугольников под каждый цвет. После этого новый цвет потребует создания уже трёх классов для всех видов фигур. Чем дальше, тем хуже.

Мост позволил нам осуществить вывод параметров в отдельный класс.

Класс Circle включает в себя параметры ColorAdapter и Color, которые являются классами идентифицирующие цвет

```
class Circle {
    constructor(X, Y, D, Color, Context) {
        this.History = [this];
        this.X = X;
        this.Y = Y;
        this.D = D;
        this.Color = Color;
        this.Adapter = new ColorAdapter(this.Color);
        this.Context = Context;
    }
}
```

## 3. Iterator
![Iterator](https://refactoring.guru/images/patterns/cards/iterator-mini-2x.png)

Итератор — это поведенческий паттерн проектирования, который даёт возможность последовательно обходить элементы составных объектов, не раскрывая их внутреннего представления.

Идея паттерна Итератор состоит в том, чтобы вынести поведение обхода коллекции из самой коллекции в отдельный класс.

![Iterator](https://refactoring.guru/images/patterns/diagrams/iterator/solution1-2x.png)

Иттератор дает нам возможность выполнить реализацию разных проходов по листу 

Данный паттерн реализуется следующим интерфейсов:

```
interface IIterator{
    next();
    hasMore();
}

export default IIterator
```

А интерфейс реализуется классом

```
import IITerator from './IITerator'

class Iterator implements IITerator{
    private currentPos = 0;
    private Collection:Array<any> = new Array();

    Iterator(){ }

    next():void{
        if(this.currentPos < this.Collection.length){
            return this.Collection[this.currentPos++]
        }
        else{
            this.currentPos = 0
        }
    }
    push(item:any):void{
        this.Collection.push(item)
    }
    pop():void{
        this.Collection.pop()
    }
    hasMore():Boolean{
        if(this.currentPos < this.Collection.length){
            return true;
        }
        else{
            return false;
        }
    }
}

export default Iterator
```

## 4. Memento
![Iterator](https://refactoring.guru/images/patterns/cards/memento-mini-2x.png)

Снимок — это поведенческий паттерн проектирования, который позволяет сохранять и восстанавливать прошлые состояния объектов, не раскрывая подробностей их реализации.
Пример проблемы, где можно использовать данный паттерн:

Предположим, что вы пишете программу текстового редактора. Помимо обычного редактирования, ваш редактор позволяет менять форматирование текста, вставлять картинки и прочее.

В какой-то момент вы решили сделать все эти действия отменяемыми. Для этого вам нужно сохранять текущее состояние редактора перед тем, как выполнить любое действие. Если потом пользователь решит отменить своё действие, вы достанете копию состояния из истории и восстановите старое состояние редактора.

![Iterator](https://refactoring.guru/images/patterns/diagrams/memento/problem1-ru-2x.png)

Снимок позволяет сохранять нам историю объекта, его предыдущее состояние

Данный паттерн реализуется интерфейсом

```
interface IMementoCircle{
    getName():string;
    getSnapshot(version:number)
}

export default IMementoCircle;
```

А интерфейс реализуется
```
class Circle implements IMementoCircle{
    ...
    getName(){
        return "Circle";
    }

    getSnapshot(version:number){
        if(this.History.length > version){
            return this.History[version];
        }
        else{
            return this;
        }
    }
}
```

## 5. Adapter
![Adapter](https://refactoring.guru/images/patterns/cards/adapter-mini-2x.png)

Адаптер — это структурный паттерн проектирования, который позволяет объектам с несовместимыми интерфейсами работать вместе.

Пример проблемы, где может пригодиться этот паттерн:

 Представьте, что вы делаете приложение для торговли на бирже. Ваше приложение скачивает биржевые котировки из нескольких источников в XML, а затем рисует красивые графики.

В какой-то момент вы решаете улучшить приложение, применив стороннюю библиотеку аналитики. Но вот беда — библиотека поддерживает только формат данных JSON, несовместимый с вашим приложением.

![Adapter](https://refactoring.guru/images/patterns/diagrams/adapter/problem-2x.png)

Вы смогли бы переписать библиотеку, чтобы та поддерживала формат XML. Но, во-первых, это может нарушить работу существующего кода, который уже зависит от библиотеки. А во-вторых, у вас может просто не быть доступа к её исходному коду.

Адаптер нужен был нам, чтобы можно было принудительно привести один объект к интерфейсу другого 

### Hex color Interface
```
interface IColorHEX{
    Hex:string;
}

export default IColorHEX;
```

### RGB color Interface
```
interface IColorRGB{
    Red:number;
    Green:number;
    Blue:number;
}

export default IColorRGB;
```

### Color Adapter класс который позволяет RGB и HEX объединить

```
class ColorAdapter implements IColorHEX,IColorRGB{
    public Red:number;
    public Green:number;
    public Blue:number;
    public Hex:string;
}
```
