# SOLID PRINCIPLES AND DESIGN PATTERNS

## LEARN MAP

![map](./solid.svg)

[CHECK THE INTERACTIVE MAP](https://sanix-darker.github.io/solid_learn/solid.html)

## CONCEPTS

- What are SOLID Principles?
  - Five principles of object-oriented programming
  - Introduced by Robert C. Martin
- Importance of SOLID Principles
  - Maintainability
  - Extensibility
  - Reusability

## SOLID Principles
- Single Responsibility Principle (SRP)
  - Definition: A class should have only one reason to change.
  - Example:
    ```python
    class Book:
        def __init__(self, title, author):
            self.title = title
            self.author = author

        def get_title(self):
            return self.title

        def get_author(self):
            return self.author

    class Printer:
        def print_book(self, book):
            print(f"Title: {book.get_title()}, Author: {book.get_author()}")

    book = Book("Clean Code", "Robert C. Martin")
    printer = Printer()
    printer.print_book(book)
    ```
- Open/Closed Principle (OCP)
  - Definition: Classes should be open for extension but closed for modification.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Shape(ABC):
        @abstractmethod
        def draw(self):
            pass

    class Circle(Shape):
        def draw(self):
            print("Drawing Circle")

    class Square(Shape):
        def draw(self):
            print("Drawing Square")

    class ShapeDrawer:
        def __init__(self, shape):
            self.shape = shape

        def draw_shape(self):
            self.shape.draw()

    circle = Circle()
    square = Square()

    circle_drawer = ShapeDrawer(circle)
    square_drawer = ShapeDrawer(square)

    circle_drawer.draw_shape()
    square_drawer.draw_shape()
    ```
- Liskov Substitution Principle (LSP)
  - Definition: Objects of a superclass should be replaceable with objects of its subclasses without affecting the functionality.
  - Example:
    ```python
    class Bird:
        def fly(self):
            pass

    class Sparrow(Bird):
        def fly(self):
            print("Sparrow flying")

    class Ostrich(Bird):
        def fly(self):
            raise NotImplementedError("Ostrich cannot fly")

    def make_bird_fly(bird):
        bird.fly()

    sparrow = Sparrow()
    ostrich = Ostrich()

    make_bird_fly(sparrow)
    make_bird_fly(ostrich)
    ```
- Interface Segregation Principle (ISP)
  - Definition: Clients should not be forced to depend on interfaces they do not use.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Printer(ABC):
        @abstractmethod
        def print_document(self, document):
            pass

    class Scanner(ABC):
        @abstractmethod
        def scan_document(self, document):
            pass

    class Fax(ABC):
        @abstractmethod
        def fax_document(self, document):
            pass

    class AllInOne(Printer, Scanner, Fax):
        def print_document(self, document):
            print(f"Printing document: {document}")

        def scan_document(self, document):
            print(f"Scanning document: {document}")

        def fax_document(self, document):
            print(f"Faxing document: {document}")

    all_in_one = AllInOne()
    all_in_one.print_document("Report")
    all_in_one.scan_document("Invoice")
    all_in_one.fax_document("Contract")
    ```
- Dependency Inversion Principle (DIP)
  - Definition: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class NotificationService(ABC):
        @abstractmethod
        def send_notification(self, message):
            pass

    class EmailNotification(NotificationService):
        def send_notification(self, message):
            print(f"Sending email notification: {message}")

    class SMSNotification(NotificationService):
        def send_notification(self, message):
            print(f"Sending SMS notification: {message}")

    class NotificationSender:
        def __init__(self, notification_service):
            self.notification_service = notification_service

        def send_notification(self, message):
            self.notification_service.send_notification(message)

    email_notification = EmailNotification()
    sms_notification = SMSNotification()

    email_sender = NotificationSender(email_notification)
    sms_sender = NotificationSender(sms_notification)

    email_sender.send_notification("Meeting reminder")
    sms_sender.send_notification("Payment due")
    ```

## Introduction to Design Patterns
- What are Design Patterns?
  - Reusable solutions to common problems
  - Cataloged by Gang of Four (GoF)
- Categories of Design Patterns
  - Creational Patterns
  - Structural Patterns
  - Behavioral Patterns
- Benefits of Design Patterns
  - Encapsulation
  - Flexibility
  - Scalability

## Creational Patterns
- Singleton Pattern
  - Definition: Ensures a class has only one instance and provides a global point of access to it.
  - Example:
    ```python
    class Singleton:
        _instance = None

        def __new__(cls):
            if cls._instance is None:
                cls._instance = super().__new__(cls)
            return cls._instance

    singleton1 = Singleton()
    singleton2 = Singleton()

    print(singleton1 == singleton2)  # Output: True
    ```
- Factory Method Pattern
  - Definition: Defines an interface for creating objects, but allows subclasses to alter the type of objects that will be created.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Animal(ABC):
        @abstractmethod
        def speak(self):
            pass

    class Dog(Animal):
        def speak(self):
            return "Woof"

    class Cat(Animal):
        def speak(self):
            return "Meow"

    class AnimalFactory(ABC):
        @abstractmethod
        def create_animal(self):
            pass

    class DogFactory(AnimalFactory):
        def create_animal(self):
            return Dog()

    class CatFactory(AnimalFactory):
        def create_animal(self):
            return Cat()

    dog_factory = DogFactory()
    cat_factory = CatFactory()

    dog = dog_factory.create_animal()
    cat = cat_factory.create_animal()

    print(dog.speak())  # Output: Woof
    print(cat.speak())  # Output: Meow
    ```
- Abstract Factory Pattern
  - Definition: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class AbstractFactory(ABC):
        @abstractmethod
        def create_product_a(self):
            pass

        @abstractmethod
        def create_product_b(self):
            pass

    class ConcreteFactory1(AbstractFactory):
        def create_product_a(self):
            return ConcreteProductA1()

        def create_product_b(self):
            return ConcreteProductB1()

    class ConcreteFactory2(AbstractFactory):
        def create_product_a(self):
            return ConcreteProductA2()

        def create_product_b(self):
            return ConcreteProductB2()

    class AbstractProductA(ABC):
        @abstractmethod
        def useful_function_a(self):
            pass

    class ConcreteProductA1(AbstractProductA):
        def useful_function_a(self):
            return "The result of the product A1."

    class ConcreteProductA2(AbstractProductA):
        def useful_function_a(self):
            return "The result of the product A2."

    class AbstractProductB(ABC):
        @abstractmethod
        def useful_function_b(self):
            pass

        @abstractmethod
        def another_useful_function_b(self, collaborator):
            pass

    class ConcreteProductB1(AbstractProductB):
        def useful_function_b(self):
            return "The result of the product B1."

        def another_useful_function_b(self, collaborator):
            return f"The result of the B1 collaborating with {collaborator.useful_function_a()}"

    class ConcreteProductB2(AbstractProductB):
        def useful_function_b(self):
            return "The result of the product B2."

        def another_useful_function_b(self, collaborator):
            return f"The result of the B2 collaborating with {collaborator.useful_function_a()}"

    def client_code(factory: AbstractFactory) -> None:
        product_a = factory.create_product_a()
        product_b = factory.create_product_b()

        print(product_b.useful_function_b())
        print(product_b.another_useful_function_b(product_a))


    client_code(ConcreteFactory1())  # Output: The result of the product B1. The result of the B1 collaborating with The result of the product A1.
    client_code(ConcreteFactory2())  # Output: The result of the product B2. The result of the B2 collaborating with The result of the product A2.
    ```

- Builder Pattern
  - Definition: Separates the construction of a complex object from its representation so that the same construction process can create different representations.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Builder(ABC):
        @abstractmethod
        def build_part_a(self):
            pass

        @abstractmethod
        def build_part_b(self):
            pass

    class ConcreteBuilder1(Builder):
        def __init__(self):
            self.product = Product()

        def build_part_a(self):
            self.product.add("PartA1")

        def build_part_b(self):
            self.product.add("PartB1")

    class ConcreteBuilder2(Builder):
        def __init__(self):
            self.product = Product()

        def build_part_a(self):
            self.product.add("PartA2")

        def build_part_b(self):
            self.product.add("PartB2")

    class Product:
        def __init__(self):
            self.parts = []

        def add(self, part):
            self.parts.append(part)

        def list_parts(self):
            print(f"Product parts: {', '.join(self.parts)}")

    class Director:
        def __init__(self):
            self.builder = None

        def set_builder(self, builder):
            self.builder = builder

        def build_minimal_viable_product(self):
            self.builder.build_part_a()

        def build_full_featured_product(self):
            self.builder.build_part_a()
            self.builder.build_part_b()

    director = Director()

    builder1 = ConcreteBuilder1()
    director.set_builder(builder1)
    director.build_minimal_viable_product()
    builder1.product.list_parts()  # Output: Product parts: PartA1

    builder2 = ConcreteBuilder2()
    director.set_builder(builder2)
    director.build_full_featured_product()
    builder2.product.list_parts()  # Output: Product parts: PartA2, PartB2
    ```
- Prototype Pattern
  - Definition: Creates new objects by copying an existing object, known as the prototype.
  - Example:
    ```python
    import copy

    class Prototype:
        def __init__(self):
            self._objects = {}

        def register_object(self, name, obj):
            self._objects[name] = obj

        def unregister_object(self, name):
            del self._objects[name]

        def clone(self, name, **attrs):
            obj = copy.deepcopy(self._objects.get(name))
            obj.__dict__.update(attrs)
            return obj

    class Car:
        def __init__(self):
            self.make = "Toyota"
            self.model = "Corolla"
            self.year = 2022

        def __str__(self):
            return f"{self.year} {self.make} {self.model}"

    car_prototype = Prototype()
    car_prototype.register_object("Corolla", Car())

    car1 = car_prototype.clone("Corolla")
    print(car1)  # Output: 2022 Toyota Corolla

    car2 = car_prototype.clone("Corolla", year=2023)
    print(car2)  # Output: 2023 Toyota Corolla
    ```

## Structural Patterns
- Adapter Pattern
  - Definition: Allows objects with incompatible interfaces to collaborate.
  - Example:
    ```python
    class Target:
        def request(self):
            return "Target: The default target's behavior."

    class Adaptee:
        def specific_request(self):
            return ".eetpadA eht fo roivaheb laicepS"

    class Adapter(Target):
        def __init__(self, adaptee):
            self.adaptee = adaptee

        def request(self):
            return f"Adapter: (TRANSLATED) {self.adaptee.specific_request()[::-1]}"

    def client_code(target):
        print(target.request())

    adaptee = Adaptee()
    adapter = Adapter(adaptee)

    client_code(adapter)  # Output: Adapter: (TRANSLATED) Special behavior of the adaptee.
    ```

- Bridge Pattern
  - Definition: Decouples an abstraction from its implementation so that the two can vary independently.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Implementor(ABC):
        @abstractmethod
        def operation_implementation(self) -> str:
            pass

    class ConcreteImplementorA(Implementor):
        def operation_implementation(self) -> str:
            return "ConcreteImplementorA: Here's the result on the platform A."

    class ConcreteImplementorB(Implementor):
        def operation_implementation(self) -> str:
            return "ConcreteImplementorB: Here's the result on the platform B."

    class Abstraction:
        def __init__(self, implementor: Implementor):
            self.implementor = implementor

        def operation(self) -> str:
            return (f"Abstraction: Base operation with:\n"
                    f"{self.implementor.operation_implementation()}")

    class ExtendedAbstraction(Abstraction):
        def operation(self) -> str:
            return (f"ExtendedAbstraction: Extended operation with:\n"
                    f"{self.implementor.operation_implementation()}")

    def client_code(abstraction: Abstraction) -> None:
        print(abstraction.operation())

    implementor_a = ConcreteImplementorA()
    implementor_b = ConcreteImplementorB()

    abstraction_a = Abstraction(implementor_a)
    abstraction_b = ExtendedAbstraction(implementor_b)

    client_code(abstraction_a)  # Output: Abstraction: Base operation with: ConcreteImplementorA: Here's the result on the platform A.
    client_code(abstraction_b)  # Output: ExtendedAbstraction: Extended operation with: ConcreteImplementorB: Here's the result on the platform B.
    ```
- Composite Pattern
  - Definition: Composes objects into tree structures to represent part-whole hierarchies. Allows clients to treat individual objects and compositions of objects uniformly.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Component(ABC):
        @abstractmethod
        def operation(self) -> str:
            pass

    class Leaf(Component):
        def operation(self) -> str:
            return "Leaf"

    class Composite(Component):
        def __init__(self):
            self._children = []

        def add(self, component: Component) -> None:
            self._children.append(component)

        def remove(self, component: Component) -> None:
            self._children.remove(component)

        def operation(self) -> str:
            results = []
            for child in self._children:
                results.append(child.operation())
            return f"Branch({'+'.join(results)})"

    def client_code(component: Component) -> None:
        print(f"RESULT: {component.operation()}", end="")

    leaf = Leaf()
    print("Client: I've got a simple component:")
    client_code(leaf)  # Output: Client: I've got a simple component: RESULT: Leaf

    composite = Composite()
    tree = Composite()

    tree.add(leaf)
    tree.add(composite)
    print("\n\nClient: Now I've got a composite tree:")
    client_code(tree)  # Output: Client: Now I've got a composite tree: RESULT: Leaf+Branch()

    branch1 = Composite()
    branch1.add(Leaf())
    branch1.add(Leaf())

    branch2 = Composite()
    branch2.add(Leaf())

    tree.add(branch1)
    tree.add(branch2)

    print("\n\nClient: Now I've got a more complex composite tree:")
    client_code(tree)  # Output: Client: Now I've got a more complex composite tree: RESULT: Leaf+Branch(Leaf+Leaf)+Branch(Leaf)
    ```

- Decorator Pattern
  - Definition: Attaches additional responsibilities to an object dynamically. Provides a flexible alternative to subclassing for extending functionality.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Component(ABC):
        @abstractmethod
        def operation(self) -> str:
            pass

    class ConcreteComponent(Component):
        def operation(self) -> str:
            return "ConcreteComponent"

    class Decorator(Component):
        def __init__(self, component: Component) -> None:
            self._component = component

        @abstractmethod
        def operation(self) -> str:
            pass

    class ConcreteDecoratorA(Decorator):
        def operation(self) -> str:
            return f"ConcreteDecoratorA({self._component.operation()})"

    class ConcreteDecoratorB(Decorator):
        def operation(self) -> str:
            return f"ConcreteDecoratorB({self._component.operation()})"

    def client_code(component: Component) -> None:
        print(f"RESULT: {component.operation()}", end="")

    simple = ConcreteComponent()
    print("Client: I've got a simple component:")
    client_code(simple)  # Output: Client: I've got a simple component: RESULT: ConcreteComponent

    decorator1 = ConcreteDecoratorA(simple)
    decorator2 = ConcreteDecoratorB(decorator1)
    print("\n\nClient: Now I've got a decorated component:")
    client_code(decorator2)  # Output: Client: Now I've got a decorated component: RESULT: ConcreteDecoratorB(ConcreteDecoratorA(ConcreteComponent))
    ```

- Facade Pattern
  - Definition: Provides a simplified interface to a complex system of classes, interfaces, or subsystems.
  - Example:
    ```python
    class Subsystem1:
        def operation1(self) -> str:
            return "Subsystem1: Ready!"

        def operation_n(self) -> str:
            return "Subsystem1: Go!"

    class Subsystem2:
        def operation1(self) -> str:
            return "Subsystem2: Get ready!"

        def operation_z(self) -> str:
            return "Subsystem2: Fire!"

    class Facade:
        def __init__(self, subsystem1: Subsystem1, subsystem2: Subsystem2):
            self._subsystem1 = subsystem1
            self._subsystem2 = subsystem2

        def operation(self) -> str:
            results = []
            results.append("Facade initializes subsystems:")
            results.append(self._subsystem1.operation1())
            results.append(self._subsystem2.operation1())
            results.append("Facade orders subsystems to perform the action:")
            results.append(self._subsystem1.operation_n())
            results.append(self._subsystem2.operation_z())
            return "\n".join(results)

    def client_code(facade: Facade) -> None:
        print(facade.operation())

    subsystem1 = Subsystem1()
    subsystem2 = Subsystem2()
    facade = Facade(subsystem1, subsystem2)

    client_code(facade)
    ```

- Proxy Pattern
  - Definition: Provides a surrogate or placeholder for another object to control access to it.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Subject(ABC):
        @abstractmethod
        def request(self) -> str:
            pass

    class RealSubject(Subject):
        def request(self) -> str:
            return "RealSubject: Handling request."

    class Proxy(Subject):
        def __init__(self, real_subject: RealSubject) -> None:
            self._real_subject = real_subject

        def request(self) -> str:
            if self.check_access():
                return self._real_subject.request()
            return "Proxy: Access denied."

        def check_access(self) -> bool:
            return True  # For simplicity, always grant access.

    def client_code(subject: Subject) -> None:
        print(subject.request())

    real_subject = RealSubject()
    proxy = Proxy(real_subject)

    print("Client: Executing the client code with a real subject:")
    client_code(real_subject)  # Output: Client: Executing the client code with a real subject: RealSubject: Handling request.

    print("\nClient: Executing the same client code with a proxy:")
    client_code(proxy)  # Output: Client: Executing the same client code with a proxy: RealSubject: Handling request.
    ```

## Behavioral Patterns

- Observer Pattern
  - Definition: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
  - Example:
    ```python
    class Observer:
        def update(self, subject):
            pass

    class Subject:
        def __init__(self):
            self._observers = []

        def attach(self, observer):
            self._observers.append(observer)

        def detach(self, observer):
            self._observers.remove(observer)

        def notify(self):
            for observer in self._observers:
                observer.update(self)

        def some_business_logic(self):
            self.notify()

    class ConcreteObserverA(Observer):
        def update(self, subject):
            if subject.some_business_logic():
                print("ConcreteObserverA: Reacted to the event")

    class ConcreteObserverB(Observer):
        def update(self, subject):
            if subject.some_business_logic():
                print("ConcreteObserverB: Reacted to the event")

    subject = Subject()
    observer1 = ConcreteObserverA()
    observer2 = ConcreteObserverB()

    subject.attach(observer1)
    subject.attach(observer2)

    subject.some_business_logic()
    ```

- Strategy Pattern
  - Definition: Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Strategy(ABC):
        @abstractmethod
        def do_algorithm(self, data):
            pass

    class ConcreteStrategyA(Strategy):
        def do_algorithm(self, data):
            return sorted(data)

    class ConcreteStrategyB(Strategy):
        def do_algorithm(self, data):
            return reversed(sorted(data))

    class Context:
        def __init__(self, strategy: Strategy):
            self._strategy = strategy

        def context_interface(self, data):
            return self._strategy.do_algorithm(data)

    context = Context(ConcreteStrategyA())
    result = context.context_interface([3, 2, 1])
    print(result)  # Output: [1, 2, 3]

    context = Context(ConcreteStrategyB())
    result = context.context_interface([3, 2, 1])
    print(result)  # Output: [3, 2, 1]
    ```

- Chain of Responsibility Pattern
  - Definition: Allows passing requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.
  - Example:
    ```python
    from abc import ABC, abstractmethod

    class Handler(ABC):
        @abstractmethod
        def set_next(self, handler):
            pass

        @abstractmethod
        def handle(self, request):
            pass

    class AbstractHandler(Handler):
        _next_handler = None

        def set_next(self, handler):
            self._next_handler = handler
            return handler

        @abstractmethod
        def handle(self, request):
            if self._next_handler:
                return self._next_handler.handle(request)
            return None

    class ConcreteHandler1(AbstractHandler):
        def handle(self, request):
            if request == "Handler1":
                return f"{self.__class__.__name__}: Handled {request}"
            else:
                return super().handle(request)

    class ConcreteHandler2(AbstractHandler):
        def handle(self, request):
            if request == "Handler2":
                return f"{self.__class__.__name__}: Handled {request}"
            else:
                return super().handle(request)

    handler1 = ConcreteHandler1()
    handler2 = ConcreteHandler2()

    handler1.set_next(handler2)
    result = handler1.handle("Handler1")
    print(result)  # Output: ConcreteHandler1: Handled Handler1

    result = handler1.handle("Handler2")
    print(result)  # Output: ConcreteHandler2: Handled Handler2
    ```

- Command Pattern
  - Definition: Turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as arguments, delay or queue a request's
    ```python
    from abc import ABC, abstractmethod

    class Command(ABC):
        @abstractmethod
        def execute(self):
            pass

    class Receiver:
        def action(self):
            return "Receiver: executing action."

    class ConcreteCommand(Command):
        def __init__(self, receiver):
            self._receiver = receiver

        def execute(self):
            return self._receiver.action()

    class Invoker:
        def __init__(self):
            self._command = None

        def set_command(self, command):
            self._command = command

        def execute_command(self):
            return self._command.execute()

    receiver = Receiver()
    command = ConcreteCommand(receiver)
    invoker = Invoker()

    invoker.set_command(command)
    result = invoker.execute_command()
    print(result)  # Output: Receiver: executing action.
    ```
- State Pattern
  - Definition: Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
    Example:
    ```python
    from abc import ABC, abstractmethod

    class Context:
        _state = None

        def __init__(self, state):
            self.transition_to(state)

        def transition_to(self, state):
            print(f"Context: Transition to {type(state).__name__}")
            self._state = state
            self._state.context = self

        def request(self):
            self._state.handle()

    class State(ABC):
        @property
        def context(self):
            return self._context

        @context.setter
        def context(self, context):
            self._context = context

        @abstractmethod
        def handle(self):
            pass

    class ConcreteStateA(State):
        def handle(self):
            print("ConcreteStateA: Handle request")

    class ConcreteStateB(State):
        def handle(self):
            print("ConcreteStateB: Handle request")

    context = Context(ConcreteStateA())
    context.request()  # Output: Context: Transition to ConcreteStateA, ConcreteStateA: Handle request

    context.transition_to(ConcreteStateB())
    context.request()  # Output: Context: Transition to ConcreteStateB, ConcreteStateB: Handle request
    ```

- Template Method Pattern
  - Definition: Defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.
    Example:
    ```python
    from abc import ABC, abstractmethod

    class AbstractClass(ABC):
        def template_method(self):
            result = []
            result.append(self.base_operation1())
            result.append(self.required_operations1())
            result.append(self.base_operation2())
            result.append(self.hook1())
            if self.hook2():
                result.append(self.required_operations2())
            return "\n".join(result)

        def base_operation1(self):
            return "AbstractClass: base operation1"

        @abstractmethod
        def required_operations1(self):
            pass

        def base_operation2(self):
            return "AbstractClass: base operation2"

        def hook1(self):
            return "AbstractClass: hook1"

        def hook2(self):
            return True

        @abstractmethod
        def required_operations2(self):
            pass

    class ConcreteClass1(AbstractClass):
        def required_operations1(self):
            return "ConcreteClass1: implementing required operation1"

        def required_operations2(self):
            return "ConcreteClass1: implementing required operation2"

    class ConcreteClass2(AbstractClass):
        def required_operations1(self):
            return "ConcreteClass2: implementing required operation1"

        def hook2(self):
            return False

        def required_operations2(self):
            return "ConcreteClass2: implementing required operation2"

    def client_code(abstract_class):
        print(abstract_class.template_method())

    concrete_class1 = ConcreteClass1()
    print("Client: ConcreteClass1 calls the template method:")
    client_code(concrete_class1)

    concrete_class2 = ConcreteClass2()
    print("\nClient: ConcreteClass2 calls the template method:")
    client_code(concrete_class2)
    ```

## Best Practices and Considerations
  - When to Apply SOLID Principles
  - Choosing the Right Design Pattern
  - Trade-offs and Caveats
  - Refactoring Legacy Code
  - Testing Strategies

## Resources
  - Books on SOLID Principles and Design Patterns
  - Online Courses and Tutorials
  - Design Pattern Catalogs
  - Code Examples and Implementations
  - Community Forums and Discussions

## Conclusion
  - Practice Implementing SOLID Principles and Design Patterns
  - Continuously Refactor and Improve Code
  - Share Knowledge and Learn from Others
