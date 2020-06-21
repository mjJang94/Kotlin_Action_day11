# 동반 객체를 일반 객체처럼 사용

동반 객체란 클래스 안에 정의된 일반 객체이다.   
회사의 급여 명부를 제공하는 웹 서비스를 만든다고 가정했을때, 서비스에서 사용하기 위해 객체를 JSON형식으로 직렬화하거나 역직렬화를 해야한다.   
직렬화 로직을 동반 객체 안에 넣어보자.

<code>
<pre>
class Person(val name: String) {
  companion object Loader {  //----동반 객체에 이름을 붙여줌
     fun fromJSON(jsonText: String) : Person = ...
     }
  }
  
>>>person = Person.Loader.fromJSON("{name: 'Dimitry'}")
>>>person.name
Dimitry

</code>
</pre>

대부분의 경우 클래스 이름을 통해 동반 객체에 속한 멤버를 참조할 수 있으므로 객체의 이름은 필요없다. 하지만 필요로 한다면 위 코드처럼 동반 객체에 이름을 붙이도록 하자. 특별히 이름을 지정하지 않으면 동반 객체 이름은 자동으로 Companion이라는 이름을 가지게 된다.   

# 동반 객체에서 인터페이스 구현

다른 객체 선언과 마찬가지로 동반 객체도 인터페이스를 구현할 수 있는데, 인터페이스를 구현하는 동반 객체를 참조할 때 객체를 둘러싼 클래스의 이름을 바로 사용할 수 있다.   
이번에는 모든 객체를 역직렬화를 통해 만들어야 하므로 모든 타입의 객체를 생성하는 일반적인 방법과 JSON을 역직렬화하는 JSONFactory 인터페이스가 존재한다.

<code>
<pre>
interface JSONFactory<T> {
  fun fromJSON(jsonText: String) : T
  }
  
class Person(val name: String) {
  companion object: JSONFactory<Person> {
    override fun fromJSON(jsonText: sTring) : Person = ...
  }
}
</code>
</pre>

이제 JSON으로부터 각 원소를 다시 만들어내는 추상 팩토리가 있다면 Person객체를 그 팩토리에 넘길 수 있다.

<code>
<pre>
fun loadFromJSON<T>(factory: JSONFactory<T>) : T {
  ...
}

loadFromJSON(Person) //----동반 객체의 인스턴스를 함수에 넘긴다.
</code>
</pre>
