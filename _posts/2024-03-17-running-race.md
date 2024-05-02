---
title: 달리기 경주
author: 753
date: 2024-03-17 17:30:00 +0800
categories: [코딩테스트, Lv0]
tags: [코딩테스트, 자바스크립트, 프로그래머스, Lv0]
---

## 출처

-   프로그래머스 > 코딩테스트 연습 > 연습문제 > 달리기 경주
-   https://school.programmers.co.kr/learn/courses/30/lessons/178871

## 문제 설명

얀에서는 매년 달리기 경주가 열립니다. 해설진들은 선수들이 자기 바로 앞의 선수를 추월할 때 추월한 선수의 이름을 부릅니다. 예를 들어 1등부터 3등까지 "mumu", "soe", "poe" 선수들이 순서대로 달리고 있을 때, 해설진이 "soe"선수를 불렀다면 2등인 "soe" 선수가 1등인 "mumu" 선수를 추월했다는 것입니다. 즉 "soe" 선수가 1등, "mumu" 선수가 2등으로 바뀝니다.

선수들의 이름이 1등부터 현재 등수 순서대로 담긴 문자열 배열 players와 해설진이 부른 이름을 담은 문자열 배열 callings가 매개변수로 주어질 때, 경주가 끝났을 때 선수들의 이름을 1등부터 등수 순서대로 배열에 담아 return 하는 solution 함수를 완성해주세요.

## 제한사항

-   5 ≤ players의 길이 ≤ 50,000
    -   players[i]는 i번째 선수의 이름을 의미합니다.
    -   players의 원소들은 알파벳 소문자로만 이루어져 있습니다.
    -   players에는 중복된 값이 들어가 있지 않습니다.
    -   3 ≤ players[i]의 길이 ≤ 10
-   2 ≤ callings의 길이 ≤ 1,000,000
    -   callings는 players의 원소들로만 이루어져 있습니다.
    -   경주 진행중 1등인 선수의 이름은 불리지 않습니다.

## 입출력 예

| players                               | callings                       | result                                |
| ------------------------------------- | ------------------------------ | ------------------------------------- |
| ["mumu", "soe", "poe", "kai", "mine"] | ["kai", "kai", "mine", "mine"] | ["mumu", "kai", "mine", "soe", "poe"] |

## 입출력 예 설명

#### 입출력 예 #1

4등인 "kai" 선수가 2번 추월하여 2등이 되고 앞서 3등, 2등인 "poe", "soe" 선수는 4등, 3등이 됩니다. 5등인 "mine" 선수가 2번 추월하여 4등, 3등인 "poe", "soe" 선수가 5등, 4등이 되고 경주가 끝납니다. 1등부터 배열에 담으면 ["mumu", "kai", "mine", "soe", "poe"]이 됩니다.

## 코드 1

```javascript
function solution(players, callings) {
    for (const call of callings) {
        // console.log(call)
        const index = players.indexOf(call);
        [players[index - 1], players[index]] = [
            players[index],
            players[index - 1]
        ];
        // console.log(players)
    }

    return players;
}
```

## 결과

<span style="color:blue">
테스트 1 〉 통과 (0.08ms, 33.5MB)<br>
테스트 2 〉 통과 (0.06ms, 33.4MB)<br>
테스트 3 〉 통과 (0.22ms, 33.5MB)<br>
테스트 4 〉 통과 (0.65ms, 33.7MB)<br>
테스트 5 〉 통과 (8.59ms, 37.9MB)<br>
테스트 6 〉 통과 (17.87ms, 38.2MB)<br>
테스트 7 〉 통과 (365.90ms, 41.6MB)<br>
테스트 8 〉 통과 (1512.10ms, 47.2MB)<br>
테스트 9 〉 통과 (5945.33ms, 52.3MB)<br>
</span>
<span style="color:red">
테스트 10 〉 실패 (시간 초과)<br>
테스트 11 〉 실패 (시간 초과)<br>
테스트 12 〉 실패 (시간 초과)<br>
테스트 13 〉 실패 (시간 초과)<br>
</span>
<span style="color:blue">
테스트 14 〉 통과 (0.06ms, 33.4MB)<br>
테스트 15 〉 통과 (0.05ms, 33.5MB)<br>
테스트 16 〉 통과 (0.08ms, 33.4MB)<br>
</span>