Задание №2: “Трёхкатегорийная абстрактная фабрика”

Представьте, что ваша фабрика выпускает **n** продуктов (например, _Столы_, _Стулья_ и _Диваны_), имеющие одно категорийное деление (например, _железные/деревянные/пластиковые_) и второе категорийное деление (например, из _Российских/Китайских/Испанских_ материалов). Категорий в первом и втором делении - константа. 
Итого вы будете иметь дерево наследований, содержащее на первом уровне абстрактный класс _Изделие_, на втором - абстрактные классы всех разновидностей продукта, на третьем - абстрактные классы всех продуктов для всех Первых категорий, на четвёртом - конкретные классы всех продуктов для всех сочетаний категорий

Непосредственно задание: при всех известных классах из этой иерархии создать структуру, получающую список этих классов в случайном порядке и генерирующую абстрактную фабрику на этих классах.
Обратите внимание - несмотря на “деление по 3” в примере, категорий каждого уровня может быть **2**, **3** или **4**.

Более формальное описание.  
Интерфейс генерации:  
```c++
GetAbstractFactory<
    3, 
    5,
    Typelist<Chair, Table, Sofa>, 
    Typelist<WoodenChair, WoodenTable, WoodenSofa>, 
    Typelist<MetalChair, MetalTable, MetalSofa>, 
    Typelist<MetalRichChair, MetalRichTable, MetalRichSofa>, 
    Typelist< MetalPoorChair, MetalPoorTable, MetalPoorSofa>
>;
```
где **3** - гарантированное количество изделий в каком-либо семействе, **5** - количество тайплистов, которые идут после. 
Каждый тайплист содержит три конкретных имплементации в правильном порядке, тайплисты перемешаны. 
Все классы, даже классы **Chair**, **Table**, **Sofa**, содержат конструкторы по умолчанию.  

Интерфейс получения фабрики:  
```c++
using MyFactoryHierarchy = GetAbstractFactory<
   3, 
   5,
   Typelist<Chair, Table, Sofa>, 
   Typelist<WoodenChair, WoodenTable, WoodenSofa>, 
   Typelist<MetalChair, MetalTable, MetalSofa>, 
   Typelist<MetalRichChair, MetalRichTable, MetalRichSofa>, 
   Typelist<MetalPoorChair, MetalPoorTable, MetalPoorSofa>
>;
```
```c++
Factory* MyFactory = new MyFactoryHierarchy::GetConcreteFactory<MetalRichChair>::res;
```
запрос конкретной фабрики происходит по одному (любому) из конкретных изделий, имеющихся в ней. 
Задача, соответственно, найти фабрику в иерархии.   
```c++
Sofa a = MyFactory->Get<Sofa>();
``` 
Фабрика создана, теперь на неё приходит запрос на создание конкретного экземпляра класса. 
Она, соответственно, должна вернуть **MetalRichSofa** по указателю на **Sofa**.