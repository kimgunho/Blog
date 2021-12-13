---
emoji: ✍️
title: Infinity scroll In React
date: '2021-10-14 15:00:00'
author: me
tags: React IntersectionObserver scroll Lazyload
categories: React
---

포켓몬 도감을 만들어보면서 포켓몬 리스트를 무한 스크롤 형식으로 구현해보기로 했었다.
이 기록은 그 당시 구현한 스크롤을 잊지않기위해 복습하기 위함이다. 퍼블리셔로 근무를 할 당시에는 제이쿼리를 이용해 scroll 함수 또는 addEventListener 함수에서 스크롤을 사용하는 방법등을 사용하여 스크롤 관련 이벤트를 구현하였지만
구글링을 통해 좀더 알아보니 이방법은 스크롤할때마다 발생되는 이벤트로 인하여 성능적으로 좋지않는다고 한다.
리엑트를 공부해보면서 그말에 더욱 큰 공감이 간다.
그래서 훨씬 나은 **IntersectionObserver**와 **useRef**를 사용하여 무한 스크롤을 만들어준다.
그래서 IntersectionObserver, useRef가 도대체 무엇인가?

## IntersectionObserver & useRef

개인적인 느낌으로는 뷰포트안에 카메라를 하나 만들어 그 카메라는 뷰포트안에 지정된 특정 태그가
노출될 시 작동되는 느낌으로 받아드렸다. 코드를 확인해보자

```
import React, { useEffect, useState, useRef } from 'react'

import { UseUserPokemons } from '../../context/pokemonContext'

function List() {
  const { pokemons, setPokemons, count, setCount } = UseUserPokemons()
  const [isLoading, setIsLoading] = useState(true)

  const fetchPokemons = async (count) => {
    const res = await fetch(
      //
      `https://pokeapi.co/api/v2/pokemon?limit=${count}`,
    )
    const json = await res.json()
    const { results } = json

    const pokemonItem = await Promise.all(
      results.map(async ({ url }) => {
        const pokemonRes = await fetch(url)
        const pokemonJson = await pokemonRes.json()
        const detailUrl = pokemonJson.species.url

        const detailRes = await fetch(detailUrl)
        const detailJson = await detailRes.json()

        return {
          id: pokemonJson.id,
          name: detailJson.names[2].name,
          img: pokemonJson.sprites.other['official-artwork'].front_default,
          type: pokemonJson.types[0].type.name,
          color: detailJson.color.name,
          text: detailJson.flavor_text_entries[23].flavor_text,
          genera: detailJson.genera[1].genus,
          height: pokemonJson.height,
          weight: pokemonJson.weight,
        }
      }),
    )

    setPokemons(pokemonItem)
    setIsLoading(false)
  }

  const loadMore = () => {
    setCount((acc) => acc + 9)
  }

  useEffect(() => fetchPokemons(count), [count])

  const conutEnd = useRef()
  useEffect(() => {
    if (!isLoading) {
      const observer = new IntersectionObserver(
        (entrise) => {
          if (entrise[0].isIntersecting) {
            loadMore()
          }
        },
        { threshold: 1 },
      )
      observer.observe(conutEnd.current)
    }
  }, [isLoading])

  return (
    <div>
			<Pokemons more={loadMore} end={conutEnd} item={pokemons} />
    </div>
  )
}

export default List
```

코드가 길어서 보기 힘들어보이지만

하나하나씩 흩어보자.

우선 위 코드의 목적은 포켓몬 api를 이용하여 포켓몬의 데이터를 불러와 도감을 만드는것이다.

도감을 만드는 과정에서 포켓몬의 리스트의 배열에 데이터를 담아오며

useRef를 사용하여 화면의 끝에 다다른 특정위치값을

```
const conutEnd = useRef()
```

위의 코드로 설정해주었다.

**이후 핵심 부분이다**

```
const loadMore = () => {
    setCount((acc) => acc + 9)
  }

  const conutEnd = useRef() // 뷰포트에서 특정영역을 감지할 부분 설정
  useEffect(() => {
    if (!isLoading) { // 만약 로딩이 false라면
      const observer = new IntersectionObserver( // observer = new intersectionObserver를 이용할것이다.
        (entrise) => { // 매개변수 entrise 속성을 불러와
          if (entrise[0].isIntersecting) { // 만약 entrise[0].isIntersecting값이 true라면
            loadMore() // 해당 함수실행
          }
        },
        { threshold: 1 }, // 영역의 %를 뜻하는 프로퍼티이다 범위 설정 0 ~ 1까지 존재한다.
      )
      observer.observe(conutEnd.current) // observer실행시 conutEnd의 현재영역이 보일시
    }
  }, [isLoading]) // useEffect에서 초기 로딩이 변경될때 실행 (로딩은 처음 true이지만 데이터가 출력됨으로서 false로 변경된다.)
```

위 주석의 표기한 부분처럼 실행로직이 작동된다.

먼가 복잡해보이지만 어렵게 생각할 필요없다.

useRef를 사용하여 영역을 감지할 위치를 지정한 다음

intersectionObserver에서 entrise의 속성을 (마치 function(e) 의 고유매개와같이 이해)

useRef에 빗대어 영역을 감지한 후 속성에서 boolean값으로 데이터를 실행할지 안할지

결정한다.
