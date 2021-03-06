# kotlin-patterns
Examples of patterns in Kotlin inpired by [ActionScript 3.0 Design Patterns](https://www.amazon.com/ActionScript-3-0-Design-Patterns-Programming/dp/0596528469) book.

  Kotlin Design Patterns
  1. Factory
  2. Singleton
  3. Decorator
  4. Adapter
  5. Composite
  6. Command
  7. Observer
  8. State
  9. Strategy
  
## Factory

 ```
    class Factory {

        fun createProduct1(): Product {
            return  Product1()
        }

        fun  createProduct2(): Product {
            return  Product2()
        }
    }
  
    interface Product {
        fun manipulate()
    }
    
    class Product1 : Product {

        override fun manipulate() {
            println("factory Product 1")
        }
    }
    
    class Product2 : Product {
        override fun manipulate() {
            println("factory Product 2")
        }
    }
    
    //example of use
    var factory:Factory= Factory()
    var product1:Product1= factory.createProduct1() as Product1
    var product2:Product2= factory.createProduct2() as Product2

    product1.manipulate()
    product2.manipulate()
 ```
## Singleton

  ```
    class Singleton {

        companion object{
            val instance = Singleton()
        }
        lateinit var message:String
    }
    
    //example of use
    var singleton:Singleton= Singleton.instance
    singleton.message= "Hello Singleton"

    var nSingleton:Singleton= Singleton.instance

    println("singleton "+singleton)
    println("singleton message "+singleton.message)
    println("nSingleton message "+nSingleton.message)
  ```

## Decorator

  ```
    open class Decorator(val c:Car):Car{
        var car:Car

        init {
            car=c
        }

        override fun assemble() {
            car.assemble()
        }
    }
    
    class Adapter :Target {

        var adaptee:Adaptee

        init {
            adaptee= Adaptee()
        }


        override fun request() {
            adaptee.specificRequest()
        }

    }

    interface Target {
        fun request()
    }
    //example of use
    var target:Target= Adapter()
    target.request()
  ```
  
## Composite
  ```
    class Composite(s:String): Component() {

      var sName:String
      var aChildren:MutableList<Component>

      init {
          this.sName= s
          aChildren= mutableListOf<Component>()
      }

      override fun add(c: Component) {
          super.add(c)
          aChildren.add(c)
      }

      override fun remove(c: Component) {
          super.remove(c)
          aChildren.remove(c)
      }

      override fun operation() {
          super.operation()
          println(this.sName)
          aChildren.forEach{
              it.operation()
          }
      }

  }

  class Leaf(s:String):Component(){

      var sName:String

      init {
          this.sName=s
      }

      override fun operation() {
          super.operation()

          println(this.sName)
      }
  }

  open class Component{

      open fun add(c:Component){
      }

      open fun remove(c:Component){
      }

      open fun getChild(n:Int):Component{
          throw UnsupportedOperationException("getChild method not implemented") //To change body of created functions use File | Settings | File Templates.
      }

      open fun operation(){ }

  }
    //example of use
    var composite:Composite = Composite("root")

    var n1:Composite= Composite("composite 1")
    n1.add(Leaf("Leaf 1"))
    n1.add(Leaf("Leaf 2"))

    var n2:Composite= Composite("composite 2")
    n2.add(Leaf("Leaf 3"))
    n2.add(Leaf("Leaf 4"))
    n2.add(Leaf("Leaf 5"))

    composite.add(n1)
    composite.add(n2)
    composite.operation()
  ```
  
## Command
  ```
     class Command(r: Receiver):BaseCommand {

        var receiver:Receiver
        init {
            receiver=r
        }
        override fun execute() {
            receiver.action()
        }

    }
    interface BaseCommand {
        fun execute()
    }

    class Receiver{
        fun action(){
            println("Receiver : doing action")
        }
    }

    class Invoker{
        lateinit  var currentCommand:BaseCommand

        fun setCommand(c:BaseCommand){
            currentCommand= c
        }

        fun executeCommand(){
            currentCommand?.execute()
        }
    }
    //example of use
    var receiver:Receiver = Receiver()
    var command:BaseCommand= Command(receiver)

    var invoker:Invoker= Invoker()
    invoker.setCommand(command)
    invoker.executeCommand()
  ```
  
## State
  ```
    interface State{

        fun startPlay()
        fun stopPlay()
    }

    class PlayState:State{
        override fun startPlay() {
            println("You're already playing...")
        }

        override fun stopPlay() {
            println("Go to the stop state.")
        }

    }

    class StopState:State{
        override fun startPlay() {
            println("Go to the Play state.")
        }

        override fun stopPlay() {
            println("You're already stopped")
        }
    }

    class VideoContext():State{

        lateinit var playState:State
        lateinit var stopState:State
        lateinit var state:State

        init {
            println("Video is On")
            playState= PlayState()
            stopState= StopState()
            state= stopState
        }
        override fun startPlay() {
            state.startPlay()
        }

        override fun stopPlay() {
            state.stopPlay()
        }

    }
  ```
