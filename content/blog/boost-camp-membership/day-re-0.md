# [부스트캠프 멤버십 다시 0일차]

## 오늘의 고민들
### React에서 어떻게 하나의 변화에 대해, 연쇄적으로 다른 변화를 이르킬 수 있을까

Redux, context 등을 고려하지 않고, 순수하게 React의 사고 방식으로 부모에서 자녀로 props를 내리는 형태로 가정하자. 

만약, 부모의 객체에서 어떤 이벤트가(여기서는 클릭) 발생하여 그로 인해 부모와 자식에 존재하는 값이 동시에 변해야 한다면 어떻게 해볼 수 있을까?

가장 쉬운 방법은, 자식에 있는 값은 부모로 올리는 것이다. 
다음 예제에서는, 부모의 `초기화` 버튼을 누르면, 그에 따라 부모에 있는 값과(totalCount) 자식에 있는 값(count)이 동시에 초기화(0으로 설정)이 된다. 

```js
// 부모
const App = () => {
  const [totalCount, setTotalCount] = useState(0);
  const [count, setCount] = useState(0);

  const clearCount = () => {
    setTotalCount(0);
    setCount(0);
  };

  return (
    <div className="App">
      <Counter
        count={count}
        setCount={setCount}
      />
      <button onClick={clearCount}>초기화!</button>
    </div>
  )
}

```

가능은 하겠지만, 만약 자식이 여러개가 생기고 그 값을 모두 부모가 들고 있어야 한다고 생각해보자.. 힘들어 진다. 

물론 무조건 잘못된 방법은 아니지만, 이 경우에 각각의 count에 대한 값은 Counter가 들고 있는 형태가 더 적절할것 같다. 그리하여, Counter 스스로도 독립 가능한 컴포넌트로서의 역할을 할 수 있도록 하면 좋을듯 하다. 


두번째로 할 수 있는 방식은, 변화에 대한 기준값을 자식에 제공하는 것이다. 
React의 life cycle 혹은 hooks를 잘 활용하면, 변화에 대한 어떤 기준점에 대해서 특정 행동을 취할 수 있다. 
이러한 방식이 조금 더 React 스러운 방식같다. 

```js
// 부모
const App = () => {
  const [totalCount, setTotalCount] = useState(0);

  const clearCount = () => {
    setTotalCount(0);
  };

  return (
    <div className="App">
      <Counter
        clear={totalCount === 0 ? true : false}
      />
      <button onClick={clearCount}>초기화!</button>
    </div>
  )
}

// 자식
const Counter = ({clear}) => {
  const [count, setCount] = useState(0);

  // 부모로부터 내려오는 clear라는 props의 변화에 따라 useEffect를 실행한다. 
  // 만약, clear가 되어 totalCount가 0 이 되었다면, 이 컴포넌트 또한 0으로 설정한다. 
  useEffect(() => {
    if (clear) {
      setCount(0);
    }
  }, [clear]);

  return (
    <div>
      {count}
    </div>
  )
}
```

위에 작성 되었듯이, useEffect의 두번째 인자로 관찰 하고자 하는 인자를 제공하여 그 값에 변화가 생길때 useEffect를 실행하고자 하였다. 
이렇게 된다면, 상위의 어떤 이벤트가 발생하는지는 모르지만 그 결과로 어떤값(totalCount)이 0 이 된다면, 이 컴포넌트 또한 업데이트를 진행한다. 


## 오늘의 회고

근래에 여러 일들이 곂치고, 나름 바쁜 일들이 있어서 회고를 진행하지 못하였다. 
계속하여 시간을 내지 못한 나의 모습을 반성하고, 오늘부터 다시 회고를 시작하고자 한다. 

나의 성장을 확인하고, 돌아보기 위해서는 회고는 필수적인것 같다. 

나와의 약속을 먼저 지킬 수 있는 사람이 되도록 하자. 