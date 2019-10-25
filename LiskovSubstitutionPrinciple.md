# Liskov Substitution Principle

 Objects in a program should be **replaceable** with instances of their **subtypes**
 **w/o** altering the correctness of the program

```c++
//This is a violation example:
class Rectangle
{
protected:
  int width, height;
public:
  Rectangle(const int width, const int height)
    : width{width},
      height{height}
  {
  }

  virtual int GetWidth() const
  {
    return width;
  }

  virtual void SetWidth(const int width)
  {
    this->width = width;
  }

  virtual int GetHeight() const
  {
    return height;
  }

  virtual void SetHeight(const int height)
  {
    this->height = height;
  }

  int Area() const { return width*height; }
};

class Square : public Rectangle
{
public:
  Square(int size) : Rectangle{size,size} {}

  void SetWidth(const int width) override {
    this->width = height = width;
  }
  void SetHeight(const int height) override {
    this->height = width = height;
  }
};

void process(Rectangle& r)
{
  int w = r.GetWidth();
  r.SetHeight(10);

  std::cout << "expect area = " << (w * 10)
    << ", got " << r.Area() << std::endl;
}

int main()
{
  Rectangle r{ 5,5 };
  process(r);

  Square s{ 5 };
  process(s);
  
  return 0;
}
```

## 对里氏替换原则的个人理解：
1.写基类的时候，很容易不自觉地写出只适用于基类的函数。
2.当子类不能完全替代父类的时候，调用到这些函数，编译运行都能通过（没有提示，比较危险），但是程序运行会得到错误的结果。