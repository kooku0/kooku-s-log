---
title: stack 2개로 queue 만들기
date: 2019-12-20 04:12:31
category: data structure
---

stack 2개로 queue를 구현하는 것은 면접 단골 질문이다.

나도 처음에는 어떻게 할지 고민했는데, 조금만 생각해보면 정말 간단히 만들 수가 있다.

```c++
class Queue{
    private stack<int> inBox;
    private stack<int> outBox;
    
    public void enQueue(int a){
        inBox.push(a);
	}
    public int deQueue(){
        if (!outBox.empty()){
            return outBox.pop();
		} else {
            while(!inBox.empty()) {
            	int tmp = inBox.pop();
                outBox.push(tmp);
			}
            return outBox.pop();
		}
	}
}
```

진짜 완벽!!!

이렇게 코드를 짜게되면 deQueue가 호출하기 전까지는 inQueue에 계속 넣게 되고, deQueue가 호출되면 inBox가 빌때까지 outBox로 모두 넘겨주게 된다. 그러면 순서가 거꾸로 되기 때문에 queue처럼 만들 수 있다.

여기서 중요한 것은 deQueue가 한 번 호출된 후 다시 outBox에서 inBox로 값을 넘겨줄 필요가 없다는 것이다. 한번 정렬이 되었기 때문에 outBox에 있는 값을 전부 pop한 뒤에 다시 넣어주면 된다.