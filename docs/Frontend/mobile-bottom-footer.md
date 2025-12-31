---
layout: default
title: 모바일 하단 네비게이션 대응
parent: Frontend
nav_order: 1
---

# 모바일 주소창, 하단네비게이션 대응


모달창이나 하단에서 올라오는 시트를 개발할 때 `100vh`를 자주 사용합니다.  
하지만 모바일 브라우저의 주소창이나 하단 네비게이션 바로 인해 화면 일부가 가려지는 문제가 발생합니다.

## 1. 원인
`vh` 단위는 초기 브라우저 뷰포트 값을 기준으로 계산되지만, 모바일의 하단 네비게이션은 모바일 브라우저 마다 다르게 동적으로 나타나거나 사라지며 모바일 브라우저 마다 다르게 동작할 수 있습니다. 이로 인해 `100vh`로 설정해도 하단 네비게이션에 의해 화면 일부가 가려지는 문제가 발생합니다.

### **<span style="color: gray">참고 이미지</span>**
<div style="display: flex; gap: 10px;">
  <img src="{{site.url}}/assets/imgs/bottom_nav_1.jpg" style="width: 50%;" />
  <img src="{{site.url}}/assets/imgs/bottom_nav_2.jpg" style="width: 50%;" />
</div>
하단에 아이콘이나 계속해서 노출되어야 하는 컨텐츠가 있는 경우 `vh` 단위를 사용할 경우   
해당 하단 네비게이션에 의해 가려지는 문제가 발생할 수 있습니다.


## 2. 해결 방법
하단 네비게이션이 나타나거나 사라질 때 마다 resize 이벤트가 발생하여 
해당 이벤트를 이용하여 `vh` 값을 동적으로 변경하여 해결할 수 있습니다.

### 리액트 적용 예시
{% raw %}
```javascript
const [height, setHeight] = useState(0);

const handleSize = () => {
  setHeight(window.innerHeight);
};

useEffect(() => {
  handleSize();
  window.addEventListener('resize', handleSize);
  return () => {
    window.removeEventListener('resize', handleSize);
  };
}, []);

return (
  <div style={{ ['--vh' as string]: `${height * 0.01}px` }}>
    ...
  </div>
);
```
{% endraw %}

### 커스텀 훅으로 공통화 (`useViewportSize`)
매번 `resize` 이벤트가 발생할 때마다 리렌더링되는 것을 방지하기 위해 디바운서(Debouncer)를 추가한 훅입니다.

```javascript
import { useEffect, useState, useRef } from 'react';

interface ViewportSize {
  width: number;
  height: number;
}

function useViewportSize(): ViewportSize {
  const [viewport, setViewportSize] = useState<ViewportSize>({
    width: 0,
    height: 0,
  });

  let timer = useRef<NodeJS.Timeout>();

  const sizeDebouncer = () => {
    if (timer.current) clearTimeout(timer.current);
    timer.current = setTimeout(handleSize, 1000);
  };

  const handleSize = () => {
    setViewportSize({
      width: window.innerWidth,
      height: window.innerHeight,
    });
  };

  useEffect(() => {
    handleSize();
    window.addEventListener('resize', sizeDebouncer);
    return () => {
      window.removeEventListener('resize', sizeDebouncer);
    };
  }, []);

  return viewport;
}

export default useViewportSize;
```

### 사용 방법
```javascript
const { width, height } = useViewportSize();
```
필요한 컴포넌트에서 위와 같이 불러와 사용하면 됩니다.