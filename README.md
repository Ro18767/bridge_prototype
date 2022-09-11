# pattern_bridge

## Цель патерна
### Проблема
Перед нами стоит задача объеденять 2 групы класов так чтоб уменьшить зависимости между групами.
### Решение
Паттерн мост иместо хранения его состояний и поведения внутри одного класса
решает проблемы агрегированием или композицией.
Таким образом ссылаясь на обект с состояниями и поведениями.
Такая связь и станет мостом между новыми классами.


## Плюсы и минусы
### Плюсы
+ Позволяет строить платформо-независимые программы.
+ Скрывает лишние или опасные детали реализации от клиентского кода.
+ Реализует принцип открытости/закрытости.
 ### Минусы
+ Усложняет код программы из-за введения дополнительных классов.

## Аналогия
У нас есть двери и у нас есть стены.
Если вдруг нам не нравится двери,
то мы можем попробовать их заменить.
Если не нравится стена,
то тут посложнее, и не можем ее заменить отдельно от двери
Поэтому у стены есть дверь, а не наоборот

![мем с дверью](https://user-images.githubusercontent.com/108687865/189526338-57af2b4f-fd64-46ba-8c7e-978c90b94e27.jpg)


## Пример
```cpp
#include <iostream>
#include <string>
using namespace std;


class Object3D abstract
{
protected:
  string name;
  Object3D(string _name) : name{ _name } {}
public:
  virtual void setName(string _name) {
    name = _name;
  }
  virtual string getName() const {
    return name;
  }
  virtual Object3D* clone() const = 0;
  virtual void draw() const = 0;
};
class Box : public Object3D
{
public:
  Box(string _name) :Object3D{ _name } {}

  Object3D* clone() const override {
    cout << "box clone" << endl;
    return new Box(name);
  }
  void draw() const override {
    cout << "draw box with name: " << name << endl;
  };
};
class Sphere : public Object3D
{
public:
  Sphere(string _name) :Object3D{ _name } {}

  Object3D* clone() const override {
    cout << "sphere clone" << endl;
    return new Box(name);
  }
  void draw() const override {
    cout << "draw sphere with name: " << name << endl;
  };
};
int main()
{
  // prototype
  Object3D* box1 = new Box("Big box");

  // clone
  Object3D* box2 = box1->clone();
  box2->setName("Big box clone");

  box1->draw();
  box2->draw();

  // prototype
  Object3D* sphere1 = new Sphere("Big sphere");

  // clone
  Object3D* sphere2 = sphere1->clone();
  sphere2->setName("Big sphere clone");

  sphere1->draw();
  sphere2->draw();
}
```
### Выводит в консоль
```
box clone
draw box with name: Big box
draw box with name: Big box clone
sphere clone
draw sphere with name: Big sphere
draw box with name: Big sphere clone
```

![ClassDiagram](https://user-images.githubusercontent.com/108687865/188584146-dd8ec2a3-d48b-4869-9e9e-e59acf1d7f31.jpg)

## Список типов
+ Базовый тип Object3D - выступает как интерфейс для прототипа
+ Наследник Box - выступает как реалзация прототипа
+ Наследник Sphere - выступает как реалзация прототипа

## Аналогия
Приходит человек в бар и видит другого с какимто напитком.
Просит у бармена такого же (копию или клон) напитка.
Бармен денлает такой же и врочает тому кто его просил.


