# unittest

## 一些名词

* 单元测试：用于核实函数的某个方面没有问题
* 测试用例：一组单元测试，核实函数在各种情形下的行为都符合要求
* 全覆盖的测试用例：包含一整套单元测试，涵盖了各种可能的函数使用方式

## 示例

待测试函数如下所示：

```python
def function_be_tested(input: str) -> str:
    return input * 3
```

编写测试用例如下：

```python
import unittest
from main import function_be_tested

class MyTestCase(unittest.TestCase):
    """一个继承unittest.TestCase的类"""

    def test_basic_io(self):
        """能够满足基本的输入输出吗"""

        output = function_be_tested('ha')
        self.assertEqual(output, 'hahaha')

if __name__ == '__main__':
    unittest.main()
```

其中：
1. 测试用例是一个继承了“unittest模块中的TestCase类”的类
2. 测试用例中的每一个方法都会进行一个单元测试，核实函数的某个方面没有问题
3. 单元测试的方法名必须**以"test_"开头**，这是unittest模块的规定，告诉Python这是一个单元测试。方法名必须详细说明该单元测试的测试内容，没必要简写，你又不会自己来调用这个方法，只有这样，测试未通过时你才能看懂是哪里有问题
4. 在单元测试中，像平常那样调用函数，然后交给**断言方法**核实结果
5. `if __name__ == '__main__':`仅当直接执行本测试文件时才运行下述代码。当该测试文件被其它.py文件import时，这行if语句能够防止`unittest.main()`被误执行

## 断言方法

* `assertEqual(a, b)`核实a == b
* `assertNotEqual(a, b)`核实a != b
* `assertTrue(x)`核实x为True
* `assertFalse(x)`核实x为False
* `assertIn(member, container)`核实member在组合数据类型container内，即member in container
* `assertNotIn(member, container)`核实member不在组合数据类型container内，即member not in container

## 测试用例的输出

运行测试用例时，每完成一个单元测试就会输出一个字符：
* `.`：测试通过
* `E`：引发错误
* `F`：断言失败

## setUp方法

当测试前需要准备测试数据时（比如测试某个类时，需要先由这个类创建被测实例），会用到setUp方法。运行测试用例时，setUp方法会在每个单元测试执行前先执行，保证每个单元测试执行时得到的都是新创建的测试数据，没有被其它单元测试改变

假设有如下待测试类：

```python
class Warship:
    """一个战舰类"""

    def __init__(self, name: str, hp: int) -> None:
        """初始化一个新战舰实例"""

        self.name = name
        self.hp = hp
        self.sunk = False
        self.shell_shooted = 0

    def salvo(self) -> None:
        """定义一次齐射动作"""

        self.shell_shooted = self.shell_shooted + 3*2
        print(f"{self.name} made a salvo! ")

    def shell_count(self) -> int:
        """计算已射出炮弹数"""

        return self.shell_shooted

    def hit_by_JeanBart(self, shell_num: int) -> None:
        """被让巴尔打了一轮"""

        self.hp -= shell_num*1190
        print(f"{self.name} was hit by Jean Bart! ")

        if self.hp < 0:
            self.hp = 0
            self.sunk = True
            print(f"{self.name} sunk! ")
```

则可使用setUp方法测试该类：

```python
import unittest
from main import Warship

class TestWarshipClass(unittest.TestCase):
    """测试Warship类"""

    def setUp(self):
        """创建被测实例和测试数据"""

        self.my_ship = Warship('Shimakaze', 17900)  # 被测实例my_ship
        self.hp_with_commander_skill = 21400  # 测试中会用到的数据

    def test_salvo(self):
        self.my_ship.salvo()
        self.assertEqual(self.my_ship.shell_shooted, 6)

    def test_shell_count(self):
        for i in range(3):
            self.my_ship.salvo()
        self.assertEqual(self.my_ship.shell_count(), 18)

    def test_hit_by_JeanBart1(self):
        # 先被打一轮没死
        self.my_ship.hit_by_JeanBart(8)
        self.assertEqual(self.my_ship.hp, 17900-1190*8)
        self.assertFalse(self.my_ship.sunk)

        # 再被打一轮死了
        self.my_ship.hit_by_JeanBart(8)
        self.assertEqual(self.my_ship.hp, 0)
        self.assertTrue(self.my_ship.sunk)

    def test_hit_by_JeanBart2(self):
        # 换成加血岛风
        self.my_ship.hp = self.hp_with_commander_skill

        # 被打两轮死不了
        for i in range(2):
            self.my_ship.hit_by_JeanBart(8)
        self.assertEqual(
            self.my_ship.hp, self.hp_with_commander_skill-1190*8*2)
        self.assertFalse(self.my_ship.sunk)

        # 再被打一轮还是死了
        self.my_ship.hit_by_JeanBart(8)
        self.assertEqual(self.my_ship.hp, 0)
        self.assertTrue(self.my_ship.sunk)

if __name__ == '__main__':
    unittest.main()
```
